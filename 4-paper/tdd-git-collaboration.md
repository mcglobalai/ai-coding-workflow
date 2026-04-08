# TDD 阶段 Git 协同方案

> **版本**: v1.0
> **状态**: Draft
> **基于决策**: D001 (TDD定义), D002 (六阶段流程)

---

## 一、背景

在 AI 驱动开发的六阶段流程中：

```
0 → Spec(需求分析) → TDD → Spec(开发需求) → Code → Test
```

**TDD (Tech Design Document) 是协同核心**，团队需要在此阶段达成一致。Spec 是中间状态，非协同关键点。

本文档定义 TDD 阶段多人基于 git 协同的具体方案。

---

## 二、TDD 文档结构

### 2.1 标准格式

```
# [TDD-ID]: [标题]

Status: draft | exploring | review | approved | locked | deprecated
Owner: @username
Reviewers: [@username, ...]
Depends On: [TDD-ID, ...] (可选)
Deadline: YYYY-MM-DD (可选)

---

## Intent

为什么做这个。2-3 句话，说明背景和目标。

## Decisions

人做出的判断，AI 不能推翻。每条决策需说明理由。

1. **[决策点]**: [选择] — [为什么]

## Contract

输入输出边界。类型定义或接口声明。

    // 示例
    function process(input: InputType): OutputType

## Rules

可测试的行为规则。每条规则应对应一个测试用例。

1. [条件] → [行为]

## Examples

手写验证的校准用例。2-3 个，AI 将基于此扩展。

    Input: ...
    Output: ...

## Done When

验收标准清单。

- [ ] [标准1]
- [ ] [标准2]

## Not This

明确边界，防止 AI 越界扩展。

- NOT: [边界1]
- NOT: [边界2]
```

### 2.2 字段说明


| 字段         | 必填  | 说明         |
| ---------- | --- | ---------- |
| Status     | 是   | 当前状态，见状态流转 |
| Owner      | 是   | TDD 负责人    |
| Reviewers  | 是   | 评审人列表      |
| Depends On | 否   | 依赖的其他 TDD  |
| Deadline   | 否   | 交付时间约束     |


---

## 三、Git 协同流程

### 3.1 分支策略

```
main (已 approved/locked 的 TDD 集合)
  │
  ├── tdd/TDD-001-用户认证 ──→ PR #1 ──→ merge
  │
  ├── tdd/TDD-002-权限管理 ──→ PR #2 ──→ merge
  │
  └── tdd/TDD-003-日志系统 ──→ PR #3 ──→ review 中
```

**规则**：

- `main` 分支存放已 approved 和 locked 的 TDD
- 每个 TDD 一个分支：`tdd/<ID>-<简短描述>`
- 每个 TDD 一个 PR，便于追踪讨论

### 3.2 标准协作流程

```
┌─────────────────────────────────────────────────────────────┐
│  Owner                                                      │
│    │                                                        │
│    ├── 1. 本地 AI 生成 TDD 初稿                              │
│    │                                                        │
│    ├── 2. 创建分支，提交 TDD                                  │
│    │      git checkout -b tdd/TDD-001-xxx                   │
│    │      git add tdd/TDD-001.md                            │
│    │      git commit -m "tdd: draft TDD-001"                │
│    │      git push -u origin tdd/TDD-001-xxx                │
│    │                                                        │
│    ├── 3. 创建 PR，@Reviewers                               │
│    │                                                        │
│    └── 4. 根据 review 修改                                   │
│           git commit --amend 或新 commit                    │
│                                                             │
│  Reviewers                                                  │
│    │                                                        │
│    ├── 5. PR review，逐段评论                                │
│    │                                                        │
│    └── 6. approve 或 request changes                        │
│                                                             │
│  Merge                                                      │
│    │                                                        │
│    └── 7. merge 到 main，TDD 进入 approved 状态              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 3.3 Commit Message 约定


| 前缀               | 用途           | 示例                                          |
| ---------------- | ------------ | ------------------------------------------- |
| `tdd:`           | TDD 文档变更     | `tdd: draft TDD-001`                        |
| `tdd-review:`    | 根据 review 修改 | `tdd-review: address comments on Decisions` |
| `tdd-lock:`      | 锁定 TDD       | `tdd-lock: TDD-001 开发开始`                    |
| `tdd-supersede:` | 废弃并替代        | `tdd-supersede: TDD-001 by TDD-001-v2`      |


---

## 四、状态流转

### 4.1 状态定义


| 状态           | 含义             | 可转换到                   |
| ------------ | -------------- | ---------------------- |
| `exploring`  | 探索中，方案未定       | `draft`                |
| `draft`      | 起草中，可自由修改      | `review`, `deprecated` |
| `review`     | PR opened，等待评审 | `approved`, `draft`    |
| `approved`   | 已合并 main，开发前   | `locked`, `deprecated` |
| `locked`     | 开发已开始，禁止修改     | `deprecated`           |
| `deprecated` | 已废弃            | —                      |


### 4.2 状态流转图

```
                    ┌──────────────┐
                    │  exploring   │  (方案未定，可迭代)
                    └──────┬───────┘
                           │ 方案稳定
                           ▼
                   ┌──────────────┐
         ┌─────────│    draft     │◄────────────┐
         │         └──────┬───────┘             │
         │                │ 创建 PR              │
         │                ▼                     │
         │         ┌──────────────┐             │
         │         │    review    │─── request ─┘
         │         └──────┬───────┘   changes
         │                │ approve
         │                ▼
         │         ┌──────────────┐
         │         │   approved   │
         │         └──────┬───────┘
         │                │ 开发开始
         │                ▼
         │         ┌──────────────┐
         │         │    locked    │ (不可逆)
         │         └──────┬───────┘
         │                │
         └────────────────┴──────────► deprecated
                                       (任何阶段都可废弃)
