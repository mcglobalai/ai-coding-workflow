---
name: review
description: >
  检查仓库当前状态。查看 2-open questions、pending confirmations、
  0-inbox 待处理项、以及 3-decisions 和 4-paper 之间的一致性。
allowed-tools: Read, Grep, Glob
---

# Review — 仓库状态检查

执行以下检查并输出报告：

## 1. Inbox 状态
- 0-inbox/ 中有多少待处理文件？列出。

## 2. 2-Open questions 状态
- 列出所有 2-open/ 中 Status: Open 的文件
- 按 Opened 日期排序
- 标注哪些超过 14 天未更新

## 3. Pending confirmations
- 扫描所有 1-log/ 文件，找 [pending confirmation] 标记
- 列出待确认的候选 3-decisions

## 4. 一致性检查
- 3-decisions/ 中 accepted 的内容是否已反映在 4-paper/ 中？
  （对比 3-decisions 列表和 4-paper 内容，标记 gap）
- 有没有 2-open questions 实际上已经被某个 3-decision 回答了但没有 close？
- 有没有 3-decision 之间存在矛盾？

## 5. 输出格式
简洁的状态报告，分为上述几个区块。
不要做任何修改，只报告。
