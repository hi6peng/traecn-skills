# Obsidian 任务笔记 参考规范

## 一、字段取值域

### status（全小写）
`fixed` | `done` | `pending` | `in-progress` | `blocked` | `wontfix`

- 新任务：`in-progress`
- 排查完成待验证：`pending`
- 已验证关闭：`done` 或 `fixed`
- 无法复现/不修复：`wontfix`

### type
`bug` | `task` | `cve` | `info` | `分析说明`

- `分析说明`：对外分析文档（`kylin/{年份}/分析说明/`），配 `task_type: 分析`，**不含日期块与工时行**
- 单条 CVE 用 `type: cve`；一次涉及多个 CVE 的组件级/批次分析用 `type: 分析说明` + `cves:` 回指，不再用 `cve-` 编号

### task_type
`开发` | `测试` | `分析` | `沟通` | `文档`

按关键词匹配（见标签规范.md）：
| task_type | 关键词 |
|---|---|
| 开发 | 修改、提交、合入、编译、构建、编码 |
| 测试 | 测试、验证、回归、压测 |
| 分析 | 排查、分析、定位、诊断、评估、调研 |
| 沟通 | 沟通、会议、评审、讨论、对接 |
| 文档 | 编写、整理、更新、撰写 |

### priority
`high` | `medium` | `low`（默认 `medium`）

### project（规范命名，禁止自创写法）

用户声明值须先按下表归一化，再填入；不在表中的新值，生成前与用户确认。

| 规范值 | 别名 / 旧写法（禁止使用） |
|---|---|
| `GF-V10桌面-ZF整机` | V10-GF-ZF整机、ZF |
| `GFKD2605` | V10-GF-GFKD2605 |
| `GFKD2510` | — |
| `GF-V10桌面-集采` | V10-GF-集采、GF-V10桌面-JICAI |
| `GF-V10桌面-LA` / `GF-V10桌面-LA64` / `GF-V10桌面-ZFOS` | — |
| `GF-V10桌面-20.1.6` / `GF-V10桌面-20.1.7` / `GF-V10桌面-020补丁` | — |
| `GF-V10桌面-预研` / `GF-V10桌面-摸底` / `GF-V10桌面-信创` / `GF-V10桌面-维护` / `GF-V10桌面-适配` / `GF-V10桌面-掉电保护` | — |
| `V11桌面-预研` / `V11服务器-预研` | V11（须确认桌面/服务器后归一） |
| `GF-项目成果` / `GF-售前` / `GF-生态适配` | — |

无法归一时填 `待确认`，并在正文中提示用户补充。

---

## 二、标签体系（2–5 个，小写，斜杠分层）

**唯一来源：`spec/标签规范.md`**（工作标签、状态、技术分类、内容类型、平台、硬件组件、驱动、工具、图形子类、启动子类、镜像子类、软件栈）。本文件不再维护第二份分类清单，避免两处各写一套；新增类别、废弃项与替代项只改 spec（如 `software/xorg`→`software/xorg-server`、`driver/d3000m`→`platform/d3000m`、`software/ukui-power-manager`→`software/power-manager`）。

### 组成要求（2–5 个）
- 数量口径以 `spec/标签规范.md` 为单一来源：2–5 个，不是 3–5
- **工作/内容类型（至少一个）**：`kylin/bug`、`kylin/task`、`kylin/cve`、`reference`、`important`（另有 `kylin`、`todo`）
- **技术维度（至少一个）**：技术前缀（`kernel`、`network`、`graphics`、`power`、`boot`、`systemd`、`debug`、`performance`、`security`、`filesystem`、`memory`、`virtualization`、`accessibility`、`i18n`、`install`、`upgrade`、`stability`、`tty`、`live`、`firmware`）、`platform/*`、`component/*`，或分层专名 `driver/*`、`tool/*`、`software/*`、`graphics/*`、`boot/*`、`live/*`、`filesystem/*`、`virtualization/*`
- 上位类裸用（如单独 `graphics`）仅在泛指、不涉及具体对象时可用

### `note/` 标签口径（与 kylin/ 隔离）
- 形态：`learn/<域>/<主题>`，2–3 段，根固定为 `learn/`；**词表开放、不做白名单**，只查形状
- `learn/` 命名空间与 kylin 前缀（`platform/`、`driver/`、`component/`、`software/`、`boot/`、`tool/` 等）互不重叠，note/ 不使用这些前缀
- **不再保留裸 `learn` 作为独立工作类型标签**：层级根已包含在每条标签里
- 数量 1–5；校验只查形状：以 `learn/` 开头、2–3 段、无空段、无重复
- 示例：`learn/命令行/grep`、`learn/内核/perf`、`learn/网络/tcp`；❌ `learn`、`learn/命令行/grep/grep`、`learn//tcp`、`kernel`

