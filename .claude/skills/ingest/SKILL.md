---
name: ingest
description: >
  处理讨论材料并整理进仓库。当用户提供讨论记录、白板照片、
  会议笔记、语音转录、或任何需要归档的项目讨论内容时使用。
  也处理 0-inbox/ 中的待整理内容。
allowed-tools: Read, Write, Edit, Bash(git:*)
---

# Ingest — 讨论材料处理流程

## 输入类型
- 文字讨论记录/总结
- 白板照片（描述或 OCR 内容）
- 语音转录
- inbox/ 中的零散想法
- 任意组合

## 处理步骤

### Step 1：理解输入
读取全部输入材料。识别：
- 这是一次讨论 session 还是一个零散想法？
- 参与者是谁？
- 大致时间？

如果是零散想法且很短（几句话），直接放 0-inbox/，commit with `idea:` 前缀，流程结束。

### Step 2：创建 1-log 文件
文件名格式：`1-log/YYYY-MM-DD-关键词.md`
使用模板：参考 templates/log.md

整理规则：
- 保留原始内容和措辞，不改写观点
- 补充结构（分段、标注发言者）
- 如果有图片，放入 `log/assets/` 并在 log 中引用
- 如果输入已经很结构化，不要过度整理

### Step 3：扫描并提取 2-open questions
从 1-log 中识别：
- 明确提出但未回答的问题
- 存在分歧的点
- 标记了"待讨论""TODO""?"的内容

对每个候选问题：
1. 检查 2-open/ 中是否已有相同或相似的问题（避免重复）
2. 如果是新问题，创建 2-open/ 文件，使用 templates/open.md
3. 文件名 = 问题的几个关键词（英文，kebab-case）

### Step 4：扫描并标记候选 3-decisions
从 1-log 中识别看起来已达成共识的选择。

**关键判断**：如果不确定这是否是共识——不要创建 decision 文件。
在 log 文件末尾标记为 `[pending confirmation]`，列出候选内容。
等参与者确认后再由 /commit 流程处理。

如果确定是共识（比如明确写了"我们决定..."）：
1. 确定下一个编号（读取 3-decisions/ 目录中的最大编号 +1）
2. 创建 3-decision 文件，使用 templates/decision.md
3. 在 1-log 文件末尾的 Outcomes 区域添加引用

### Step 5：交叉引用
- 检查新内容是否与已有 3-decisions 矛盾 → 如果是，开新 2-open question
- 检查新内容是否回答了某个 2-open question → 如果是，标注但不自行关闭
- 在 1-log 末尾写 Outcomes 区域，列出本次产生的所有 2-open 和 3-decision 文件

### Step 6：更新讨论状态快照
更新 `4-paper/living-claw.notes.md`，反映当前全局进展。
读取所有 3-decisions/ 和 2-open/ 文件，重新生成三个区域：
- **Decided** — 所有 accepted 3-decisions，每条一句话摘要 + 日期 + 文件链接
- **Open** — 所有 2-open questions，每条一句话描述 + 文件链接
- **Not Yet Discussed** — 已提及但尚未展开讨论的话题

### Step 7：报告
向用户输出处理摘要：
- 创建了哪些文件
- 标记了哪些 pending confirmation 项
- 发现了哪些与已有内容的关联或矛盾

不要自动 commit。等用户确认后交给 /commit。
