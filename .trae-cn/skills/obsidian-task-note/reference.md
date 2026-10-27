# Obsidian 任务笔记 参考规范

## 一、字段取值域

### status（全小写）
`fixed` | `done` | `pending` | `in-progress` | `blocked` | `wontfix`

- 新任务：`in-progress`
- 排查完成待验证：`pending`
- 已验证关闭：`done` 或 `fixed`
- 无法复现/不修复：`wontfix`

### type
`bug` | `task` | `cve` | `info`

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

## 二、标签体系（3–5 个，小写，斜杠分层）

### 工作标签
`kylin/bug`、`kylin/task`、`kylin/cve`

### 技术分类
`kernel`、`network`、`graphics`、`power`、`boot`、`systemd`、`debug`、`performance`、`security`、`filesystem`、`memory`、`virtualization`、`accessibility`、`i18n`、`install`、`upgrade`、`stability`、`tty`、`live`

### 平台
`platform/x86`、`platform/loongarch`、`platform/sw64`、`platform/arm`

### 硬件组件
`component/gpu`、`component/nic`、`component/audio`、`component/storage`、`component/usb`、`component/touchpad`、`component/battery`、`component/serial`

### 分层专名
具体技术名词必须挂类别分层，不得裸用：`driver/<驱动名>`（i915、loonggpu、mwv207…）、`tool/<工具名>`（lmbench、fio、glmark2…）、`software/<软件名>`（xorg-server、ukui-greeter…）、`graphics/<技术点>`（drm、hdmi、gamma…）；完整表见 `spec/标签规范.md`

### 优先级
`priority/high`、`priority/medium`、`priority/low`

### 内容类型
`learn`、`work`、`important`、`todo`、`reference`

**规则：**
- status 不写入 tags（`fixed`/`done` 等只在 frontmatter `status` 字段）
- 3–5 个，全小写，斜杠分层
- `type=info` 无对应 `kylin/info`，改用 `reference` 或 `work`

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
- `### 结果` 末行写 `工时: Nh`（允许小数 `3.5h`），格式固定：
  - ✅ `工时: 3h`、`工时: 3.5h`
  - ❌ `工时：3小时`、`耗时3h`、另起 `### 工时` 段
- `### 结果` 当天无结论也要写一句（如"待验证"），不留空
- `### 下一步` 用 `- [ ]` 待办写法

---

## 五、文件命名与位置

### 文件名前缀由 type 决定

| type | 文件名格式 | 示例 |
|---|---|---|
| bug | `bug-<id>.md` | `bug-705127.md` |
| task | `task-<id>.md` | `task-705127.md` |
| cve | `cve-<id>.md` | `cve-20261026.md` |
| info | `info-<id>.md` | `info-001.md` |

- 编号类用**连字符** `-`，不用下划线（`bug_705127.md` ❌）
- 文件名必须含对应前缀，否则不进 `kylin/日报提取.md` 统计
- frontmatter `id` 必须与文件名一致（`bug-705127` ↔ `bug-705127.md`）
- 模板占位符约定: <PREFIX> == <TYPE>；<WORK_TAG>：bug→kylin/bug、task→kylin/task、cve→kylin/cve、info→reference

> 例外：`tsk-` 前缀（如 `tsk-20260113-1.md`）用于按日期批次的任务，**不参与 `type` 映射**，单独处理。

### 位置
- 默认：`kylin/2026/`
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

### 说明
- 「分析类」「测试类」是 `task_type` 的值，**不是 `type` 的值**；`type` 只有 bug/task/cve/info。
- 新写笔记统一补 `（最终结论）` 后缀。
- 无法复现类：在「根因分析（最终结论）」描述测试情况，「解决方案（最终结论）」写"问题无法复现"（此为结论已定后的回填写法，非新建预填）。
- 新建笔记时两个分级结论段**只保留标题、正文留空**，待有结论后回填；不得预填"排查中""待验证"等占位说明。未处理状态由 frontmatter `status: in-progress` 表达。
- 注意：`### 结果`（日期块内）允许写"排查中/待验证"；`## 根因分析（最终结论）`/`## 解决方案（最终结论）` 新建时必须为空。两者不冲突。

---

## 八、校验清单（13 条）
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
- [ ] 分级结论格式与 `type`+`task_type` 匹配（见第七节）
- [ ] 新建笔记分级结论段为空（仅标题），无"排查中/初步方向/计划动作"等预填