```

### 4.3 状态变更规则

- **draft → review**：创建 PR 时自动变更
- **review → approved**：所有 Reviewers approve 后 merge
- **review → draft**：有 request changes，PR 关闭或修改后重新 review
- **approved → locked**：开发工作开始，Owner 提交 `tdd-lock:` commit
- **locked 后禁止修改**：如需变更，必须创建新 TDD supersede

---

## 五、高频场景处理（重点细化）

### 5.1 依赖关系 ⭐高频

#### 5.1.1 场景描述

TDD-A 依赖 TDD-B。常见情况：

- 基础设施 TDD → 业务功能 TDD
- 公共组件 TDD → 使用方 TDD
- API 定义 TDD → 客户端 TDD

#### 5.1.2 处理流程

```
TDD-B (基础设施)
    │
    ├── draft ──→ review ──→ approved ──→ locked
    │                                      │
    │                                      │ 依赖就绪
    │                                      ▼
    └─── TDD-A (业务功能) ──→ draft ──→ review ──→ approved
                              (可提前起草，但不可 approved)
```

#### 5.1.3 详细规则

**1. 依赖声明**

```markdown
# TDD-A: 用户认证功能

**Depends On**: TDD-B (数据库连接池)
```

**2. 依赖检查（自动化）**

```
PR 创建时自动检查：
- 若 Depends On 中的 TDD 未 approved → 阻止 merge
- 若 Depends On 中的 TDD 状态变更 → 通知 Owner
```

**3. 依赖方评审**

- 依赖 TDD 的 Reviewers 中必须包含被依赖 TDD 的 Owner
- 或明确标注：`Reviewers: [@xxx, @yyy (依赖方) ]`

**4. 依赖变更处理**

```
场景：TDD-B 已 locked，但发现需要修改接口

流程：
1. TDD-B Owner 创建 TDD-B-v2
2. 标注 Supersedes: TDD-B
3. 通知所有依赖方（TDD-A, TDD-C, ...）
4. 依赖方评估影响，更新各自的 TDD
5. TDD-B-v2 approved 后，依赖方才能继续推进
```

**5. 循环依赖检测**

```
禁止：
TDD-A Depends On TDD-B
TDD-B Depends On TDD-A

自动化检查：PR 创建时检测循环依赖，阻止 merge
```

#### 5.1.4 依赖关系可视化

建议维护 `dependencies.md` 或使用工具生成依赖图：

```
TDD-001 (基础设施-数据库)
    ├── TDD-003 (用户认证)
    │       └── TDD-007 (权限管理)
    └── TDD-004 (日志系统)

TDD-002 (基础设施-缓存)
    └── TDD-005 (会话管理)
```

---

### 5.2 TDD 拆分 ⭐高频

#### 5.2.1 场景描述

单个 TDD 发现过大，需要拆分成多个子 TDD。触发条件：

- Decisions 超过 10 条
- Contract 涉及多个独立模块
- 预估开发周期超过 2 周

#### 5.2.2 处理流程

```
TDD-X (过大)
    │
    ├── 发现需要拆分 (review 阶段)
    │
    ├── 创建父 TDD
    │       TDD-X (父)
    │       Status: split
    │       Children: [TDD-X-1, TDD-X-2, TDD-X-3]
    │
    ├── 创建子 TDD
    │       TDD-X-1
    │       Parent: TDD-X
    │       Status: draft
    │
    │       TDD-X-2
    │       Parent: TDD-X
    │       Status: draft
    │
    │       TDD-X-3
    │       Parent: TDD-X
    │       Status: draft
    │
    └── 所有子 TDD approved 后
            父 TDD Status → done
```

#### 5.2.3 拆分原则

**1. 按边界拆分**

```
优先按 Contract 边界拆分：
- 每个子 TDD 有独立的 Contract
- 子 TDD 之间尽量无循环依赖

