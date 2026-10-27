---
name: obsidian-task-note
description: 将用户粘贴的任务描述转换为符合团队规范的 Obsidian Markdown 任务笔记（frontmatter + 标题 + 核心问题 + 日期块）
---

# Obsidian 任务笔记生成

## 何时使用
用户提供任务描述（粘贴文本），并声明 `project`、`type`、`task_type`，要求生成/创建 Obsidian 笔记时。

## 输入声明（由用户提供，Skill 不自行抓取 URL）

**必填：**
- `task_description`：任务描述全文
- `project`：项目代号
- `type`：`bug` | `task` | `cve` | `info`
  - **决定笔记文件名前缀**：bug → `bug-xxxxx.md`，task → `task-xxxxx.md`，cve → `cve-xxxxx.md`，info → `info-xxxxx.md`
- `task_type`：`开发` | `测试` | `分析` | `沟通` | `文档`

**可选：**
- `task_id`：任务编号（从描述或 URL 中提取，如 `705127`）
- `priority`：`high` | `medium` | `low`（默认 `medium`）
- `alias`：一句话摘要（默认从描述提取）
- `target_dir`：输出目录（默认 `kylin/2026/`）

> ⚠️ Skill **不抓取 URL、不访问网络**。所有信息由用户粘贴提供。缺失必填项时，先向用户追问，不得猜测。

## 处理流程

### 1. 解析任务描述
从 `task_description` 中提取：
- **任务标题**：去掉 `task-xxxxx`、`【…】` 前缀后的内容
- **核心问题**：任务描述中"问题/现象/故障"部分，一句话概括
- **ps/备注**：补充信息，放入 `## 核心问题`；**不得放入根因分析/解决方案段**（新建时该两段留空）
- **task_id**：若描述中含 `task-view-<id>` 或 `task-<id>`，提取数字部分

### 2. 填充 frontmatter（严格按 reference.md 取值域）

| 字段 | 来源 / 规则 |
|---|---|
| `id` | <PREFIX>-<id>，<PREFIX> 由 type 决定（见第 4 步）；**`task_id` 缺失时停止输出、追问用户，不得生成 TBD** |
| `status` | 新任务默认 `in-progress`；已完成用 `done`（全小写） |
| `project` | 用户声明值，原样填入 |
| `type` | 用户声明值（`bug`/`task`/`cve`/`info`） |
| `task_type` | 用户声明值（`开发`/`测试`/`分析`/`沟通`/`文档`） |
| `start_date` / `date` | 生成当天日期，`YYYY-MM-DD` 补零 |
| `contribution` | 按 reference.md 速查表推断；无法判断时留空 |
| `patches`/`commits`/`documents`/`packages` | 默认 `0`，按描述更新 |
| `alias` | 一句话摘要，**不含** `task-xxxxx`、【…】前缀 |
| `tags` | 3–5 个，小写分层，按 reference.md 标签体系；**status 不写入 tags** |
| `priority` | 用户提供或按描述关键词推断，默认 `medium` |

### 3. 填充正文（按 template.md）

- `# 标题`：同 alias 规范，**不含** `task-xxxxx`、【…】前缀
- `## 核心问题`：一句话写清现象，WPS 日报 `core_issue` 数据源
- `## 根因分析（最终结论）` / `## 解决方案（最终结论）`：
  - 新建笔记时标题下留空，不写"排查中""初步方向""计划动作"等任何预填内容
  - 仅当用户明确提供了已确定的根因/方案时才回填
  - 分级结论标题按 type+task_type 选择（见 reference.md 第七节），本步仅示意 bug/task 排查类
- `---` 分隔
- 首个日期块 `## YYYY-MM-DD`（补零），含 `### 进展` / `### 结果` / `### 下一步`
  - `### 结果` 末行写 `工时: 0h`
  - `### 下一步` 用 `- [ ]` 待办写法

### 4. 文件命名与位置（按 type + 命名规范.md）

文件名前缀由 `type` 决定：

| type | 文件名格式 | 示例 |
|---|---|---|
| bug | `bug-<id>.md` | `bug-705127.md` |
| task | `task-<id>.md` | `task-705127.md` |
| cve | `cve-<id>.md` | `cve-20261026.md` |
| info | `info-<id>.md` | `info-001.md` |

- 编号类一律用**连字符** `-`，不用下划线
- 位置：`kylin/2026/`；按主题归档放 `kylin/<两位编号>_<中文主题>/`
- 文件名必须含对应前缀（`bug-`/`task-`/`cve-`），否则不进日报统计
- frontmatter 的 `id` 字段必须与文件名前缀一致（`bug-705127` ↔ `bug-705127.md`）

### 5. 校验（输出前逐项检查，共 13 条）
- [ ] frontmatter 取值域合规（status/type/task_type/priority）
- [ ] status 只在 frontmatter，tags 无 `fixed`/`done` 等状态词
- [ ] 标题无编号/【…】前缀
- [ ] tags 3–5 个、小写、分层
- [ ] 日期 `## YYYY-MM-DD` 补零，三子标题精确无后缀
- [ ] `工时: Nh` 在 `### 结果` 末行，格式固定
- [ ] 块内子点用 `####`/加粗，无 `###` 抢层级
- [ ] 文件名前缀与 `type` 一致（bug→`bug-`，task→`task-`，cve→`cve-`，info→`info-`）
- [ ] frontmatter `id` 与文件名一致
- [ ] `### 结果` 段非空（无结论也写"待验证"等说明）
- [ ] `type=info` 时 tags 无 `kylin/info`，用 `reference`/`work`
- [ ] 分级结论格式与 `type`+`task_type` 匹配（见 reference.md 第七节）
- [ ] 新建笔记分级结论段为空（仅标题），无"排查中/初步方向/计划动作"等预填

### 6. 输出
返回：
1. 生成的文件路径
2. 笔记全文（markdown code block）
3. 待人工确认项（如 `contribution` 推断不确定、`task_id` 缺失）

## 约束
- 不访问网络、不抓取 URL
- 不猜测用户未声明的 `project`/`type`/`task_type`
- 日期必须补零；三个子标题不得加冒号/括号
- 工时行格式固定 `工时: Nh`（允许小数如 `3.5h`）
