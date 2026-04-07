# 各阶段之间的 Handoff 接口如何定义？

## 问题

AI驱动开发分四阶段：0→Spec→TDD→Code→Test。每个阶段的输出如何成为下一阶段的输入？

## 需要回答

1. Spec 阶段输出的格式是什么？
   - 文档结构？
   - 必需字段？

2. TDD 阶段输出的格式是什么？
   - 与 Spec 的映射关系？

3. 各阶段间的 handoff package 应包含什么？
   - 背景？
   - 约束？
   - DoD？

## 相关

PPM Agent 的 handoff/packaging skill 可能适用于此。

## 状态

Open
