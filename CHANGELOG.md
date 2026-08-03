# Changelog

本文件记录版本历史。版本号定义在 SKILL.md frontmatter 的 `version` 字段（单一真相源）。

## 3.0.0 (2026-08-03)

### Changed（四要素命名体系重构）
- **goal.md → spec.md**：目标改规格——「标准由 spec 定」语义顺滑；SPEC=可更改规格身份统一（change 级 spec + 项目级 spec 两级，引用写全路径区分）
- **scheme.md → design.md**：方案改设计——与 design/ 目录流转对齐（design 要点合并进 design/）；消除 scheme/schema 与宪法 SCHEMA 的拼写混淆
- **templates/ 收敛四模板**：spec/design/tasks/check 与四要素一一对应；删除项目约束模板（项目级 spec 按宪法 §五 直接建）和 reference.md 模板（产出物格式见宪法 §四）
- **§四 重定位「知识沉淀衔接」**：workbench 面向任务段（四要素定义任务内工程流转），实践完成触发知识沉淀（产出物入知识库 references/），知识库侧机制由对应体系承担，本 skill 不规定（SKILL 不知道 SKILL 原则）
- 项目级 spec.md 全文档加限定（区分 change 级 spec / 项目级 spec）
- 存量 archive 快照（goal.md/scheme.md）不动，文件名不变原则
- 版本 2.0.0 → 3.0.0

## 2.0.0 (2026-08-03)

### Changed（参考分层 A 模式 + 概念平级对齐）
- **宪法提升根目录**：`references/workbench-spec.md` → 根目录 `SCHEMA.md`（与 SKILL.md 平级）——宪法不可替换，templates/references 是参考层
- **SKILL.md 瘦身**：只留操作流程（触发条件/操作导航/新建/闭环），做成什么样内容归 SCHEMA.md（单一真相源，消除双份定义）
- **反模式移入 `references/troubleshooting.md`**：坑按需查阅（A 模式）
- **qwiki 过时副本清理**：`~/qwiki/personal/workbench-spec.md`（旧版 workbench/ 命名）销毁，真相源唯一在 skill
- install.sh 拷贝清单 +1（SCHEMA.md）
- 版本 1.0.0 → 2.0.0

## 1.0.0 (2026-08-01)

### Added
- 初始发布：workbench 项目任务管理体系（change 四要素生命周期）
- 顶部新增「AI 替你完成」使用哲学头节：用户指挥 AI，AI 替用户执行
- triggers：workbench/change/新建任务/闭环/验收/归档/spec/design吸收/四要素
- templates 六件套：goal.md / scheme.md / tasks.md / check.md / spec.md / reference.md
- references/workbench-spec.md 完整规范
- 仓库根目录即 skill 本体布局，install.sh 一键安装
