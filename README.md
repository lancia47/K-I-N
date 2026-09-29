# K-I-N

> **关系先于剧情。选择生成历史，历史塑造主体，主体再塑造世界。**

K-I-N 是一个以 **社会关系、有限认知、持续记忆与因果后果** 为核心的涌现式世界模拟项目。

项目并不试图让 AI 直接“写剧情”，而是构建一个能够持续产生真实选择与后果的世界：主体在局部信息下做选择，WorldCore 结算真实后果，历史留下不可逆痕迹，记忆与关系改变下一次选择；故事则由玩家或其他观察者从历史中发现。

## 当前阶段

- 状态：**Pre-Alpha / Design Specification**
- 当前主版本：**v0.5**
- 当前核心任务：把 v0.4 的设计哲学重构为可直接指导原型开发的六层项目规格
- 第一优先级系统：**Social Causality System / 社会因果系统**

## 项目代号

**KIN** 原意为亲族、同族、关系共同体。

在本项目里，它强调的不是“血缘系统”本身，而是：

> 个体之间发生的事情会被记住；记忆沉淀为关系；关系逐渐形成身份、家族、派系与制度；这些结构再反过来改变每个人的选择。

Jev 保留为项目内部的 **Local Choice Operator（局部选择算子）**：它帮助主体在当前认知、需求、记忆、关系和允许行动中做局部选择，但不负责修改世界事实。

## 核心公式

```text
Possibility
→ Choice
→ Consequence
→ History
→ Memory / Belief / Relationship
→ Agent Adaptation
→ New Choice
```

同时：

```text
History
→ Observation
→ Story
```

社会层进一步展开为：

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

## 文档

- [`docs/KIN_Game_Project_Design_v0.5.md`](docs/KIN_Game_Project_Design_v0.5.md) — 六层主设计规格
- [`docs/systems/Social_Causality_System_v0.1.md`](docs/systems/Social_Causality_System_v0.1.md) — 社会因果系统详细规格
- [`docs/archive/v0.4_来源与设计宪法.md`](docs/archive/v0.4_来源与设计宪法.md) — v0.4 上游文档来源与不可轻易破坏的设计公理

## 六层结构

1. **核心哲学 / Why** — 为什么做，以及哪些原则不能被实现细节破坏
2. **玩家体验与核心循环 / What the player does** — 玩家每分钟、每小时、每局到底在做什么
3. **世界模拟 / WorldCore** — 资源、时间、空间、行动与后果如何被真实结算
4. **社会因果 / Social Causality** — 事件如何变成记忆、关系、身份、组织与新的行为
5. **技术架构 / How** — 规则系统、Jev、Belief、Memory、History Ledger 等如何分工
6. **MVP 与验证 / Proof** — 第一版究竟验证什么，什么算成功，什么算失败

## 设计红线

- 模型不能直接修改世界状态
- NPC 不能读取完整 World State
- 不为了戏剧性偷偷控制角色选择
- 不把随机噪声称为涌现
- 不把文本描述当成制度或组织
- 不把单一“好感度”当作关系
- 不把所有 NPC 做成服从玩家的工蜂
- 不把在线 RL / 微调当作 MVP 的前置条件

## 当前最重要的问题

> **能否让一群有局部认知、不同记忆和真实利益的主体，在没有主线脚本的情况下，形成可追溯的关系、共同体与历史；并让玩家真正感到自己生活在这段历史之中？**