### 写法与禁止项
- **必须是 YAML 数组，键名必须是 `tags`**：`tags: [kylin/bug, graphics]` ✅；`tag: [...]` ❌（Obsidian 不识别，等于没打标签）；`tags: plymouth` ❌（裸标量）
- 具体技术名词必须挂类别分层，不得裸用：`i915` ❌ → `driver/i915`；`plymouth` ❌ → `boot/plymouth`
- 未收录的新名词：先挂最近类别分层；无合适类别时先在 `spec/标签规范.md` 补类别再使用，不得自造体系外标签
- **不接受 `priority/*` 标签**：优先级只用 frontmatter `priority` 字段（`high` | `medium` | `low`，默认 `medium`）
- **不接受 `work` 标签**：工作记录用 `kylin/bug`/`kylin/task`/`kylin/cve`/`reference`/`important`
- status 不写入 tags（`fixed`/`done` 等只在 frontmatter `status` 字段）
- `type=info` 无对应 `kylin/info`，只用 `reference`

---

## 三、标题规范
- `# 标题内容`，**不含** `task-xxxxx`、`bug-xxxxx`、【…】前缀
- 用"动词+对象"描述，如 `# 修复 Xorg 进程 CPU 占用高`
- 别名放 frontmatter `alias`

---

## 四、日期块硬性格式
- 日期标题：`## YYYY-MM-DD`，**月和日必须补零**（`## 2026-10-9` 会被漏掉）
- 一天一个块，追加在笔记末尾
- 三个子标题**精确**为 `### 进展` / `### 结果` / `### 下一步`，**不加冒号或括号**
  - `### 结果：`、`### 进展（10-22）` 会导致整段取不到
- 块内子点用 `####` 或加粗，**不得用 `###` 抢层级**
- `### 结果` 末行写 `工时: <数字>h`，格式固定：
  - ✅ `工时: 3h`、`工时: 3.5h`（小数合法）
  - ❌ `工时: 3.0` / `工时: 6.0`（缺 `h` 单位）、`工时 : 1h`（冒号前空格）、`工时:1h`（冒号后无空格）、`工时: 6 h`（数字与 `h` 间有空格）、`工时：3小时`、`耗时3h`、另起 `### 工时` 段
- `### 结果` 当天无结论也要写一句（如"待验证"），不留空
- `### 下一步` 用 `- [ ]` 待办写法
- **`### 下一步` 豁免**：结项当天（`status: done`/`fixed`/`wontfix`）可省略 `### 下一步`，或只写一行「未记录（已结项）」；但 `### 结果` 与末行工时仍必须有
- **重复判据**：同一日期块内 `### 进展` 与 `### 结果` 正文相似度 ≥0.85 视为重复（自检项），人工确认后合并：过程留在进展、结论留在结果
- `type: 分析说明` 不含日期块与末行工时

---

## 五、文件命名与位置

### 文件名由 type 决定

| type | 文件名格式 | 示例 |
|---|---|---|
| bug | `bug-<id>.md` | `bug-705127.md` |
| task | `task-<id>.md` | `task-705127.md` |
| cve（单条） | `CVE-YYYY-NNNN.md` | `CVE-2024-52533.md` |
| cve（多 CVE 批次分析） | `<组件>_CVE_分析_YYYYMMDD.md`（`type: 分析说明`） | `xorg_server_CVE_分析_20250814.md` |
| info | `info-<id>.md` | `info-001.md` |
| 分析说明 | `{主题}_分析说明.md` | `GP201_startx_分析说明.md` |

- 编号类用**连字符** `-`，不用下划线（`bug_705127.md` ❌）
- 单条 CVE 用官方编号 `CVE-YYYY-NNNN.md`；**校验只对 `CVE-` 前缀强校验 `CVE-\d{4}-\d{4,}`**，其他 CVE 相关文件名不报警
- 多 CVE 批次分析**禁止 `CVE_` 伪编号**，用 `<组件>_CVE_分析_YYYYMMDD.md` + `type: 分析说明` + `cves:` 回指各编号笔记；存量 `CVE_xxx_yyyymmdd.md` 保留原名不要求改名
- 文件名必须含 `bug-`/`task-`/`tsk-` 才进 `kylin/日报提取.md` 统计；`CVE-`、`cve-` 与 `{主题}_分析说明` 不进统计，需另行处理
- frontmatter `id` 必须与文件名一致（`bug-705127` ↔ `bug-705127.md`）；`type: 分析说明` 规范未定义 `id`，不强制
- 模板占位符约定: <PREFIX> == <TYPE>；<WORK_TAG>：bug→kylin/bug、task→kylin/task、cve→kylin/cve、info→reference

> 例外：`tsk-` 前缀（如 `tsk-20260113-1.md`）用于按日期批次的任务，**不参与 `type` 映射**，单独处理。

### 位置
- 默认：`kylin/<当前年份>/`（2026 → `kylin/2026/`，2027 → `kylin/2027/`，逐年新建）
- bug/task 总结说明：`kylin/<当前年份>/分析说明/{主题}_分析说明.md`，frontmatter 使用 `type: 分析说明` + `task_type: 分析`，不含日期块与工时行
- 按主题归档：`kylin/<两位编号>_<中文主题>/`
  - `kylin/00_graphics/`、`kylin/04_kernel/`、`kylin/12_porting/`

