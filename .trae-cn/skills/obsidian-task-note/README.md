# obsidian-task-note v1.0.1

将用户粘贴的任务描述转换为符合团队规范的 Obsidian Markdown 任务笔记。

## 特性

- 输入：任务描述 + project/type/task_type（不抓 URL、不访问网络）
- 输出：frontmatter + 标题 + 核心问题 + 分级结论 + 日期块，严格按团队规范
- 文件名前缀由 type 决定：bug→`bug-xxx.md`、task→`task-xxx.md`、cve→`cve-xxx.md`、info→`info-xxx.md`
- frontmatter `id` 与文件名强制一致
- 13 条输出前校验（取值域/标签/日期/工时/分级结论/结论段留空）
- 分级结论段新建时为空（仅标题），无预填占位

## 目录结构

```
obsidian-task-note/
├── SKILL.md          # 主流程：输入声明、6 步处理、校验、约束
├── template.md       # 笔记模板（frontmatter + 正文 + 日期块占位）
├── reference.md      # 取值域、标签体系、命名规则、分级结论、校验清单
├── examples/
│   └── bug-705127.md # 完整示例（type=bug）
└── README.md
```


## 安装

1. 将整个 `obsidian-task-note/` 目录放入 Skill 目录（如 `~/.claude/skills/` 或团队约定的 Skill 根目录）。
2. 确认 `SKILL.md` 的 frontmatter `name: obsidian-task-note` 与目录名一致。
3. 在 Obsidian vault 中确认 `kylin/2026/` 目录存在（或按 `target_dir` 指定）。
4. 无需额外依赖；Skill 不访问网络。

## 使用

### 输入（用户提供）

**必填：**
- `task_description`：任务描述全文（从 pm.kylin.com 粘贴）
- `project`：项目代号（如 `ZF`、`V11`）
- `type`：`bug` | `task` | `cve` | `info`
- `task_type`：`开发` | `测试` | `分析` | `沟通` | `文档`

**可选：**
- `task_id`：任务编号（从描述/URL 提取）
- `priority`：`high` | `medium` | `low`（默认 `medium`）
- `alias`：一句话摘要（默认从描述提取）
- `target_dir`：输出目录（默认 `kylin/2026/`）

### 示例

用户输入：

```
project: V11
type: bug
task_type: 分析
任务描述：（粘贴 pm.kylin.com 页面全文）
```


输出：`kylin/2026/bug-705127.md`（见 `examples/bug-705127.md`）


### 输出

1. 生成的文件路径
2. 笔记全文（markdown code block）
3. 待人工确认项（如 `contribution` 推断不确定、`task_id` 缺失）

## 输出前校验（13 条）

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

## 规则来源

- `标签规范.md`
- `工作流.md`
- `命名规范.md`
- `模板-12345.md`

## 已知限制

- **Skill 不抓取 URL**：所有信息由用户粘贴，缺失必填项时追问，不猜测。
- **`task_id` 缺失**：停止输出并追问，不生成 `TBD` 文件。
- **`type=info`**：无 `kylin/info` 标签，用 `reference`/`work` 替代。
- **`tsk-` 前缀**：按日期批次任务，不参与 `type` 映射，单独处理。
- **`cve-` 笔记**：文件名不含 `bug-`/`task-`/`tsk-`，不进 `kylin/日报提取.md` 统计，需另行处理。

## 版本

- v1.0.1：新建笔记分级结论段留空；输出前校验扩为 13 条，三处清单对齐；SKILL 第 1 步禁止备注写入结论段
- v1.0.0：首次发布，规则闭环；四份文件（SKILL/template/reference/示例）通过交叉一致性检查。

## 后续迭代（非本版阻断）

- 补 `task-` 类型示例，与 `bug-` 示例并列（`examples/task-xxxxx.md`）

## 维护
- 维护者：6peng
- 反馈：11373815@qq.com
