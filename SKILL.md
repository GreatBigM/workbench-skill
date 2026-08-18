---
name: workbench-workflow
description: 项目任务管理体系：change 四要素生命周期、闭环验收、spec 维护，任务全程可控。
version: 3.1.0
category: knowledge
metadata:
  agent:
    triggers: [workbench, change, 新建任务, 闭环, 验收, 归档, spec, design吸收, 四要素]
---

# 项目任务管理（workbench）

> 合并自 openspec-workflow（2026-08-05）：执行防护（对话分离 + 失败熔断）+ 规范先行/颗粒拆细。

> 宪法（做成什么样）：`SCHEMA.md`——目录结构/四要素边界/拒绝态/流转规则/知识沉淀衔接/项目级 spec 维护/design 吸收
> 2026-07-24 确立。任务体系跟代码走，知识产出留在知识库。

## ⚡ 使用方式（AI 替你完成）

**本技能的使用方式是：用户指挥 AI，AI 替用户执行。用户不面对命令行。**

```
用户: 新建一个 change 做 WiFi 驱动优化 / 验收昨天的 change / 项目现在什么状态
AI:  加载本 skill → 识别操作（新建/修改/闭环/查阅）→ 按 SCHEMA.md 定位 0_workbench/ 结构
AI:  对话层向用户询问缺失信息（change 名称/目标/验收标准）
用户: 提供信息
AI:  按四要素模板创建/更新文档 → 回报结果
```

**铁律：用户给出任务意图 → AI 按四要素规范直接执行，不反问"你要不要先看 spec"。**
信息缺失 → AI 对话层引导用户补齐，拿齐就干。

> **AI 交互约定（agent 必读）**：
> - 操作识别：新建 change / 修改 change / 闭环（验收→吸收 design→更新 spec→归档）/ 查阅状态
> - 信息引导：缺 change 名称/目标/验收标准 → 对话层询问，按 `templates/*.md` 四要素填充
> - 交互式设定优先：路径/名称等参数 AI 问清后写入文档，不让用户手动编辑
> - 执行回报：创建/更新的文件 + 状态（施工中/已闭环）

## 触发条件

- 新建/修改 change
- change 闭环（验收 → 吸收 design → 更新 spec → 移至 archive）
- 写 references 产出
- 查阅项目当前状态（spec.md）
- 创建新项目的 workbench

## 操作导航

| 输入 | 操作 | 依据 |
|------|------|------|
| 新建 change | 四要素模板创建 | `templates/*.md` + SCHEMA.md §二 |
| 修改 change | 更新四要素 | SCHEMA.md §二 |
| 闭环 change | 验收→吸收→更新 spec→归档 | SCHEMA.md §三 |
| 查阅状态 | 读 spec.md | SCHEMA.md §五 |
| 触发知识沉淀（产出物入知识库） | YYYYMMDD 命名 + 两态 | SCHEMA.md §四 |

> 完整规范（做成什么样）见 `SCHEMA.md`：目录结构/四要素边界约定/拒绝态/流转规则/知识沉淀衔接/项目级 spec.md 维护/design 吸收原则。SKILL.md 只承载操作流程。

## change 四要素（速查）

spec（规格：成功标准+约束）· design（设计：怎么改）· tasks（任务）· check（验收）——四文件同目录 `change/<name>/`，模板 `templates/*.md`；边界约定（标准由 spec 定 / 方法由 check 给 / 结果落 tasks）见 `SCHEMA.md` §二。

### check.md 写法

必须可测试、定量。反例：「功能正常」。正例：

```
- iperf3 TCP RX ≥ 80Mbps（原基线 52Mbps）
- 5 次循环重启无 crash
- memcpy 占比 ≤ 5%
- wpa_state=COMPLETED 重连时间 ≤ 3s
```

## 新建 change 流程

1. 在 `0_workbench/change/` 下创建 `<change_name>/` 目录
2. 依次写 spec.md → design.md → check.md，tasks.md 边做边填（从 `templates/` 复制对应模板起步）
3. 过程中产生的分析报告写入知识库 references/
4. 四要素缺一不可——验收标准必须在开工前就写好

## change 闭环流程

