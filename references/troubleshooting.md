# workbench 反模式（troubleshooting）

> 排障参考：按需查阅，非必读。主流程见 SKILL.md，规范见 SCHEMA.md。
> 定位：坑（反模式）——遇问题查，不占主文档（参考分层 A 模式）。

## 反模式表

| ❌ | ✅ |
|----|-----|
| 验收写「功能正常」 | 验收写可测试的定量标准 |
| change 闭环不更新 spec | 判断功能是否成为约束，是则更新 |
| spec 做成文件夹塞子文档 | spec 永远是单文件 |
| 分析报告放 change 目录 | 放知识库 `references/`，按命名规范 |
| change 只有设计没有验收 | spec/design/tasks/check 四要素缺一不可 |
| archive 分 completed/abandoned 子目录 | 扁平，读 check.md 知成败 |
| design 吸收 = 把 design.md 拷过去 | 合并进现有文档对应章节 |
| references 用子目录按 change 组织 | 扁平，文件名 YYYYMMDD-标题.md |
| references 无头部元数据 | 必须含标题/日期/来源/状态/摘要 |
| spec.md 改完不记录 changelog | 每次改动追加一行 |

## 根因备注

- **归档漏项目级 spec 复审**：归档检查清单三项（design 合并 design/ / 项目级 spec 复审更新 / 产出落知识库）全绿才算完成——漏 spec 复审即归档未完成
- **check 越界定标准**：标准由 spec 定、方法由 check 给、结果落 tasks——check 不得出现 spec 之外的门槛（如 spec ≥94 则 check 不得写 ≥90）
- **基线双头**：基线数字以 tasks「测试数据」为单一权威源，spec 标准表/check 验收的基线列引用此处
