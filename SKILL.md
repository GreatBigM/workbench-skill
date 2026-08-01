---
name: workbench-workflow
description: 项目任务管理体系 — 0_workbench/ 目录结构、change 四要素生命周期、references 产出规范、design 吸收、spec 维护
category: knowledge
metadata:
  hermes:
    triggers: [workbench, change, 新建任务, 闭环, 验收, 归档, spec, design吸收, 四要素]
---

# 项目任务管理（workbench）

> 完整规范：`references/workbench-spec.md`
> 2026-07-24 确立。任务体系跟代码走，知识产出留在知识库。

## ⚡ 使用方式（AI 替你完成）

**本技能的使用方式是：用户指挥 AI，AI 替用户执行。用户不面对命令行。**

```
用户: 新建一个 change 做 WiFi 驱动优化 / 验收昨天的 change / 项目现在什么状态
AI:  加载本 skill → 识别操作（新建/修改/闭环/查阅）→ 定位 0_workbench/ 结构
AI:  对话层向用户询问缺失信息（change 名称/目标/验收标准）
用户: 提供信息
AI:  按四要素模板创建/更新文档 → 回报结果
```

**铁律：用户给出任务意图 → AI 按四要素规范直接执行，不反问"你要不要先看 spec"。**
信息缺失 → AI 对话层引导用户补齐，拿齐就干。

> **AI 交互约定（agent 必读）**：
> - 操作识别：新建 change / 修改 change / 闭环（验收→吸收 design→更新 spec→归档）/ 查阅状态，映射到对应动作
> - 信息引导：缺 change 名称/目标/验收标准 → 对话层询问，按 `templates/*.md` 四要素填充
> - 交互式设定优先：路径/名称等参数 AI 问清后写入文档，不让用户手动编辑
> - 执行回报：创建/更新的文件 + 状态（施工中/已闭环）

## 触发条件

- 新建/修改 change
- change 闭环（验收 → 吸收 design → 更新 spec → 移至 archive）
- 写 references 产出
- 查阅项目当前状态（spec.md）
- 创建新项目的 workbench

## 目录结构

```
<项目代码仓>/0_workbench/
├── spec.md          ← 一张纸，硬约束（自带 changelog，单文件不可拆）
├── design/          ← 已吸收的架构设计（合并，不堆叠）
├── change/          ← 施工中
│   └── <change_name>/
│       ├── goal.md      ← 目标：改什么能力/行为
│       ├── scheme.md    ← 方案：怎么改（技术路径）
│       ├── tasks.md     ← 任务：做到哪（每步打勾）
│       └── check.md     ← 验收：怎么算完（可测试标准，不含糊）
└── archive/         ← 已闭环（扁平，不分成败子目录）
    └── <change_name>/   ← 同上四要素

<知识库>/projects/<项目>/references/
    YYYYMMDD-标题.md    ← 阶段产出（扁平，不分子目录）
```

> **命名约定**：`0_workbench` 而非 `_workbench`。`0_` 前缀在 VS Code 中排序紧跟 dotfiles 之后、业务代码之前，保持工作台可见且位置固定。`_` 虽也在 a-z 之前但会与 dotfiles 拉开距离（中间夹数字和大写字母）。

## change 四要素

| 文件 | 内容 | 要求 |
|------|------|------|
| goal.md | 改什么 | 一句话说清范围 |
| scheme.md | 怎么改 | 可评审的技术路径 |
| tasks.md | 做到哪 | 每步完成打勾，过时即废 |
| check.md | 怎么算完 | 可测试的定量标准 |

> 模板：`templates/goal.md`、`templates/scheme.md`、`templates/tasks.md`、`templates/check.md`——建 change 时从模板复制，填内容即可。

### check.md 写法

必须可测试、定量。反例：「功能正常」。正例：

```
- iperf3 TCP RX ≥ 80Mbps（原基线 52Mbps）
- 5 次循环重启无 crash
- memcpy 占比 ≤ 5%
- wpa_state=COMPLETED 重连时间 ≤ 3s
```

## 流转规则

