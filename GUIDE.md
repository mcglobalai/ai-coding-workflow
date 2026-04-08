# Living Claw — 合作者说明书

## 这个仓库怎么用（一句话）

往仓库里扔讨论材料，让 Claude Code 或者你自己按规则整理成结构化记录。

## 五个目录的关系

```
0-inbox（随手想法）
  ↓ 整理
1-log（讨论记录）
  ↓ 提取
2-open（未解决的问题）
  ↓ 讨论达成共识
3-decisions（锁定的决策）
  ↓ 吸收进
4-paper（白皮书正文）
```

每个目录的一句话定义：

- **0-inbox** — 任何时候任何人的任何想法。无格式要求。唯一允许删除内容的地方。
- **1-log** — 一次讨论 session 一个文件。Append-only，不改已有内容。
- **2-open** — 一个文件 = 一个可以被明确回答的问题。可自由编辑。
- **3-decisions** — 一个文件 = 一个问题的确定答案。Immutable。只能被新 decision supersede。
- **4-paper** — 白皮书正文。只有 accepted decisions 的内容才进 paper。

## Immutability 规则

- `3-decisions/` 中的文件一旦 commit，永不修改正文。如果决策变了，创建新文件，旧文件顶部加 `Status: Superseded by XXX`。
- `1-log/` 中的文件只追加不修改。
- 永远不要 squash 或 rebase。merge only。

## Commit 约定

前缀：

- `discuss:` — log 文件的增改
- `decide:` — 新 decision 入库
- `draft:` — paper 正文修改
- `question:` — 新 open question
- `idea:` — inbox 内容
- `supersede:` — 替代旧 decision
- `close:` — 关闭 open question
- `note:` — paper notes 修改

## 日常操作场景

### 场景 A：正式讨论后

1. 把讨论材料（笔记、白板照片、录音总结）交给 Claude Code，运行 `/ingest`
2. Claude 整理成 1-log + 拆出 2-open questions + 标记疑似 3-decisions
3. 你 review `git diff`，确认后 push

### 场景 B：某人有个想法

1. 在 `0-inbox/` 里创建一个文件，写下想法，commit with `idea: 简短描述`
2. 下次整理时 `/ingest` 会处理 0-inbox

### 场景 C：不用 Claude Code

1. 手动创建/编辑文件（按目录编号顺序）
2. 按 commit 约定写 message
3. push

所有场景都有效。Claude Code 是加速器，不是必需品。

## 快速开始

```bash
git clone <repo-url>
cd living-claw
# 安装 Claude Code（如果要用）
npm install -g @anthropic-ai/claude-code
# 启动
claude
# 扔进讨论材料
/ingest "昨天我们讨论了 X，结论是 Y，还有 Z 没想清楚"
```
