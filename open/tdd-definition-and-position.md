# TDD 的定义和位置是什么？

## 问题

两份材料对 TDD 的定义和位置存在矛盾。

### 材料A：之前讨论

- **定义**：Tech Design Document（技术设计文档）
- **位置**：Spec 之后
- **流程**：0 → Spec → TDD → Code → Test
- **生成方式**：专人在本地AI上生成，团队通过git协作达成一致

### 材料B：AI-Native Development Workflow

- **定义**：契约文档（Decisions + Contract + Rules + Examples + Done When + Not This）
- **位置**：Spec 之前
- **流程**：Human → TDD → AI(Spec → Code → Tests) → Verify
- **生成方式**：完全由人编写，AI不参与决策

## 需要回答

1. TDD 的正式定义是什么？
   - Tech Design Document？
   - 契约文档？
   - 两者是同一事物的不同阶段？

2. TDD 在流程中的位置？
   - Spec 之前（人写TDD，AI生成Spec）？
   - Spec 之后（Spec确定需求，再细化成TDD）？

3. 两种定义能否统一？
   - 七段式结构是否适用于 Tech Design Document？
   - 还是代表两种不同的文档？

## 背景

这个矛盾会影响整个流程设计，需要尽早确定。

## 状态

Open
