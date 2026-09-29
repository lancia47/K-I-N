# K-I-N 社会因果系统规格 v0.1

**版本：v0.1｜2026-09-29**  
**性质：系统规格 / Social Causality System**  
**上游：KIN Game Project Design v0.5**

---

# 0. 系统目标

本系统解决的不是“让 NPC 更会聊天”，而是：

> **让社会关系成为可积累、可传播、可追责、可反作用于未来选择的世界状态。**

核心链：

```text
Event
→ Perception
→ Memory
→ Belief
→ Relationship
→ Identity
→ Organization
→ Choice
→ New Event
```

如果一段经历只改变 NPC 下一句台词，而不改变后续选择，本系统视为失败。

---

# 1. 基础原则

## 1.1 社会事实与社会解释分离

WorldCore 保存：

```text
发生了什么
谁参与
谁在场
资源如何变化
谁受伤 / 获益 / 失去
```

Agent 保存：

```text
我看见了什么
我听说了什么
我相信什么
我如何解释
我因此怎么看这个人 / 家族 / 组织
```

两者不能混写。

## 1.2 NPC 不拥有全知社会图

一个 NPC 不能因为系统数据库里存在某个家族，就自动知道：

- 该家族成员是谁
- 该家族做过什么
- 谁是首领
- 它是否危险

这些信息必须通过：

- 亲眼观察
- 对话传播
- 标志识别
- 长期接触
- 公开事件
- 组织制度

进入自身 Belief State。

## 1.3 社会影响必须可追溯

每次重要关系变化，都要能回答：

```text
为什么变？
由哪个事件触发？
这个人知道了事件的哪一部分？
信息来源是谁？
来源有多可信？
变化的是哪一个关系维度？
这个变化后来影响了哪次选择？
```

---

# 2. Event：社会因果的原子输入

## 2.1 Event 不是“剧情句子”

社会系统只消费已经由 WorldCore 结算的事件。

示例：

```yaml
event:
  id: evt_204
  tick: 8932
  type: refuse_food_request
  actor: npc_a
  target: npc_b
  location: camp_center
  context:
    famine_level: 0.8
  preconditions:
    actor_food: 1
    target_food: 0
  result:
    food_transferred: 0
  direct_witnesses:
    - npc_c
```

## 2.2 第一批社会事件类型

MVP 只需要支持高 Historical Leverage 事件：

- 请求
- 接受
- 拒绝
- 赠与
- 借出
- 偿还
- 违约
- 交换
- 帮助
- 抛弃
- 伤害
- 防御
- 救助
- 威胁
- 承诺
- 违背承诺
- 加入身份
- 放弃身份

不优先做大量低后果闲聊事件。

---

# 3. Perception：每个人看到的不是同一个事件

## 3.1 直接参与者

Actor / Target 通常拥有最高信息密度，但依然不等于全知。

例如 B 向 A 借粮：

B 知道：

```text
我请求了
A 拒绝了
```

B 未必知道：

```text
A 只剩最后一份粮
```

## 3.2 现场目击者

目击者只能获得可观察部分：

```yaml
perceived_event:
  source_event: evt_204
  observer: npc_c
  channel: direct_visual
  known:
    actor: npc_a
    target: npc_b
    action: refuse_food_request
  unknown:
    actor_inventory: true
    actor_private_beliefs: true
```

## 3.3 二手传播

二手消息必须记录传播链：

```text
World Event
→ C 目击
→ C 告诉 D
→ D 形成 Belief
```

D 不应获得原始 Event 的完整内容。

---

# 4. Memory：记住什么，而不是保存一切

## 4.1 Salience Score

建议第一版采用可解释的显著度分数：

```text
Salience =
SurvivalImpact
+ EmotionalImpact
+ RelationshipRelevance
+ IdentityRelevance
+ Novelty
+ Irreversibility
+ ExpectationViolation
```

MVP 不追求数学最优，只要求稳定、可调、可解释。

## 4.2 三层记忆

### Working Memory
当前事件和短时间窗口。

### Episodic Memory
保留具体关键事件：

```text
Day 12：饥荒时 A 拒绝给我食物。
```

### Compressed Social Memory
将多次经历压缩为稳定判断：

```text
A 在资源紧张时通常不愿帮助我。
```

## 4.3 遗忘不等于删除历史

History Ledger 永久保存世界事实。

NPC Memory 可以：

- 衰减
- 模糊
- 被新事件覆盖
- 保留结论而丢失细节

因此可能出现：

```text
“我不太信任 A，但已经记不清具体从什么时候开始。”
```

这是允许的。

---

# 5. Belief：社会传播的是认知，不是事实

## 5.1 Belief 基本结构

```yaml
belief:
  subject: npc_a
  predicate: selfish_under_scarcity
  value: true
  confidence: 0.62
  source_type: hearsay
  source_agent: npc_c
  source_event: evt_204
  acquired_tick: 9100
```

