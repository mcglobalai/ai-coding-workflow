# 各阶段之间的 Handoff 接口如何定义？

## 问题

AI驱动开发分六阶段：`0 → Spec(需求分析) → TDD → Spec(开发需求) → Code → Test`

每个阶段的输出如何成为下一阶段的输入？

## 需要回答

1. **Spec(需求分析) 阶段输出格式？**
   - 文档结构？
   - 必需字段？
   - 谁负责编写？（人/Claude/Codex协作？）

2. **TDD 阶段输出格式？**
   - 是否采用七段式结构（Intent/Decisions/Contract/Rules/Examples/Done When/Not This）？
   - 与 Spec(需求分析) 的映射关系？

3. **Spec(开发需求) 阶段输出格式？**
   - 如何从 TDD 拆解？
   - 粒度如何确定？

4. **各阶段间的 handoff package 应包含什么？**
   - 背景？
   - 约束？
   - DoD？

## 已确定

- TDD 是协同核心
- Spec 是中间状态（非协同关键点）

## 相关

PPM Agent 的 handoff/packaging skill 可能适用于此。

## 状态

Open
