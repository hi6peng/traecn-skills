---
name: obsidian-frontmatter-normalizer
description: Scan and fix Obsidian note frontmatter issues in this vault, including empty frontmatter, missing required fields, non-compliant tags, and plaintext credentials. Use when the user asks to normalize, batch-complete, audit, or fix frontmatter in kylin notes. Do not use for note learning notes unless explicitly requested.
---

# Obsidian Frontmatter Normalizer

Normalize frontmatter across `kylin/` notes per vault conventions. Read `spec/标签规范.md` and `AGENTS.md` first for the authoritative tag taxonomy and naming rules.

## Critical — Obsidian Overwrite Issue

When Obsidian desktop is running, the `Edit` tool changes get reverted by Obsidian's in-memory save. **Always use `sed -i` via Shell for file modifications**, then verify with `Read` or `grep`. Do not rely on the `Edit` tool for bulk frontmatter fixes.

## Detection

Run these scans from the vault root to find issues. Filter `WARNING` noise from shell output.

### 1. Empty frontmatter `{}`

```bash
find kylin -name "*.md" -type f | while read f; do
  if head -2 "$f" | tail -1 | grep -q "^{}$"; then echo "$f"; fi
done
```

### 2. Missing task_type (bug/task notes)

```bash
grep -rL "task_type:" kylin --include="bug-*.md" --include="task-*.md" --include="tsk-*.md" --exclude-dir="分析说明"
```

> 扫描覆盖所有年度目录（`kylin/2026/`、`kylin/2027/` …）。`kylin/{年份}/分析说明/` 下的总结说明使用 `type: 分析说明`，不要求 `task_type`，故排除。

### 3. Non-compliant tags (bare technical nouns without category prefix)

```bash
grep -rn "^tags:" kylin --include="*.md" | grep -vE "kylin/|platform/|driver/|tool/|graphics/|software/|component/|performance|security|filesystem|memory|virtualization|accessibility|i18n|install|upgrade|stability|tty|live|firmware|debug/|learn|reference|important|todo|fixed|done|pending|in-progress|wontfix|kernel|network|boot|systemd|debug|graphics|power"
```

### 3b. Invalid tag key / scalar tags

`tag:` (singular) is **not recognized by Obsidian** — the note effectively has no tags. A bare `tags: <word>` is a YAML scalar, not a tag list.

```bash
grep -rnE "^tag:" kylin --include="*.md"
grep -rnE "^tags:[[:space:]]*[^[]" kylin --include="*.md"
```

### 4. Plaintext credentials

```bash
grep -rnE "密码|password|passwd" kylin --include="*.md" | grep -vE "输入|验证|修改|忘记|重置|错误|正确|框|回显|<已移除>|需要密码|无需密码|passwd \*|getpwnam|/etc/passwd|struct passwd|注意密码|密码锁定|密码服务|用户名和密码|密码要求|No username" | grep -iE "(:|：|是|为|=|@|[0-9]{1,3}\.|root |kylin )"
```

## Fix Rules

### Empty frontmatter

Replace the first 3 lines (`---`, `{}`, `---`) with proper frontmatter. Use `sed -i '1,3c\...'`.

**Info/reference files** (in `14_info/`, `11_infra/`, `15_sharing/`, `99_archive/`, etc.):

```yaml
---
tags: [kylin, reference, <category>]
type: info
status: done
date: <file-modification-date>
---
```

Pick `<category>` from the file content or directory: `tool/...`, `debug`, `graphics`, `performance`, `security`, `platform/loongarch`, `platform/x86`, `platform/arm`, `software/...`, `driver/...`, etc. Result must stay 2-5 tags: one work/content type + one technical dimension, per `spec/标签规范.md`.

**Bug/task files** already have full frontmatter; only fix the specific missing field.

### Missing task_type

Infer from content keywords:
- 分析 (排查/定位/分析/诊断)
- 测试 (测试/验证/回归/压测)
- 开发 (修改/提交/合入/编译/构建/编码)
- 沟通 (沟通/会议/评审/讨论/对接)
- 文档 (编写/整理/更新/撰写)

Add `task_type: <value>` after the `type:` line.

### Non-compliant tags

- Bare technical nouns must get a category prefix per `spec/标签规范.md`: `i915` → `driver/i915`, `ltp` → `tool/ltp`, `xorg` → `software/xorg-server`, `lightdm` → `software/lightdm`, `plymouth` → `boot/plymouth`, `squashfs` → `filesystem/squashfs`, `live-build` → `live/live-build`
- Tags must be a **YAML array under the key `tags`**: `tags: [a, b]` — never `tag:`, never a bare scalar
- Keep 2-5 tags per file: at least one work/content type (`kylin/bug`/`kylin/task`/`kylin/cve`/`reference`/`important`) **and** at least one technical dimension (tech prefix / `platform/*` / `component/*` / layered name). Never add tags just to hit the count
- New technical nouns not in the spec: map to the **closest existing layered category** first; if none fits, add the category to `spec/标签规范.md` before using it — never invent out-of-taxonomy tags
- Tags must not contain `priority/*` or `work` (priority goes in the `priority` field; work records use `kylin/bug`/`kylin/task`/`kylin/cve`/`reference`/`important`)
- Use `kylin/bug` for bug files, `kylin/task` for task files
- `priority` is a **frontmatter field** (`high`/`medium`/`low`), not a tag: move any `priority/*` tag value into `priority:` and delete the tag

### Invalid tag key / scalar tags

- `tag:` → rename the key to `tags:` on the same line (Obsidian ignores `tag:`)
- `tags: <scalar>` → rewrite as an array: `tags: [<work type>, <technical tag>]`
- The scalar value is usually a bare technical noun → map it to its layered name (`plymouth` → `boot/plymouth`) instead of keeping it bare
- After conversion still keep 2-5 tags with one work/content type + one technical dimension

### Plaintext credentials

Replace passwords and internal IPs with `<已移除>`. **Always confirm with the user before removing** — they may want credentials kept in the vault. If kept, no action needed.

## Verification

After each batch fix, re-run the detection scans to confirm the count dropped to zero (or to the expected remaining count). Spot-check 2-3 files with `Read` to confirm `sed` wrote correctly.

## Do Not

- Do not process `note/` learning notes unless explicitly asked — they use a different (`learn`) tag convention
- Do not remove `contribution: ` empty field in bug/task frontmatter — it is consistent across the vault
- Do not fill 根因分析/解决方案 sections when `task_type: 分析` — convention says conclusion sections stay empty for analysis tasks
