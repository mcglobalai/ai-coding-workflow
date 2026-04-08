# Living Claw 讨论仓库

## 这个仓库是什么
两个人围绕 Living Claw（silicon-based life 协议）的协作讨论记录。
不是代码仓库。Git history = 讨论的时间线。

## 目录结构和流向
0-inbox → 1-log → 2-open → 3-decisions → 4-paper

- 0-inbox/：随手想法，无格式要求，可删除
- 1-log/：讨论记录，按日期一文件，append-only
- 2-open/：未解决的问题，一文件一问题，可编辑
- 3-decisions/：锁定的决策，一文件一问题的答案，immutable
- 4-paper/：白皮书正文 + notes 编辑注释层

## 关键规则
- 3-decisions/ 中的文件一旦 commit 永不修改，只能 supersede
- 1-log/ 只追加不修改
- 不要 squash 或 rebase
- 4-paper 的每次实质修改应对应一个 3-decisions
- 不确定是否已达成共识的内容，不要放进 3-decisions，留在 1-log 里标记 pending

## Commit message 前缀
discuss: / decide: / draft: / question: / idea: / supersede: / close: / note:

## 处理讨论材料时的行为
参考 .claude/skills/ 下的对应 skill：
- 收到输入材料 → .claude/skills/ingest/SKILL.md
- 检查仓库状态 → .claude/skills/review/SKILL.md
- 准备提交 → .claude/skills/commit/SKILL.md

## 边界
- 可以：整理格式、拆文件、生成 commit message、发现问题和矛盾
- 不可以：替参与者做决策、修改 log 原始表述、直接改 paper（除非明确要求）
- 发现矛盾时：开一个新的 open question，不要自行解决
