# Changelog

本文件记录版本历史。版本号定义在 SKILL.md frontmatter 的 `version` 字段（单一真相源）。

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