避免按"任务"拆分：
- 不要拆成"开发"、"测试"、"部署"
- TDD 本身已包含这些考量
```

**2. 父 TDD 保留什么**

```markdown
# TDD-X: 用户系统 (父 TDD)

**Status**: split
**Children**: [TDD-X-1, TDD-X-2, TDD-X-3]

## Intent
(保留，作为整体背景)

## Decisions
(保留全局决策，子 TDD 各自细化)

## Contract
(保留顶层接口，子 TDD 实现细节)

## Not This
(保留全局边界)
```

**3. 子 TDD 继承什么**

```markdown
# TDD-X-1: 用户注册

**Parent**: TDD-X
**Status**: draft

## Intent
(继承父 TDD，补充子模块说明)

## Decisions
(细化本模块的决策，引用父 TDD 全局决策)

## Contract
(本模块的具体接口)
```

#### 5.2.4 子 TDD 协同

```
并行开发：
- 子 TDD 可并行推进 review 流程
- 有依赖关系的子 TDD 按 5.1 处理

状态同步：
- 父 TDD 自动跟踪子 TDD 状态
- 所有子 TDD approved → 父 TDD 可标记 done
- 任一子 TDD blocked → 父 TDD 标注阻塞原因
```

---

### 5.3 紧急变更（已 locked）⭐高频

#### 5.3.1 场景描述

TDD 已 locked（开发已开始），但发现：

- Decisions 有重大错误
- Contract 接口定义不合理
- 需求发生重大变更

#### 5.3.2 核心原则

```
原则：locked 的 TDD 禁止直接修改

原因：
1. 开发已开始，修改会影响已完成的工作
2. 需要追溯变更历史
3. 需要评估影响范围
```

#### 5.3.3 处理流程

```
┌─────────────────────────────────────────────────────────────┐
│  发现 locked TDD 需要修改                                    │
│    │                                                        │
│    ├── 1. 创建新 TDD                                         │
│    │      TDD-001-v2                                        │
│    │      Status: draft                                     │
│    │      Supersedes: TDD-001                               │
│    │      Reason: [变更原因]                                 │
│    │                                                        │
│    ├── 2. 标注原 TDD                                         │
│    │      在 TDD-001 顶部添加：                              │
│    │      **Superseded by: TDD-001-v2**                     │
│    │      **Supersede Reason: [原因]**                      │
│    │                                                        │
│    ├── 3. 影响评估                                           │
│    │      - 列出已完成的工作                                  │
│    │      - 评估需要回滚/修改的范围                            │
│    │      - 记录在 TDD-001-v2 的 Impact Assessment 段        │
│    │                                                        │
│    ├── 4. 走标准 review 流程                                 │
│    │      Reviewers 必须包含：                               │
│    │      - 原 TDD Owner                                    │
│    │      - 已开发部分的 Owner                                │
│    │      - 依赖方 TDD Owner                                 │
│    │                                                        │
│    └── 5. approved 后执行变更                                │
│           - 开发团队根据 Impact Assessment 调整计划            │
│           - 依赖方评估是否需要同步修改                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### 5.3.4 影响评估模板

```markdown
## Impact Assessment (TDD-001-v2 新增段)

### 变更内容
- [具体变更点]

### 影响范围
- 已完成代码：[文件/模块列表]
- 已完成测试：[测试列表]
- 依赖方 TDD：[TDD-ID 列表]

### 修复方案
- [ ] 代码回滚/修改
- [ ] 测试更新
- [ ] 通知依赖方

### 工作量评估
- 预估额外工时：[X 人日]
```

#### 5.3.5 紧急变更的权限

```
谁可以发起紧急变更：
- 原 TDD Owner
- 技术负责人
- PM（仅需求变更）

必须获得批准：
- 所有 Reviewers re-approve
- 技术负责人签字
- （可选）涉及架构变更需架构师同意
```

---

### 5.4 并行冲突 ⭐高频

#### 5.4.1 场景描述

多个 TDD 并行开发，存在交集：

- Contract 定义了相同/相似的接口
- Decisions 对同一问题有不同判断
- 共享依赖同一基础设施 TDD

#### 5.4.2 冲突类型


| 类型           | 表现                   | 严重程度 |
| ------------ | -------------------- | ---- |
| Contract 冲突  | 两个 TDD 定义了同名但不同签名的函数 | 高    |
| Decisions 冲突 | 两个 TDD 对同一技术选型有不同决策  | 高    |
| 依赖版本冲突       | 依赖同一 TDD 但期望不同版本     | 中    |
| 命名冲突         | 使用了相同命名但含义不同         | 低    |


#### 5.4.3 检测机制

**1. PR 创建时自动检测**

