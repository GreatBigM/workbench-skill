---
name: workbench-workflow
description: 项目任务管理体系 — change 四要素生命周期、闭环、spec 维护（宪法见 SCHEMA.md）
version: 2.0.0
category: knowledge
metadata:
  hermes:
    triggers: [workbench, change, 新建任务, 闭环, 验收, 归档, spec, design吸收, 四要素]
---

# 项目任务管理（workbench）

> 宪法（做成什么样）：`SCHEMA.md`——目录结构/四要素边界/拒绝态/流转规则/产出规范/spec 维护/design 吸收
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
| 写 references 产出 | YYYYMMDD 命名 + 两态 | SCHEMA.md §四 |

> 完整规范（做成什么样）见 `SCHEMA.md`：目录结构/四要素边界约定/拒绝态/流转规则/references 规范/spec.md 维护/design 吸收原则。SKILL.md 只承载操作流程。

## change 四要素（速查）

goal（目标）· scheme（方案）· tasks（任务）· check（验收）——四文件同目录 `change/<name>/`，模板 `templates/*.md`；边界约定（标准由 goal 定 / 方法由 check 给 / 结果落 tasks）见 `SCHEMA.md` §二。

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
2. 依次写 goal.md → scheme.md → check.md，tasks.md 边做边填（从 `templates/` 复制对应模板起步）
3. 过程中产生的分析报告写入知识库 references/
4. 四要素缺一不可——验收标准必须在开工前就写好

## change 闭环流程

1. 对照 check.md 逐条确认通过
2. 提取 scheme.md 关键设计决策，合并到 design/ 对应文档
3. 判断 goal.md 中哪些是持久约束 → 更新 spec.md + changelog
4. 整个 `<change_name>/` 目录移至 archive/
5. git commit（项目仓）

> 归档检查清单：① scheme 已吸收进 design ② spec 已复审并按需更新 ③ 产出物已落知识库。三项全绿才算归档完成（SCHEMA.md §三）。

## 相关文档

- `SCHEMA.md` — 宪法（做成什么样：目录结构/四要素边界/拒绝态/流转/产出规范/spec 维护/design 吸收）
- `references/troubleshooting.md` — 反模式（坑，按需查）

## 支持文件清单

本 skill 依赖以下模板与参考文件（安装时随 SKILL.md 一并打包，请勿删除）：

- 模板：`templates/goal.md`、`templates/scheme.md`、`templates/tasks.md`、`templates/check.md`、`templates/spec.md`、`templates/reference.md`
- 宪法：`SCHEMA.md`（与 SKILL.md 平级，随安装拷贝）
- 参考：`references/troubleshooting.md`
