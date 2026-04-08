# PPM Agent 是否应扩展到前两阶段？

## 问题

PPM AI Agent 原定位是统筹 TDD→Code→Test 阶段。现在有意愿加入前两阶段（0→Spec、Spec→TDD）统计人的执行状态和计划。

## 需要回答

1. PPM Agent 在前两阶段的具体职责是什么？
   - 仅统计状态？
   - 还是参与调度/审核？

2. 需要哪些权限和能力扩展？
   - 读取 Spec 文档？
   - 读取讨论记录？
   - 触发审核流程？

3. 与 Claude Code / Codex 的边界在哪里？

## 背景

- 现有 PPM agent plugin 使用情况不佳
- 用户感知：前置步骤缺少、使用上有限制

## 状态

Open
