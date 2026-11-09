---
alwaysApply: true
scene: git_message
---

# Git 提交信息生成规则

生成 commit message 时，必须遵循以下结构与风格。提交信息使用中文，分为两个固定段落：**修改思路** 与 **改动对象**。

## 结构模板

```
修改思路：[用一整段连贯文字说明改动的来龙去脉]

改动对象：
- [相对路径文件名]
  - [改动点1：具体描述做了什么]
  - [改动点2：具体描述做了什么]
```

## 各字段要求

### 修改思路

用一段连贯的中文文字说明，必须覆盖以下要点：

1. **原有逻辑的问题**：改动前代码是怎么做的，存在什么不足（如判断条件不完整、遗漏边界、仅支持目录不支持符号链接等）。
2. **本次改动方案**：改成了什么机制/逻辑（如用 realpath 解析后比对受保护列表、增加 S_ISLNK 判断等）。
3. **影响范围与安全性**：说明改动如何避免影响非目标场景，未命中条件时行为与改动前保持一致。

要求：不堆砌术语，讲清"为什么改 + 怎么改 + 为什么不会误伤"。

### 改动对象

按文件逐条列出，每个文件下用缩进的 `-` 子项列出具体改动点：

- 路径使用仓库相对路径（如 `src/chmod.c`，不要写绝对路径）。
- 每个子项聚焦一个可验证的改动点，写明"删除了什么 / 新增了什么 / 改成了什么"，避免笼统。
- 若多个文件改动性质相同，仍需分文件列出，不合并。

## 示例

修改思路：原有逻辑仅通过 /proc/cpuinfo 的 model name 判断 D3000M 平台并设置 video 参数，未区分整机厂商，无法定位长城整机。本次改为删除 video 参数（之前未支持 tty 中文而添加，内核已经支持因此删除），同时获取 DMI modalias（/sys/class/dmi/id/modalias，长城主板含 svnGreatWall）与 CPU model name，分别置位 IS_GREATWALL、IS_D3000M，二者同时成立才在 GRUB_CMDLINE_LINUX_OTHERS 后追加 usbcore.autosuspend=-1，避免影响非长城 D3000M 平台；追加时保留原有参数，非长城 D3000M 平台不添加参数，行为与改动前一致。

改动对象：
- util/grub.d/10_linux.in
  - 平台识别：删除原 CPU_INFO 单变量与 case 分支，改为读取 MODALIAS（DMI modalias）与 CPU_MODEL（cpuinfo 首条 model name/Hardware/Processor），经 grep -qi 匹配 svngreatwall、d3000m 分别置 IS_GREATWALL、IS_D3000M
  - 参数注入：两标志均为 1 时输出检测日志并以 "GRUB_CMDLINE_LINUX_OTHERS }usbcore.autosuspend=-1" 追加参数，否则输出未添加日志，不修改 GRUB_CMDLINE_LINUX_OTHERS