---

## 六、contribution 速查表

| 场景 | contribution |
|---|---|
| 写代码 + 提交 | 功能开发, 代码提交 |
| 修 Bug + 提交补丁 | 问题修复, 补丁提交 |
| 代码审查 | 代码审查 |
| 写技术方案文档 | 文档编写 |
| 定位问题根因 | 问题修复, 根因分析 |
| 性能调优 | 性能优化, 代码提交 |
| 技术调研 | 技术调研, 文档编写 |
| 写测试报告 | 文档编写, 测试验证 |

---

## 七、正文分级结论格式

分级结论由 **`type` + `task_type`** 共同决定，不是 `type` 单一维度。

### 主维度：type = bug / task
| task_type | 分级结论格式 |
|---|---|
| 开发 / 分析 / 沟通（排查修复类） | `## 根因分析（最终结论）` + `## 解决方案（最终结论）` |
| 测试 | `## 测试范围（最终结论）` + `## 测试结论（最终结论）` |
| 文档 | `## 文档范围（最终结论）` + `## 文档结论（最终结论）` |

### 主维度：type = info（纯信息/调研/文档）
| task_type | 分级结论格式 |
|---|---|
| 分析 / 沟通（调研类） | `## 分析范围（最终结论）` + `## 分析结论（最终结论）` |
| 文档 | `## 文档范围（最终结论）` + `## 文档结论（最终结论）` |
| 测试 | `## 测试范围（最终结论）` + `## 测试结论（最终结论）` |

### 主维度：type = cve
沿用 bug/task 规则，按 `task_type` 选上表。

### 主维度：type = 分析说明（对外分析文档）
| task_type | 分级结论格式 |
|---|---|
| 分析 | `## 分析范围（最终结论）` + `## 分析结论（最终结论）` |

### 说明
- 「分析类」「测试类」是 `task_type` 的值，**不是 `type` 的值**；`type` 取值域为 bug/task/cve/info/分析说明。
- `type: 分析说明` 只与 `task_type: 分析` 搭配，且不含日期块与工时行。
- 新写笔记统一补 `（最终结论）` 后缀。
- 无法复现类：在「根因分析（最终结论）」描述测试情况，「解决方案（最终结论）」写"问题无法复现"（此为结论已定后的回填写法，非新建预填）。
- 新建笔记时两个分级结论段**只保留标题、正文留空**，待有结论后回填；不得预填"排查中""待验证"等占位说明。未处理状态由 frontmatter `status: in-progress` 表达。
- 注意：`### 结果`（日期块内）允许写"排查中/待验证"；`## 根因分析（最终结论）`/`## 解决方案（最终结论）` 新建时必须为空。两者不冲突。

---

## 八、校验清单（15 条）
- [ ] frontmatter 取值域合规（status/type/task_type），`priority` 字段取值合法（high/medium/low），tags 中不接受 `priority/*` 与 `work`
- [ ] status 只在 frontmatter，tags 无 `fixed`/`done`/`pending`/`in-progress`/`wontfix`/`blocked` 等状态词
- [ ] 标题无编号/【…】前缀
- [ ] tags 2–5 个、小写、分层；YAML 数组写法且键名为 `tags`（不得 `tag:` 或裸标量）；至少一个工作类型 + 一个技术维度（数量口径以 `spec/标签规范.md` 为准；`note/` 不适用本条，走 `learn/<域>/<主题>` 形状校验：2–3 段、1–5 个、词表开放）
- [ ] 日期 `## YYYY-MM-DD` 补零，子标题精确无后缀；结项当天 `### 下一步` 可省略或写「未记录（已结项）」
- [ ] 工时行严格为 `工时: <数字>h`，位于 `### 结果` 末行（`3h`/`3.5h` 合法；`3.0`/`6.0`、`工时 :`、`工时:3h`、`6 h` 不合规）
- [ ] 同一日期块内 `### 进展` 与 `### 结果` 相似度 <0.85，或已人工确认合并
- [ ] 块内子点用 `####`/加粗，无 `###` 抢层级
- [ ] 文件名：bug→`bug-`、task→`task-`、info→`info-`；单条 CVE 匹配 `CVE-\d{4}-\d{4,}`；多 CVE 批次分析为 `<组件>_CVE_分析_YYYYMMDD.md` 且无 `CVE_` 伪编号；`type: 分析说明` 为 `{主题}_分析说明.md`
- [ ] frontmatter `id` 与文件名一致（`type: 分析说明` 不强制 `id`）
- [ ] `### 结果` 段非空（无结论也写"待验证"等说明）
- [ ] `type=info` 时 tags 无 `kylin/info`，只用 `reference`
- [ ] 分级结论格式与 `type`+`task_type` 匹配（含 `type: 分析说明` → 分析范围/分析结论）
- [ ] 新建笔记分级结论段为空（仅标题），无"排查中/初步方向/计划动作"等预填
- [ ] `type: 分析说明` 配 `task_type: 分析`，不含日期块与工时行