## 5.2 Confidence 不等于 Truth

高置信度也可能是错的。

允许：

```text
错误 + 高置信
正确 + 低置信
相互冲突的多个解释
```

## 5.3 最小传播规则

第一版只需要三种来源：

```text
亲眼看到
可信对象告知
不可信对象告知
```

之后再扩展：

- 群体重复
- 权威背书
- 利益相关偏差
- 恐惧放大
- 身份偏见
- 宣传

## 5.4 传播必须发生失真

消息不是无损复制。

二手传播至少可改变：

- 置信度
- 细节完整度
- 因果解释
- 情绪标签

但 MVP 中不要随机生成复杂谣言文本；先结构化地丢失信息。

---

# 6. Relationship：关系不是“好感度”

## 6.1 多维关系状态

第一版建议：

```yaml
relationship:
  trust: 0.0
  fear: 0.0
  respect: 0.0
  affection: 0.0
  resentment: 0.0
  obligation: 0.0
  familiarity: 0.0
```

范围可以统一为 `[-1, 1]` 或 `[0, 1]`，具体实现后定。

## 6.2 不同维度不能强行合并

NPC 可以同时：

```text
Affection = -0.6
Trust = 0.1
Fear = 0.9
Respect = 0.8
Obligation = 0.7
```

因此行为不能由：

```text
if friendship > 70
```

简单决定。

## 6.3 关系更新来源

关系变化必须来自结构化事件与 Belief：

```text
救命
→ obligation ↑
→ trust ↑

公开羞辱
→ resentment ↑
→ respect 可能 ↓

强力报复
→ fear ↑
→ respect 可能 ↑ 或 ↓
```

同一事件对不同主体的更新可以不同。

## 6.4 关系衰减

不同维度有不同稳定性：

- familiarity：长期接触累积，缓慢衰减
- affection：中速变化
- trust：重大事件可快速变化
- obligation：直到偿还 / 解除前保持
- resentment：可以长期保存
- fear：随威胁持续性变化

---

# 7. Reputation：个人、家族与组织分离

## 7.1 三种声誉对象

```text
Person Reputation
House Reputation
Faction Reputation
```

任何 NPC 都可以分别持有三套 Belief。

## 7.2 声誉不是全局数值

禁止：

```text
House Varen reputation = 80
```

然后所有 NPC 自动读取。

正确结构是：

```text
NPC_C believes House Varen is dangerous (0.7)
NPC_D believes House Varen protects allies (0.5)
NPC_E has never heard of House Varen
```

## 7.3 声誉迁移

个人行为影响集体声誉，需要 Attribution Weight：

```text
CollectiveImpact =
IdentityVisibility
× MemberRepresentativeness
× EventSeverity
× InformationSpread
```

例如普通成员一次小冲突，不应立刻定义整个家族。

家主公开处决盟友，则可能产生很高集体声誉影响。

---

# 8. Identity：姓氏作为社会契约

## 8.1 姓氏不是字符串

```text
name: Arlen Varen
```

只有当世界中存在对应的社会关系，`Varen` 才是身份。

至少包括：

```yaml
identity_membership:
  identity: house_varen
  member: npc_arlen
  recognition:
    self: true
    internal: true
  obligations:
    defense: expected
    resource_support: optional
  privileges:
    protection: expected
```

## 8.2 获得姓氏的方式

未来可以包含：

- 血缘
- 收养
- 婚姻
- 宣誓
- 授姓
- 政治加入

MVP 只实现：

```text
玩家授予
+
NPC 接受
+
其他成员承认
```

## 8.3 姓氏的可玩意义

当玩家 / NPC 报出姓氏时，对方不是响应固定台词，而是查询：

```text
我听说过这个姓氏吗？
我相信什么？
消息从哪里来？
我怎么看这个家族？
这个人看起来真的是成员吗？
当前我有多大风险？
```

从而可能出现：

- 威慑
- 尊重
- 怀疑
- 寻求庇护
- 报复
- 拒绝交易
- 主动合作
- 毫无反应（从未听过）

---

# 9. Recognition：组织被世界承认出来

## 9.1 Create Faction 不是组织存在的充分条件

玩家可以命名某个团体，但名称本身不制造社会现实。

组织稳定度可以粗略由以下因素构成：

```text
InternalRecognition
+ SharedIdentity
+ RepeatedCoordination
+ ResourceCoupling
+ MutualObligation
+ ExternalRecognition
+ Persistence
```

## 9.2 最小组织状态

```yaml
organization:
  id: house_varen
  name: Varen
  members:
  roles:
  shared_resources:
  obligations:
  internal_recognition:
  external_beliefs:
  established_tick:
```

## 9.3 组织瓦解

组织不是永久对象。

当：

- 内部承认下降
- 共同义务持续失效
- 成员大量退出
- 资源结构解体
- 外部不再识别

可以出现：

```text
稳定组织
→ 派系争夺
→ 分裂
→ 两个新组织
```