1. 对照 check.md 逐条确认通过
2. 提取 design.md 关键设计决策，合并到 design/ 对应文档
3. 判断 spec.md 中哪些成为持久约束 → 更新项目级 spec.md + changelog
4. 整个 `<change_name>/` 目录移至 archive/
5. git commit（项目仓）

> 归档检查清单：① design 要点已合并进 design/ ② 项目级 spec.md 已复审并按需更新 ③ 产出物已落知识库。三项全绿才算归档完成（SCHEMA.md §三）。

## 执行防护（对话分离 + 失败熔断）

### 失败熔断（铁律）

**同一任务连续失败 3 次，立即停下，向用户报告。** 不允许第 4 次自动重试。

- 适用范围：编译、烧录、日志采集、编码、分析等操作
- 报告内容：①已尝试的 3 次各是什么（参数/方法/命令）②每次失败的具体现象（错误信息/串口输出/exit code）③判断（参数错误？环境问题？方法不可行？）④下一步建议（需用户决策的选项）
- **为什么必须有熔断**：没有熔断会陷入"失败→换参数→再失败"循环，消耗大量 token 无法收敛。设备半死/串口无输出/网络不通需要人工介入（断电/连线/配网），自动重试无法解决。3 次足够覆盖"参数错误修正"，超出说明问题不在参数层面
- **熔断后恢复**：用户介入处理后告知"可以继续"，从断点继续，失败计数清零
- **参数错误主动检测**：错误参数（波特率/路径/IP）时进程常持续运行不报错（能 write 但 read 乱码/空），不能被动等超时——执行后主动检查（烧录类 `fuser /dev/ttyUSB0` + `ps aux | grep auto-uboot`；编译类 `ps aux | grep make` + 查日志），发现错误先 `kill -9` 释放资源（串口/终端）再重试

### 对话分离模式

将"无限长的模糊上下文"转化为"有限、确定的执行单元"。核心原理：**两个对话之间只传文件（工件），不传历史（聊天记录）。**

```
规划会话              解耦期              执行会话（新对话）      反馈期（规划会话）
┌──────────────┐    ┌──────────┐    ┌──────────────┐    ┌──────────────┐
│ 分析 + 设计   │ → │ 产出物化  │ → │ 只读 tasks    │ → │ 读阻塞原因    │
│ 冻结工件      │    │ design   │    │ 逐条勾选执行   │    │ 标注原因      │
│ >3模块拆      │    │ tasks    │    │ 遇阻塞→退出   │    │ 修正设计      │
│ 提示退出      │    │ constr.  │    │ 严禁改设计文档  │    │ 产出 v2       │
└──────────────┘    └──────────┘    └──────────────┘    └──────────────┘
```

- 规划会话：讨论定稿，产出 spec/design/tasks/check 工件（本 skill 四要素）
- 解耦期：工件是唯一跨对话载体
- 执行会话：新对话只读 tasks 逐条执行，遇阻塞立即退出（不自行改设计），严禁执行中改设计文档
- 反馈期：阻塞原因带回规划会话，修正设计产出 v2，再开新对话

### 规范先行 + 颗粒拆细

- **规范先行**：崩溃/失败发生时，先分析根因（只读）→ 更新设计文档（标记废弃 + 回退方案）→ 按更新后 tasks 执行修复。不能先改代码后补规范——中途被打断则规范误导后续工作
- **颗粒拆细**：分析（只读）→ 执行（只改）→ 编译 → 验证，不混在一个任务里。单任务目标 ≤ 3

## 相关文档

- `SCHEMA.md` — 宪法（做成什么样：目录结构/四要素边界/拒绝态/流转/产出规范/spec 维护/design 吸收）
- `references/troubleshooting.md` — 反模式（坑，按需查）

## 支持文件清单

本 skill 依赖以下模板与参考文件（安装时随 SKILL.md 一并打包，请勿删除）：

- 模板：`templates/spec.md`、`templates/design.md`、`templates/tasks.md`、`templates/check.md`
- 宪法：`SCHEMA.md`（与 SKILL.md 平级，随安装拷贝）
- 参考：`references/troubleshooting.md`
