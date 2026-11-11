# Changelog

本文件记录 obsidian-task-note 的版本变更。格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，版本号遵循 [语义化版本](https://semver.org/lang/zh-CN/)。

## [Unreleased]

### 计划中
- 补 `task-` 类型示例，与 `bug-` 示例并列（`examples/task-xxxxx.md`）

---

## [1.0.5] - 2026-11-05

### 变更
- `### 下一步` 不再必需：`### 进展`、`### 结果` 必需，`### 下一步` 可选——有明确后续就写 `- [ ]` 待办，没有就整节省略，**不写「未记录」「暂无」等占位**
- 删除原「结项当天（`status: done`/`fixed`/`wontfix`）豁免」表述，不再保留两套规则
- 校验清单该项降级为**提示项**（非 error/warn）：仅当未结项（`status: pending`/`in-progress`/`blocked`）且该日期块无 `### 下一步` 时提示「可补充后续计划」；已结项不提示
- SKILL/reference/README 三处校验清单与约束段同步；reference 第四节标题由「日期块硬性格式」改为「日期块格式约束」
- `template.md` 的 `### 下一步` 标注为可选并注明省略方式（整节省略，连带占位行删除）；示例 `bug-705127.md` 的下一步为真实待办，无需改动
- 版本号顺延 1.0.4 → 1.0.5

---

## [1.0.4] - 2026-11-05

### 变更
- `note/` 标签改用 `learn/` 根命名空间：形态 `learn/<域>/<主题>`（2–3 段），词表开放、不做白名单，与 kylin 前缀（`platform/`、`driver/`、`component/`、`software/`、`boot/`、`tool/` 等）互不重叠
- 删除裸 `learn` 作为独立工作类型标签（层级根已包含在每条标签内）
- `note/` 只校验形状（以 `learn/` 开头、2–3 段、无空段、无重复、数量 1–5），不查词表；kylin/ 侧规则不变（闭合词表 + 强校验 + 2–5 + 工作类型/技术维度）
- 替换旧表述「note/ 适用 learn 约定、允许 1–2 个自由技术词」；SKILL/reference/README 三处校验清单同步
- 版本号顺延 1.0.3 → 1.0.4

---

## [1.0.3] - 2026-11-04

### 变更
- `type` 取值域补 `分析说明`（配 `task_type: 分析`，用于 `kylin/{年份}/分析说明/` 对外分析文档，不含日期块与工时行）；分级结论补 `## 分析范围（最终结论）` + `## 分析结论（最终结论）`
- CVE 命名对齐 `spec/命名规范.md`：单条用 `CVE-YYYY-NNNN.md`（只对 `CVE-` 前缀强校验 `CVE-\d{4}-\d{4,}`）；多 CVE 批次分析用 `<组件>_CVE_分析_YYYYMMDD.md` + `type: 分析说明` + `cves:` 回指；禁止 `CVE_` 伪编号；删除原 `cve-<id>.md` 写法
- 工时校验收紧为 `工时: <数字>h`：新增不合规样例 `3.0`/`6.0`（缺 h）、`工时 :`（冒号前空格）、`工时:3h`（冒号后无空格）、`6 h`（数字与 h 间空格）；修正示例 `工时: 0.5` → `工时: 0.5h`
- 日期块补 `### 下一步` 结项豁免（`status: done`/`fixed`/`wontfix` 可省略或写「未记录（已结项）」，`### 结果` 与工时仍必须）〔已被 v1.0.5 取代：`### 下一步` 整体改为可选，不再有结项豁免〕
- 补重复判据：同块 `### 进展` 与 `### 结果` 正文相似度 ≥0.85 视为重复（自检项，人工确认后合并）
- 标签数量口径明确以 `spec/标签规范.md` 为单一来源（2–5，非 3–5）；补 `note/` 学习笔记 `learn` 约定豁免（允许 1–2 个自由技术词）〔已被 v1.0.4 取代：改为 `learn/<域>/<主题>` 形状校验〕；标签表新增/废弃项改由 spec 单一维护（含 `software/xorg`→`software/xorg-server`、`driver/d3000m`→`platform/d3000m`、`software/ukui-power-manager`→`software/power-manager`）
- 输出前校验由 13 条扩为 15 条（新增日期块结项豁免与重复判据、命名规则合并且覆盖 CVE/分析说明）；SKILL/reference/README 三处清单同步
- README「规则来源」改为实际路径 `spec/*.md`，移除不存在的 `模板-12345.md`

---

## [1.0.2] - 2026-11-04

### 变更
- 工单笔记按年度归集：默认输出目录由 `kylin/2026/` 改为 `kylin/<当前年份>/`（2026 → `kylin/2026/`，2027 → `kylin/2027/`，逐年新建）
- bug/task 总结说明统一放在 `kylin/<当前年份>/分析说明/`，文件名 `{主题}_分析说明.md`，frontmatter 使用 `type: 分析说明`，由对应工单笔记 wikilink 引用
- `SKILL.md` / `reference.md` / `README.md` 三处位置规则同步

## [1.0.1] - 2026-10-27

### 变更
- 新建笔记分级结论段只保留标题、正文留空；移除 template/SKILL/示例中的"排查中/初步方向/计划动作"预填内容（示例日期块 `### 结果` 的"排查中"属当天记录，保留）
- 输出前校验由 12 条扩为 13 条，新增「新建笔记分级结论段为空（仅标题）」；SKILL/reference/README 三处清单对齐
- `SKILL.md` 第 1 步明确禁止将 ps/备注写入根因分析/解决方案段
- `reference.md` 第七节补充区分说明：`### 结果`（日期块内）允许写"排查中/待验证"，两个分级结论段新建时必须为空

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
- 日期标题 `## YYYY-MM-DD` 必须补零；三个子标题精确为 `### 进展` / `### 结果` / `### 下一步`〔已被 v1.0.5 取代：`### 下一步` 改为可选〕
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