```
检测逻辑：
- 扫描所有 draft/review 状态的 TDD
- 对比 Contract 字段（函数名、类型名）
- 对比 Decisions 字段（关键词）
- 发现潜在冲突 → 在 PR 中自动评论警告
```

**2. Reviewer 人工检查**

```
Reviewers 在 review 时需检查：
- 是否与其他 TDD 有交集
- 交集部分的 Decisions 是否一致
- 是否需要抽出公共 TDD
```

#### 5.4.4 处理流程

```
发现冲突
    │
    ├── 情况A：冲突轻微
    │       - 协商调整其中一个 TDD
    │       - 在 PR comment 中记录协商结果
    │       - 继续推进
    │
    ├── 情况B：冲突严重，需要公共定义
    │       - 抽出公共 TDD
    │       - 原 TDD 依赖公共 TDD
    │       - 公共 TDD 优先 approved
    │
    └── 情况C：冲突无法调和
            - 升级到技术负责人仲裁
            - 或在团队会议讨论
            - 记录决策理由
```

#### 5.4.5 优先级规则

当冲突无法同时满足时：

```
优先级排序：
1. 已 approved/locked 的 TDD 优先
2. 有外部依赖约束的 TDD 优先
3. Deadline 更早的 TDD 优先
4. 更基础的 TDD 优先（基础设施 > 业务）
5. Owner 协商或升级仲裁
```

---

## 六、其他场景处理

### 6.1 TDD 合并

**触发条件**：多个 TDD 发现关联紧密，分开维护成本高

**处理流程**：

1. 创建合并 TDD（如 TDD-ABC）
2. 原 TDD 标注 `Status: merged into TDD-ABC`
3. 原 TDD 分支关闭
4. 合并 TDD 走标准 review 流程

### 6.2 TDD 废弃

**触发条件**：需求变更、技术方案调整

**处理流程**：

1. Status 改为 `deprecated`
2. 添加废弃原因：`Deprecated Reason: [原因]`
3. 若已有开发进度，评估回滚范围
4. 通知依赖方

### 6.3 跨团队协作

**场景**：Team A 的 TDD 依赖 Team B 的 TDD

**处理流程**：

1. Reviewers 包含跨团队代表
2. 依赖方 Team 需在 PR 中 approve
3. 明确 handoff 时间点和责任人
4. 可使用 `Deadline` 字段约束

### 6.4 探索性开发

**场景**：技术方案不确定，需要边探索边定义

**处理流程**：

1. 创建 `Status: exploring` 的 TDD
2. 可多次迭代修改 Decisions/Contract
3. 方案稳定后转为 `draft`
4. `exploring` 状态不可进入开发

### 6.5 外部依赖

**场景**：TDD 依赖外部系统/第三方服务

**处理**：

- `Depends On` 标注外部依赖（非 TDD 格式）
- 增加风险评估段
- 外部接口变更时触发重新 review

---

## 七、工具支持建议

### 7.1 自动化检查


| 检查项         | 触发时机  | 动作                         |
| ----------- | ----- | -------------------------- |
| 依赖状态        | PR 创建 | 检查 Depends On 是否已 approved |
| 循环依赖        | PR 创建 | 检测并阻止                      |
| Contract 冲突 | PR 创建 | 扫描并警告                      |
| 状态一致性       | 状态变更  | 更新依赖图                      |


### 7.2 文档生成

```
自动生成：
- TDD 列表（按状态分类）
- 依赖关系图
- 变更历史（通过 git log）
```

### 7.3 通知机制

```
通知场景：
- TDD 状态变更 → 通知 Owner 和 Reviewers
- 依赖 TDD 变更 → 通知所有依赖方
- PR 新评论 → 通知相关人
```

---

## 八、附录

### A. TDD 模板

```
# [TDD-ID]: [标题]

Status: draft
Owner: @username
Reviewers: [@reviewer1, @reviewer2]
Depends On: (可选)
Deadline: (可选)

---

## Intent

为什么做这个。2-3 句话。

## Decisions

1. **[决策点]**: [选择] — [为什么]

## Contract

    // 输入输出定义

## Rules

1. [条件] → [行为]

## Examples

    Input: ...
    Output: ...

## Done When

- [ ] [标准1]
- [ ] [标准2]

## Not This

- NOT: [边界]
```

### B. 状态变更 Checklist

**draft → review**:

- 所有必填字段已填写
- Decisions 有明确理由
- Contract 定义清晰
- 已创建 PR

**review → approved**:

- 所有 Reviewers 已 approve
- 依赖 TDD 已 approved
- 无未解决的冲突

**approved → locked**:

- 开发工作已分配
- 开发团队已确认理解 TDD

### C. 相关决策

- D001: TDD = Tech Design Document
- D002: 六阶段流程

---

**文档历史**


| 版本   | 日期         | 变更  |
| ---- | ---------- | --- |
| v1.0 | 2026-04-08 | 初版  |