```
change/ 施工中
    ↓ check.md 验收通过
archive/ 归档
    ├── scheme.md → 吸收进 design/（合并到现有文档，不堆文件）
    ├── goal.md   → 按需更新 spec.md（判断：是否成为持久约束）
    ├── tasks.md  → 随 change 归档
    ├── check.md  → 随 change 归档
    └── 分析/实验 → 写入知识库 references/
```

## references 产出规范

仅在 change 产生值得保留的分析/数据时写入。

> 模板：`templates/reference.md`——含 YAML 头骨架（标题/日期/来源/状态/吸收至/摘要）+ 数据表。

### 命名

```
YYYYMMDD-标题.md
```

示例：`20260724-rx-baseline.md`

### 文件头部 YAML

```markdown
---
标题: RX基线测量
日期: 2026-07-24
来源: wifi_driver_refactor
状态: 接受
吸收至: design/wifi_driver_design.md §3, spec.md §性能
摘要: iperf3 TCP RX 基准测试，52→82Mbps 优化路径验证
---
```

### 状态：仅两态

| 状态 | 含义 |
|------|------|
| 接受 | 结论已被吸收，可引用 |
| 拒绝 | 不采纳，此路不通，立警示 |

不留"待定"——产出即裁决。

## design 吸收原则

**合并而非堆叠**。更新现有设计文档对应章节，末尾标注来源：

```
> 来源: wifi_driver_refactor
```

design/ 始终是可读的当前架构，不是方案坟场。

## spec.md 维护

- 单文件，不可拆为文件夹
- 自带 changelog，不依赖 git log
- 每次 design 吸收方案后，按需更新约束
- 新建项目时从 `templates/spec.md` 复制初始化

### changelog 格式

```markdown
## 变更记录
2026-07-24 | wifi_driver_refactor | RX 天花板从 52Mbps → 82Mbps
2026-07-20 | partition_fix       | 约束分区大小，禁止手动 resize
```

每行：日期 | 触发 change | 改动内容。不依赖 git log。

## 新建 change 流程

1. 在 `0_workbench/change/` 下创建 `<change_name>/` 目录
2. 依次写 goal.md → scheme.md → check.md，tasks.md 边做边填（从 `templates/` 复制对应模板起步）
3. 过程中产生的分析报告写入知识库 references/
4. 四要素缺一不可——验收标准必须在开工前就写好

## change 闭环流程

1. 对照 check.md 逐条确认通过
2. 提取 scheme.md 关键设计决策，合并到 design/ 对应文档
3. 判断 goal.md 中哪些是持久约束 → 更新 spec.md + changelog
4. 整个 `<change_name>/` 目录移至 archive/
5. git commit（项目仓）

## 反模式

| ❌ | ✅ |
|----|-----|
| 验收写「功能正常」 | 验收写可测试的定量标准 |
| change 闭环不更新 spec | 判断功能是否成为约束，是则更新 |
| spec 做成文件夹塞子文档 | spec 永远是单文件 |
| 分析报告放 change 目录 | 放知识库 `references/`，按命名规范 |
| change 只有方案没有验收 | goal/scheme/tasks/check 四要素缺一不可 |
| archive 分 completed/abandoned 子目录 | 扁平，读 check.md 知成败 |
| design 吸收 = 把 scheme.md 拷过去 | 合并进现有文档对应章节 |
| references 用子目录按 change 组织 | 扁平，文件名 YYYYMMDD-标题.md |
| references 无头部元数据 | 必须含标题/日期/来源/状态/摘要 |
| spec.md 改完不记录 changelog | 每次改动追加一行 |

## 相关文档

- `references/workbench-spec.md` — 完整规范（含四要素边界约定、拒绝态、实施清单）

## 支持文件清单

本 skill 依赖以下模板与参考文件（安装时随 SKILL.md 一并打包，请勿删除）：

- 模板：`templates/goal.md`、`templates/scheme.md`、`templates/tasks.md`、`templates/check.md`、`templates/spec.md`、`templates/reference.md`
- 参考：`references/workbench-spec.md`