---

# 10. Micro–Macro Continuity

## 10.1 设计目标

传统策略游戏常出现：

```text
宏观层：你是国王
微观层：街上的 NPC 完全不认识你
```

K-I-N 需要保证：

```text
个人历史
→ 集体声誉
→ 宏观结构
→ 微观相遇反馈
```

## 10.2 认识一个“国王”也是认知过程

普通 NPC 是否知道玩家身份，由：

```text
直接见过？
见过画像 / 标志？
知道姓氏？
知道护卫制服？
当地消息传播程度？
他关心政治吗？
```

共同决定。

因此“高身份”不会自动获得全图识别。

---

# 11. 群体命令：证明 NPC 不是工蜂

## 11.1 命令只是一种社会输入

玩家下达：

```text
去伐木。
```

对 NPC 来说应被翻译为：

```yaml
request:
  issuer: player
  action: gather_wood
  urgency: medium
  context: winter_preparation
```

然后 NPC 自己判断。

## 11.2 决策输入

```text
Need
+ Goal
+ Relationship(player)
+ Obligation
+ Identity Duty
+ Expected Reward
+ Risk
+ Alternative Tasks
+ Belief about Winter
```

## 11.3 第一版允许的结果

- 接受
- 拒绝
- 延迟
- 要求报酬
- 因义务优先执行

暂不需要复杂“假装答应后偷懒”机制。

---

# 12. 数据流

完整社会因果数据流：

```text
[WorldCore Event]
      ↓
[Perception]
      ↓
[Perceived Event]
      ↓
[Salience]
      ↓
[Memory]
      ↓
[Belief Update]
      ↓
[Relationship Update]
      ↓
[Identity / Reputation Update]
      ↓
[Jev Decision Context]
      ↓
[Action Intent]
      ↓
[WorldCore]
      ↓
[New Event]
```

其中 WorldCore 是唯一能够最终提交世界事实变化的组件。

---

# 13. MVP 测试用例

## Test A：直接记忆

```text
B 向 A 借粮
A 拒绝
B Trust(A) 下降
```

要求：下降可追溯到 event_id。

## Test B：目击者认知不足

C 看见拒绝，但不知道 A 库存。

要求：C 可形成与 B 不同的 Belief。

## Test C：二手传播

C 告诉 D。

要求：D 获得较低置信度，且 source_chain 可追溯。

## Test D：二次选择

再次缺粮。

要求：B 不再优先找 A，且原因可从 Relationship / Memory 找到。

## Test E：姓氏传播

B 加入 Varen。

D 从未见过 B，但已听过 Varen。

要求：D 第一次见 B 时，根据其对 Varen 的 Belief 改变互动。

## Test F：未知身份

E 从未听说 Varen。

要求：Varen 身份对 E 不产生直接社会加成。

## Test G：集体声誉不等于个人

Varen 家族声誉危险，但 B 本人曾救过 D。

要求：D 的决策同时读取 Person 与 House 两个层级，而不是直接覆盖。

## Test H：不是工蜂

玩家要求 6 人采木。

要求：不同 NPC 基于不同关系 / 需求得到不同响应。

---

# 14. 失败模式

以下任何一种出现，都说明系统方向偏离：

### 失败 1：Memory = 无限聊天记录

### 失败 2：Relationship = 单一好感度

### 失败 3：Reputation = 全局共享数值

### 失败 4：加入家族 = 改一个字符串

### 失败 5：成立组织 = 点击按钮即永久存在

### 失败 6：NPC 自动知道玩家所有身份

### 失败 7：谣言传播 = 模型随意编故事

### 失败 8：玩家下令 = NPC 必然执行

### 失败 9：社会状态只改变对话，不改变行为

### 失败 10：为了“有戏”偷偷强制关系变化

---

# 15. 实现顺序

建议严格按以下顺序：

```text
P0 Event + History Ledger
↓
P1 Perception
↓
P2 Memory + Belief
↓
P3 Relationship
↓
P4 二次选择验证
↓
P5 Identity / Surname
↓
P6 Reputation Propagation
↓
P7 Organization Recognition
↓
P8 Group Request / Non-Hive Behavior
```

不要在 P4 之前扩展大量 NPC。

---

# 16. 本系统的最终验收句

当我们能够真实跑出下面这段，而不是人工写出这段时，Social Causality System 才算成立：

> 十天前，A 在饥荒中拒绝帮助 B。C 亲眼见过，D 只听 C 讲过。后来 B 加入了玩家的 Varen 家族。D 第一次见到 B 时并不认识他本人，但因为过去听说过 Varen 会保护成员，于是改变了谈判方式；E 从未听说这个姓氏，因此完全没有同样反应。又过了一段时间，D 的经历反过来改变了当地对 Varen 的认知。

这条链如果能够逐事件、逐认知、逐关系地追溯，K-I-N 就开始拥有真正的**社会历史**，而不只是带记忆的聊天 NPC。
