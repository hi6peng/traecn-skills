# Changelog

本文件记录 obsidian-task-note 的版本变更。格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，版本号遵循 [语义化版本](https://semver.org/lang/zh-CN/)。

## [Unreleased]

### 计划中
- `SKILL.md` 第 5 步补第 12 条校验「frontmatter 取值域合规（status/type/task_type/priority）」，与 README 12 条完全对齐
- 补 `task-` 类型示例，与 `bug-` 示例并列（`examples/task-xxxxx.md`）
- `SKILL.md` 第 3 步补说明「分级结论标题按 reference.md 第七节选择，本步仅示意 bug/task 排查类」

---

## [1.0.0] - 2026-10-26

### 新增
- 首次发布 `obsidian-task-note` Skill，将用户粘贴的任务描述转换为符合团队规范的 Obsidian Markdown 任务笔记
- `SKILL.md`：输入声明（必填/可选）、6 步处理流程、输出前校验、约束
- `template.md`：笔记模板（frontmatter + 标题 + 核心问题 + 分级结论 + 日期块占位）
- `reference.md`：字段取值域、标签体系、标题规范、日期块硬性格式、文件命名与位置、contribution 速查表、正文分级结论双维度矩阵、12 条校验清单
- `examples/bug-705127.md`：完整示例（type=bug，kdump 排查场景）
- `README.md`：特性、目录结构、安装、使用、输出前校验、规则来源、已知限制、版本、后续迭代、维护信息

### 规则
- 文件名前缀由 `type` 决定：bug→`bug-xxx.md`、task→`task-xxx.md`、cve→`cve-xxx.md`、info→`info-xxx.md`
- frontmatter `id` 与文件名强制一致
- `status` 只在 frontmatter，不写入 tags
- 日期标题 `## YYYY-MM-DD` 必须补零；三个子标题精确为 `### 进展` / `### 结果` / `### 下一步`
- `工时: Nh` 固定在 `### 结果` 末行
- `type=info` 无 `kylin/info` 标签，用 `reference`/`work`
- 分级结论由 `type` + `task_type` 共同决定

### 已知限制
- Skill 不抓取 URL、不访问网络，所有信息由用户粘贴
- `task_id` 缺失时停止输出并追问，不生成 `TBD` 文件
- `type=info` 无对应 `kylin/info` 标签
- `tsk-` 前缀不参与 `type` 映射，单独处理
- `cve-` 笔记不进 `kylin/日报提取.md` 统计，需另行处理

### 规则来源
- `标签规范.md`
- `工作流.md`
- `命名规范.md`
- `模板-12345.md`

### 维护
- 维护者：6peng
- 反馈：11373815@qq.com
