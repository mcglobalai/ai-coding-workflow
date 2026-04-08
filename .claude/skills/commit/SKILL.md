---
name: commit
description: >
  将当前工作区的变更按规则分步 commit。
  遵循仓库的 commit message 约定和分步提交规则。
allowed-tools: Read, Bash(git:*)
---

# Commit — 分步提交规则

## 原则
- 一个 commit 做一件事
- 不同目录的变更分开 commit
- commit message 必须带前缀

## 提交顺序
如果一次 ingest 产生了多种变更，按以下顺序提交：

1. **1-log 文件**（含 assets）
   - `git add 1-log/`
   - `discuss: 简短描述讨论主题`

2. **2-open questions**
   - `git add 2-open/`
   - `question: 列出新增的问题关键词`

3. **3-decisions**
   - 每个 decision 单独一个 commit
   - `git add 3-decisions/NNN-xxx.md`
   - `decide: NNN - 问题的简短描述`

4. **4-paper 修改**（如果有）
   - `git add 4-paper/`
   - `draft: 修改了什么`

5. **0-inbox 内容**（如果有）
   - `git add 0-inbox/`
   - `idea: 简短描述`

## Commit message 格式
`前缀: 简短描述（中英文均可，50字以内）`

前缀表：
- discuss: — log 增改
- decide: — 新 decision
- draft: — paper 正文
- question: — 新 open question
- idea: — inbox 内容
- supersede: — 替代旧 decision（message 中注明被替代的编号）
- close: — 关闭 open question
- note: — paper notes 修改

## 执行前确认
列出将要执行的所有 commit（文件 + message），等用户确认后再执行。
不要自动 push。commit 完成后提醒用户 `git push`。
