---
title: "用木匠做凳子的比喻，理解整个 LLM 技术栈"
date: 2026-05-25
tags: ["AI框架", "思维模型", "LLM", "Agent", "MCP", "Claude Code"]
description: "把 40 多个 LLM/AI Agent 技术概念装进一个木匠工坊的比喻里——大脑、肢体、学徒、刨子、工作台、神经系统，看完你就全懂了。"
---

## 为什么用木匠做凳子来比喻？

LLM 技术栈越来越复杂——Prompt、Agent、MCP、RAG、Embedding、Token Budget……概念多到让人头疼。

我找到一个办法：**把它们全装进一个木匠工坊的比喻里**。你只需要想象"一个师傅带着学徒做一把凳子"的完整流程，就能理解整个技术栈。

这个比喻框架覆盖了 **14 个层级、40+ 个技术概念**，以下是最核心的映射。

---

## 一、核心角色：谁是师傅，谁是大脑？

| 工坊角色 | 技术实体 | 一句话 |
|----------|----------|--------|
| 师傅（你） | User | 提需求、做决策、验收成品 |
| 大脑 | LLM | 理解意图、推理、决策、生成 |
| 肢体 | Claude Code / Codex / Hermes | 执行动作、协调工具 |
| 学徒 | Agent / Sub-agent | 接受任务，独立完成，汇报结果 |
| 工头 | Orchestrator | 拆活儿、派单、验收 |

**Prompt 的三层含义：**
- **System Prompt** = 师傅每天开工前的例行规矩："榫头宁紧勿松"
- **User Prompt** = 顾客上门说的："我要一把 45cm 高的槐木凳子"
- **Skill Prompt** = 贴在工作台上的工序卡："先打榫眼，再锯榫头"

---

## 二、能力层：Tools、Plugin、API 怎么区分？

| 概念 | 比喻 | 一句话 |
|------|------|--------|
| **Tools** | 尺子、刨子、凿子、锯子 | 原子能力，一把螺丝刀 |
| **Plugin** | 台钻的斜孔模块 | 插上就用，拔掉不影响，封装了多 tool + prompt |
| **API** | M6 标准螺丝 | 互换性协议，谁产的都能拧 |
| **Tool Use** | 知道何时该用刨子 | 大脑的核心能力：判断 + 调用 |

---

## 三、MCP 和 Context：神经 vs 工作台

这是我个人最满意的两个比喻：

- **MCP (Model Context Protocol)** = **神经系统**。大脑想"刨这块木头"，信号要经过神经传到手。MCP 管的是连接和路由——大脑不直接握刨子。

- **Context** = **工作台上的东西**。当前手边的半成品、图纸、尺寸、刨子。这就是大脑决策的全部依据。工作台之外的东西，大脑不知道——除非你起身去翻（RAG）。

---

## 四、约束层：不是你想做多大就多大

| 概念 | 比喻 |
|------|------|
| Context Window | 工作台面积——就这么大，一次铺不了太多 |
| Token Budget | 工钱预算——一把椅子预算 50 文 |
| Rate Limit | 接单产能——一天最多接 3 个活 |
| Caching | 模板/模具——一样的榫头做十个，做个模子比每次划线快且省 |
| Compaction | 收拾台面——台面满了，收进工具箱，丢掉细节保留要害 |

---

## 五、质量控制：做错了怎么办？

| 概念 | 比喻 |
|------|------|
| **Temperature** | 胆量。T≈0 严格按图纸（算数），T≈1 随性发挥（写诗） |
| **Hallucination** | 闭眼瞎刨。自信满满觉得平了，拿尺一量歪了 2mm。概率模型的固有属性，不是 bug |
| **Guardrails** | 台锯防护罩。硬约束，必须装，没商量 |
| **Structured Output** | 公差要求 ±0.2mm。JSON Schema 就是把格式卡死 |
| **Verification** | 拿尺量。做完一步验一步，不靠信任靠测量 |

---

## 六、学习层：大脑怎么变聪明？

| 概念 | 比喻 | 一句话 |
|------|------|--------|
| **Memory** | 肌肉记忆/工坊日记 | 跨会话保留经验，不用每次重新想 |
| **Fine-tuning** | 师承/流派 | 跟鲁班还是朱由校学的，风格不同 |
| **RAG** | 翻木工手册 | 查标准尺寸→喂入上下文→大脑决策 |
| **Embeddings** | 木料分类嗅觉 | 一闻就知硬杂木还是软杂木（语义聚类） |
| **In-context Learning** | 师傅示范一遍，徒弟当场学会 | Few-shot prompting |

---

## 七、运行与运维：刨坏了怎么办？

| 概念 | 比喻 |
|------|------|
| Streaming | 边刨边看——刨花一卷一卷飞，随时喊停 |
| Batch | 五把椅子腿一起锯——批量加工省时间 |
| Latency (TTFT) | 第一片刨花飞出来的时间 |
| Concurrency | 三个学徒同时刨板子，互不干扰 |
| Retry | 榫头刨小了，重新锯一根 |
| Fallback | 这个办法不行，换条路走 |
| Idempotency | 一刀是一刀——同一刀不可能切两次 |
| Observability | 工坊监控——Logs/Metrics/Traces 三件套 |

---

## 八、完整层级总图

```
认知层:   LLM / Prompt / Plan / Reasoning / Temperature / Hallucination
编排层:   Workflow → Skill
执行层:   Claude Code / Agent / Orchestrator
协调层:   MCP (神经系统)
能力层:   Plugin / Tools / API / Tool Use
质量层:   Guardrails / Structured Output / Verification
学习层:   Memory / Fine-tuning / RAG / Embeddings
约束层:   Context Window / Token Budget / Rate Limit / Caching
环境层:   Sandbox / Worktree / Hooks / Git
运行层:   Streaming / Batch / Latency / Concurrency
观测层:   Observability / Retry-Fallback / Idempotency
模态层:   Multi-modal / Input-Output-Thinking tokens
```

---

## 九、一个故事的总结

> 顾客上门："师傅，我要一把凳子，45cm 高，槐木。"（User Prompt）
>
> 师傅看工作台（Context Window），这单预算 50 文（Token Budget）。
>
> 翻开木工手册查尺寸（RAG），画张图纸（Plan），顺手做榫头模板（Caching）。
>
> 按工序卡开干（Skill），让三个学徒（Concurrency）同时锯腿，师傅只管验收（Verification）。
>
> 大脑通过神经（MCP）告诉手拿刨子（Tool Use）。刨花一卷卷飞（Streaming），刨完拿尺量（Structured Output），±0.2mm 合格。
>
> 台面满了收一收（Compaction），关键步骤记工序本（Git）。工坊监控全程记着（Observability）。
>
> 安全护栏卡着（Guardrails），台锯防护罩必须装。三天后交货，每一刀都有记录，出问题随时回溯。
>
> 下次再来个顾客要桌子，师傅的肌肉记忆（Memory）已经记住了：槐木硬，凳腿三缺榫，桌子得换插肩榫。

---

## 资源

- 📄 **Word 文档下载**：[LLM技术栈-木匠工坊比喻框架.docx](/LLM技术栈-木匠工坊比喻框架.docx)
- 🧠 **思维导图**：[在线查看](/LLM技术栈-木匠工坊-思维导图.html)
- 📖 **完整源文档**（16 章）：[GitHub](https://github.com/danielcao2023-cyber/my-ai-journey)

---

*2026-05-25 · Daniel Cao · [danielcao2023@gmail.com](mailto:danielcao2023@gmail.com)*
