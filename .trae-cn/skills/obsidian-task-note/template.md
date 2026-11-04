---
id: <PREFIX>-<ID>
status: in-progress
project: <PROJECT>
type: <TYPE>   # bug | task | cve | info | 分析说明
task_type: <TASK_TYPE>
start_date: <YYYY-MM-DD>
contribution: <CONTRIBUTION>
patches: 0
commits: 0
documents: 0
packages: 0
alias: <ALIAS>
date: <YYYY-MM-DD>
tags: [<WORK_TAG>, <TECH_TAG>]   # 2-5 个，数组写法；工作类型 + 技术维度；禁 priority/* 与 work
priority: <PRIORITY>
---

# <ALIAS>

## 核心问题
<CORE_ISSUE>

## 根因分析（最终结论）


## 解决方案（最终结论）


---

## <YYYY-MM-DD>

### 进展
- <PROGRESS_ITEM_1>
- <PROGRESS_ITEM_2>

### 结果
<RESULT_SUMMARY>
工时: 0h

### 下一步
- [ ] <NEXT_STEP_1>
- [ ] <NEXT_STEP_2>

<!--
说明：
- 状态只写 frontmatter 的 status 字段（fixed/done/pending/in-progress/wontfix/blocked），不要写进 tags。
- 工时行严格为 `工时: <数字>h`（3h、3.5h 合法；3.0、工时 : 、工时:3h、6 h 均不合规）。
- 结项当天（status: done/fixed/wontfix）可省略 `### 下一步` 或只写一行「未记录（已结项）」，但 `### 结果` 与末行工时必须有。
- `type: 分析说明`（配 `task_type: 分析`）不使用本模板的日期块与工时行，分级结论用「分析范围 / 分析结论」。
-->
