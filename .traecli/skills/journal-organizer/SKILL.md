---
name: "journal-organizer"
description: "Organizes Markdown notes by standardizing format and generating overview. Invoke when user wants to organize daily notes, standardize journal format, or create a README summary of notes."
---

# Journal Organizer

This skill organizes Markdown notes in a specified directory according to a standard template and generates an overview README file.

## When to Invoke

- User wants to organize daily work notes
- User wants to standardize journal format to match a template
- User wants to create a README summary of all notes
- User wants to batch-process multiple Markdown files

## Prerequisites

1. A standard template file exists at `模板/每日笔记模板.md`
2. Target directory contains Markdown files to organize
3. User provides the target directory path

## Workflow

### Step 1: Read Template

Read the standard template from `模板/每日笔记模板.md` to understand the expected structure:

- **💼 今日工作**: Task list with standardized format
- **📝 工作详情与备忘**: Detailed work records including:
  - 🎯 任务处理记录
  - 🐛 Bug处理记录
  - 📞 会议/沟通记录
  - 💡 技术发现
- **📊 今日统计**: Dataview auto-generated statistics

### Step 2: Read All Markdown Files

Scan the specified target directory recursively and read all `.md` files. Skip:
- Hidden files (starting with `.`)
- Template files in `模板/` directory
- Assets directories (like `assets/`)

### Step 3: Organize Each Note

For each note, apply the following standardization:

#### Task Format Standardization
- Ensure tasks follow: `- #task-xxx 任务标题 #项目/项目名 #类型/类型名 #状态/状态名 #优先级/优先级 [Xh]`
- Add missing tags: `#状态/xxx`, `#优先级/xxx`
- Fix format issues (spacing, ordering)
- **Important spacing rules**: 
  - `#` tags must be preceded by a space (e.g., `任务标题 #项目/xxx`, not `任务标题#项目/xxx`)
  - Chinese colon `：` must be followed by a space (e.g., `**状态**： #状态/进行中`, not `**状态**：#状态/进行中`)

#### Structure Standardization
- Reorganize "工作详情与备忘" section to group by task type:
  - 🎯 任务处理记录 for #task-xxx items
  - 🐛 Bug处理记录 for #bug-xxx items
- Use standardized subsections:
  - **核心问题** / **问题描述**
  - **进展** / **分析进展**
  - **技术细节** / **解决方案**
  - **关联信息**
  - **下一步**

### Step 4: Generate Overview README

Create a `README.md` file in the target directory with:

#### Summary Statistics
- Total number of notes
- Date range (first to last note)
- Total hours worked
- Task distribution by project
- Task distribution by type

#### Project Summary
List of all projects with:
- Number of tasks
- Total hours
- Key tasks

#### Recent Activity
Last 7 days of notes with brief summaries

#### Navigation
Table of contents with links to each note

## Example Usage

### Input
```
请帮我整理 2026/06 目录下的所有日报，并生成总览 README
```

### Output
1. All `.md` files in `2026/06/` are standardized to match `模板/每日笔记模板.md`
2. `2026/06/README.md` is created with:
   - Monthly statistics
   - Project summaries
   - Recent activity
   - Note navigation links

## Tag Standards

### Status Tags
- `#状态/待处理`: Not started
- `#状态/进行中`: In progress
- `#状态/待验证`: Waiting for verification
- `#状态/阻塞`: Blocked
- `#状态/已完成`: Completed
- `#状态/暂停`: Suspended

### Priority Tags
- `#优先级/高`: High priority
- `#优先级/中`: Medium priority
- `#优先级/低`: Low priority

### Type Tags
- `#类型/开发`: Development
- `#类型/测试`: Testing
- `#类型/分析`: Analysis
- `#类型/沟通`: Communication
- `#类型/文档`: Documentation

## Notes

- Always preserve original content when reorganizing
- Add missing tags based on context analysis
- Generate README in the same directory as the notes
- Use relative links in README for navigation
- **Spacing rules**: Ensure `#` tags are preceded by a space and Chinese colon `：` is followed by a space
