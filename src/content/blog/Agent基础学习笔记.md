---
title: 'Agent基础学习笔记'
description: 'Agent基础相关面试题'
pubDate: 'Jun 25 2026'
tags: ['agent','LangChain','LangGraph']
category: 'Agent'
---


#### 1. 你怎么理解 Agent？它和普通 LLM Chain 最大的区别是什么？  
我理解 Agent 是一种让 LLM 具备自主决策和行动能力的系统。和普通 Chain 最大的区别是，Chain 的执行路径通常是开发者预先定义好的，比如固定地执行检索、生成、解析等步骤；而 Agent 会根据当前任务目标、上下文状态和工具返回结果，动态决定下一步要做什么。

它通常会包含任务规划、工具调用、结果观察、反思和循环执行等能力，直到满足终止条件。比如在 LangGraph 里，可以把 Agent 的每一步抽象成节点，把状态放在 State 里，通过条件边控制下一步走向。所以从工程角度看，Agent 更像是一个带状态的决策循环，而不是单纯的一条固定 LLM 调用链路。

#### 2. LangChain 和 LangGraph 的区别是什么？为什么复杂 Agent 更适合用 LangGraph？  
LangChain 是一个构建 LLM 应用的框架，提供了模型调用、Prompt、Output Parser、Tool、Retriever、Memory、Agent 等组件。它更适合快速搭建线性或相对简单的 LLM 应用，比如 RAG Chain、工具调用 Chain、简单 Agent。

LangGraph 则更适合复杂 Agent，因为它把 Agent 流程抽象成图结构。每个节点可以是一次 LLM 调用、一个工具调用、一个判断逻辑或者一个子 Agent；边可以是普通边，也可以是条件边。更重要的是，LangGraph 有统一的 State，可以在多轮循环中持续维护任务状态、工具结果、错误信息和中间推理结果。

所以复杂 Agent 需要多轮决策、工具调用、观察结果、失败重试、人工介入、状态持久化时，用 LangGraph 会比普通 LangChain Chain 更清晰，也更容易控制和调试。

#### 3. 你怎么理解 LangGraph 里的 State、Node、Edge 和 Conditional Edge？  
在 LangGraph 里，State 是整个 Agent 图共享的状态结构，用来保存当前任务的上下文，比如用户输入、历史消息、工具调用结果、中间步骤、错误信息、最终答案等。它通常会提前定义 schema，比如用 `TypedDict` 或 `MessagesState`。

Node 是图里的执行节点，每个节点代表一步操作，比如调用 LLM、调用工具、做结果校验、进行反思或者生成最终答案。Node 一般会接收当前 State，然后返回对 State 的部分更新，对Node返回的`messages` 通常会用 `add_messages` 追加，而不是覆盖  。 LangGraph 里还有两个特殊节点，`START` 和 `END`。`START` 表示图的入口，`END` 表示流程终止。复杂 Agent 的循环一定要设计好终止条件，否则容易无限调用工具。  

Edge 是节点之间的连接关系，表示执行完一个节点后，下一步进入哪个节点。普通 Edge 是固定流转，比如 A 执行完一定到 B。

Conditional Edge 是条件边，会根据当前 State 或节点输出决定下一步走向。比如如果 LLM 判断需要调用工具，就路由到 tool node；如果已经得到答案，就路由到 END；如果工具失败，就路由到 retry node。

#### 4. 如果让你用 LangGraph 实现一个最简单的 ReAct Agent，你会怎么设计这个图？有哪些节点和边？  
如果我用 LangGraph 实现一个最简单的 ReAct Agent，我会设计两个核心节点：一个是 agent node，也就是 LLM 决策节点；另一个是 tool node，用来执行工具调用。

流程是从 START 进入 agent node，LLM 根据用户问题和当前 messages 判断是否需要调用工具。如果模型输出里有 tool_calls，就通过 conditional edge 路由到 tool node；tool node 执行工具后把 observation 追加到 messages，然后再回到 agent node。

如果 agent node 判断不需要继续调用工具，而是已经可以生成最终答案，就通过 conditional edge 进入 END。这样就形成了 ReAct 的“思考、行动、观察、再思考”的循环。

####  5.在 LangGraph 里，怎么判断 Agent 什么时候应该进入 tool node，什么时候应该结束？  
在 LangGraph 里，Agent 是否进入 tool node 通常由 agent node 的输出决定。我们会先给 LLM绑定tools，并在 prompt 里告诉它什么时候应该使用工具。模型拿到当前 messages 后，如果判断需要调用工具，就会在 AIMessage 里生成 `tool_calls`；Conditional Edge 会检查最后一条消息是否包含 tool_calls，如果有，就路由到 tool node。

tool node 执行完成后，会把工具返回结果作为 ToolMessage 追加到 messages，然后再回到 agent node。agent node 看到 observation 后继续判断下一步。如果最后一条 AIMessage 没有 tool_calls，而是直接生成了最终回答，就进入 END。

####  6.如果 Agent 一直循环调用工具，或者进入死循环，你会怎么处理？  
如果 Agent 一直循环调用工具，我会先加工程上的限制，比如最大迭代次数、最大工具调用次数、超时时间，防止系统资源被耗尽。

然后我会在 State 里记录每一步的 tool name、tool input、tool output 和 step count。如果发现多次调用同一个工具、输入高度相似、结果没有新增信息，就可以通过 conditional edge 路由到 fallback 或 END。

接着要分析死循环原因。如果是工具描述不清楚，模型不知道什么时候该调用，就需要优化 tool description、参数 schema 和 few-shot 示例；如果是工具返回结果不稳定或信息不足，就要修复工具逻辑，或者加结果校验节点；如果是 Prompt 没有明确终止条件，就要告诉模型什么情况下应该停止调用工具并给出最终回答。

最后我会加兜底策略，比如达到最大步数后，让 Agent 基于已有信息生成一个保守答案，或者向用户请求更明确的信息，而不是无限循环。

#### 7. 在 LangGraph Agent 中，State 里一般会保存哪些信息？为什么不能只依赖 messages？  
State 里一般会保存 Agent 执行任务所需的结构化信息，比如用户原始输入、历史 messages、当前步骤数、工具调用结果、中间变量、错误信息、任务状态、最终输出等。

不能只依赖 messages，是因为 messages 本质上是对话历史，它比较非结构化，里面的信息需要模型自己从文本中理解和提取，稳定性不够。对于工程流程来说，很多变量需要被代码直接读取，比如 step count、是否完成、工具是否失败、检索结果、重试次数、当前任务阶段等，这些更适合放在 State 的字段里。

所以我理解 messages 是 State 的一部分，适合保存 LLM 和工具交互的上下文；而 State 是更大的任务状态容器，用来让整个 Agent 的流程**可控、可调试、可恢复**。

例如**条件边可以直接读取**`state["step_count"]`、`state["status"]`来进行判断， 可以清楚看到**每一步产生了什么字段**，并且更加**节省 token**：不是所有中间信息都必须塞进 messages 给模型看。    

####  8.LangGraph 里的 checkpoint / memory 有什么作用？你怎么理解短期记忆和长期记忆？  
 在 LangGraph 里，**checkpoint 是图执行过程中的状态快照**。当在 `compile()` 时传入 `checkpointer`，LangGraph 会在图执行的每一步保存 state snapshot，并且这些快照会按 `thread_id` 组织起来。这样同一个用户、同一个会话后续再调用时，可以恢复之前的状态。  

```python
from langgraph.checkpoint.memory import InMemorySaver
from langgraph.graph import StateGraph

checkpointer = InMemorySaver()

builder = StateGraph(...)
graph = builder.compile(checkpointer=checkpointer)

graph.invoke(
    {"messages": [{"role": "user", "content": "hi, I am Bob"}]},
    {"configurable": {"thread_id": "user-1"}}
)

graph.invoke(
    {"messages": [{"role": "user", "content": "what is my name?"}]},
    {"configurable": {"thread_id": "user-1"}}
)
```

 这里的关键是 `thread_id`。LangGraph 的 checkpointer 会用 `thread_id` 作为保存和恢复 checkpoint 的标识；没有它，就无法知道应该恢复哪条会话线程的状态。  

 在 LangGraph 里，**短期记忆**通常是 **thread-level 持久化**，也就是某个会话线程里的 `messages`、中间变量、工具结果、任务状态等 State 被 checkpoint 持久化下来。官方文档把短期记忆定义为 agent state 的一部分，用来支持多轮对话；实现上就是给 graph 加 checkpointer，然后 invoke 时传入同一个 `thread_id`， 通常通过 `InMemorySaver`、`PostgresSaver` 这类 checkpointer 保存  。  

  长期记忆是跨 thread、跨 session 的信息，LangGraph 里**长期记忆**通常用 `store` 实现。官方文档说明，长期记忆基于 LangGraph Store，会把数据保存成 JSON document，并**通过 **`**namespace**`** 和 **`**key**`** 组织**；它和短期记忆不同，短期记忆是 thread-scoped，长期记忆可以跨 thread 读取， 按 user_id、application_id 这类 namespace 保存用户偏好、历史事实、项目背景等信息  。  

```python
from langgraph.store.memory import InMemoryStore

store = InMemoryStore()

builder = StateGraph(...)
graph = builder.compile(store=store)

# 或者使用数据库存
from langgraph.store.postgres import PostgresStore

DB_URI = "postgresql://..."

with PostgresStore.from_conn_string(DB_URI) as store:
    store.setup()
    graph = builder.compile(store=store)
```

#### 9. 如果你的 Agent 需要长期记住用户偏好，比如“用户喜欢简洁回答”，你会怎么设计长期记忆的读写流程？  
 我会在 State 初始化阶段，根据 user_id 从长期记忆 Store 中读取用户画像，比如用户偏好、背景信息和历史需求，然后把这些信息放入 State 或 system prompt 中，辅助 Agent 当前轮回答。任务结束后，我不会同步直接写入，而是发布 MQ 消息，让后台 memory worker 对本轮对话做信息抽取、去重、冲突检测和安全过滤，再更新用户画像。这样主链路延迟更低，也能避免把临时信息或噪声写入长期记忆。 长期记忆要控制写入质量。临时信息、一次性任务状态不应该写入长期记忆；只有稳定偏好、长期身份背景、跨会话有用的信息才应该保存。  

#### 10. Agent 里的 memory 和 RAG 有什么区别？什么时候用 memory，什么时候用 RAG？  
Memory 更关注用户和会话本身，比如用户偏好、历史对话、当前任务状态；RAG 更关注外部知识，比如文档库、FAQ、代码库、论文库。

用 memory 是为了让 Agent 具备连续性和个性化，比如记住用户喜欢简洁回答、正在准备面试。用 RAG 是为了让 Agent 回答依赖外部知识的问题，比如查公司内部文档或产品说明。

在实现上，memory 通常在 State 初始化时读取，并在任务结束后经过抽取和过滤再更新；RAG 可以作为一个 tool，由 Agent 在需要外部知识时主动调用。

#### 11. 如果让你把 RAG 接入 LangGraph Agent，你会怎么设计整体流程？  
 我会把 RAG 设计成工具形式接入 Agent。用户提问后，Agent 根据当前问题和策略判断是否需要检索知识库。如果需要，会先对 query 做 rewrite，必要时做 multi-query，然后调用 RAG tool，传入改写后的 query。RAG tool 内部完成向量检索、关键词检索、rerank，并返回相关文档片段。结果写回 State 后，Agent 判断检索内容是否足够回答；如果足够就基于文档生成答案，如果不足就重新改写检索或进入 fallback。  

####  12.如果 RAG 检索出来的内容不相关，Agent 应该怎么处理？  
如果检索内容不相关，我会先阻止模型直接生成答案，而是加一个 evaluator node 判断文档和 query 的相关性以及证据是否充足。

如果不相关，就通过 conditional edge 进入 query rewrite 或 retry，比如改写查询、multi-query、增大 top-k、切换 hybrid search、加 rerank 或调整相似度阈值。

如果重试后仍然没有有效证据，就进入 fallback，告诉用户知识库里没有足够信息支持回答，或者请求用户补充更明确的问题。这样可以减少 RAG Agent 的幻觉。 

在 State 我会里记录 `retrieval_attempts`、`last_query`、`retrieved_docs`、`evidence_score`，防止 Agent 无限重试检索。  

#### 13.Agent 调用工具失败了怎么办？比如工具超时、参数错误、API 返回异常，你会怎么设计错误处理？
我会在 State 里记录每次工具调用的输入、输出、错误类型和 retry_count。处理时按错误分类：参数错误就让模型根据报错和 schema 修正参数；超时或 5xx 就有限重试，可以加指数退避；权限错误一般直接失败并告警；业务空结果可以改写 query 或切换工具。

如果达到重试上限，就通过 conditional edge 进入 fallback，避免死循环，同时记录日志和监控指标，必要时主动告警。 工具错误处理最好不要只靠 LLM 自己反思，而是用代码层面的异常捕获、schema 校验、超时控制和 retry policy 来保证稳定性 。

#### 14. Agent 的 tool description 应该怎么写？为什么 tool description 对 Agent 效果影响很大？  
Tool description 是给 LLM 看的工具说明，它会影响模型什么时候调用工具、选择哪个工具、以及如何生成工具参数。

一个好的 tool description 应该包含几个要素：工具的功能、适用场景、不适用场景、输入参数含义、参数格式、返回结果格式，以及必要的示例。描述要明确、无歧义，尤其是多个工具功能相近时，要说明它们的边界。

如果 tool description 写得不清楚，Agent 可能会调用错误工具、传错参数，或者在不该调用工具的时候频繁调用，从而导致结果错误或死循环。

```python
from langchain_core.tools import tool

@tool
def search_docs(query: str) -> str:
    """
    Search internal knowledge base documents.

    Use this tool when the user asks questions that require information
    from internal documentation, product manuals, or company knowledge base.

    Args:
        query: A specific search query in natural language. It should include
        key entities, product names, or technical terms.

    Returns:
        Relevant document snippets with source metadata.
    """
    ...
```

####  15.如果两个工具功能很相似，比如 `search_user_profile` 和 `search_knowledge_base`，你怎么避免 Agent 选错工具？  
如果两个工具功能相似，我会先从**工具命名和 description** 上做区分。比如 `search_user_profile` 明确只查用户画像、偏好和历史背景；`search_knowledge_base` 明确只查公司文档、产品文档或业务知识库。description 里不仅写什么时候调用，也要写什么时候不要调用，并给出反例。

同时我会用**结构化参数 schema **区分工具输入，让模型更容易生成正确参数。对于复杂 Agent，我还会加一个 router 节点，先根据用户问题判断属于用户画像查询还是知识库查询，然后只暴露对应工具，或者通过 conditional edge 路由到对应子图。

如果工具经常被选错，我会通过日志分析错误样本，补充 few-shot 示例或调整工具边界。

#### 16. Agent 里面的 Planner 和 Executor 分别负责什么？什么时候需要 Plan-and-Execute 架构？  
Planner 主要负责把复杂任务拆成步骤，决定整体执行路径；Executor 负责按步骤具体执行，比如调用工具、检索、处理数据或生成中间结果。

Plan-and-Execute 适合**长任务、多步骤任务和多工具协作任务**。相比**普通 ReAct 每一步都临时决定**，Plan-and-Execute 会先有一个全局计划，所以任务结构更清晰，也更方便监控进度。

但实际工程里计划不一定一次就完全正确，所以我会在 State 里保存 plan、current_step、completed_steps、tool_results 等字段，并**设计 replanner 节点**，在执行失败或信息不足时动态调整计划。

####  17.ReAct Agent 和 Plan-and-Execute Agent 有什么区别？各自适合什么场景？  
ReAct Agent 是每一轮都由 LLM 根据当前状态决定下一步，比如是否调用工具、调用哪个工具、是否结束。它的特点是**灵活，适合中短任务、工具反馈不确定、需要边观察边调整的场景**，比如搜索、问答、简单排查问题。

Plan-and-Execute Agent 会先由** Planner 根据用户目标生成整体计划，然后 Executor 按步骤执行**。它更适合复杂**长任务、多步骤任务和多工具协作任务**，比如分析多篇文档、生成调研报告、完成数据分析流程等。

两者的区别在于，ReAct 更偏局部即时决策，Plan-and-Execute 更偏全局规划。ReAct 灵活但容易缺少全局视角；Plan-and-Execute 结构清晰但计划可能过时，所以工程上通常还会加 replanner，在执行过程中根据结果调整计划。

**18. 在 LangGraph 里，如果要实现 Plan-and-Execute，你会怎么设计 State、Node 和 Edge？  **

在 LangGraph 里实现 Plan-and-Execute，我会先设计一个结构化 State，里面保存用户目标、全局 plan、当前执行到第几步、已完成步骤、每一步结果、错误信息和最终答案。

```python
# plan：全局计划
# current_step_index：当前执行到哪一步
# completed_steps：已经完成哪些步骤
# tool_results：每一步工具调用结果
# status / error：用于条件边判断是否重规划、失败或结束
class AgentState(TypedDict):
    messages: list
    user_query: str
    plan: list[str]
    current_step_index: int
    completed_steps: list[str]
    current_step_result: str | None
    tool_results: dict
    status: Literal["planning", "executing", "replanning", "finished", "failed"]
    error: str | None
    final_answer: str | None
```

Node 上我会设计 planner、executor、evaluator、replanner 和 answer node。Planner 负责生成初始计划；Executor 负责执行当前 step，可能调用工具；Evaluator 判断当前步骤是否完成、结果是否有效；如果失败或发现计划不合理，就进入 replanner 修改计划；如果所有步骤都完成，就进入 answer node 汇总结果。

Edge 上，START 先到 planner，再到 executor。Executor 执行完后进入 evaluator，由 conditional edge 判断下一步：如果当前步骤成功且还有后续步骤，就继续 executor；如果结果失败或信息不足，就进入 replanner；如果全部完成，就进入 answer，最后到 END。

**19. 什么是 Multi-Agent？在什么情况下你会用 Multi-Agent，而不是单 Agent？  **

Multi-Agent 是把一个**复杂任务拆给多个具备不同角色或工具权限的 Agent 协作完成**。每个 Agent 可以有自己的 prompt、工具、memory 和职责边界，比如 researcher 负责检索资料，writer 负责组织表达，critic 负责检查质量，coder 负责写代码。

**我**会在任务需要**明显分工、多领域能力、多工具隔离**，或者需要互相审查时使用 Multi-Agent。比如调研报告可以拆成搜索 Agent、分析 Agent、写作 Agent、审查 Agent；代码任务可以拆成 planner、coder、tester、reviewer。

但如果任务比较简单，单 Agent 加工具调用就够了。Multi-Agent 的问题是**链路更长、延迟和成本更高，也更容易出现上下文丢失、重复劳动和协调失败**。所以我会优先用单 Agent，只有当任务复杂度、角色隔离或质量要求需要时才上 Multi-Agent。

#### 20. 在 LangGraph 里，你会怎么实现 Multi-Agent 协作？常见有哪些架构？  
在 LangGraph 里实现 Multi-Agent，我会把每个 Agent 设计成一个 node，或者设计成一个 subgraph。比如 researcher agent 负责检索，writer agent 负责生成答案，reviewer agent 负责检查结果。

**常见 Multi-Agent 架构 **

**1. 架构一：Supervisor 架构 **

```python
#一个 supervisor 负责调度多个 worker agent。
supervisor
  ├── researcher
  ├── writer
  ├── reviewer
  └── coder
```

 在 LangGraph 里，我可以用 supervisor node 管理多个 agent node。Supervisor 根据当前 State 判断下一步交给哪个 Agent，比如先让 researcher 检索资料，再让 writer 写答案，最后让 reviewer 检查质量。每个 Agent 执行完后把结果写回 State，再回到 supervisor 继续决策。  

** 2.架构二：Pipeline 架构  **

```python
START → research_agent → analysis_agent → writing_agent → review_agent → END
```

 如果任务流程比较固定，我会用 pipeline 多 Agent，比如搜索 Agent 负责找资料，分析 Agent 负责提炼观点，写作 Agent 负责生成内容，审核 Agent 负责检查事实和格式。  

** 3.架构三：Hierarchical 架构  **

```python
top_supervisor
  ├── research_team_supervisor
  │     ├── web_search_agent
  │     └── kb_search_agent
  └── writing_team_supervisor
        ├── outline_agent
        └── editor_agent
```

 适合更复杂的大任务，比如企业级 Agent 系统。  

** 4.架构四：Debate / Critic 架构  **

```python
generator_agent → critic_agent → generator_agent → final_answer
```

 如果对质量要求比较高，可以加 critic agent，对 writer 或 coder 的输出进行检查，指出问题后再让原 Agent 修改。  

**State 怎么设计？**  

```python
class MultiAgentState(TypedDict):
    user_query: str
    messages: list
    current_agent: str
    research_result: str | None
    analysis_result: str | None
    draft_answer: str | None
    review_feedback: str | None
    final_answer: str | None
    status: str
    step_count: int
```

 核心是要把不同 Agent 的结果结构化保存，而不是全部塞进 messages。  

工程上要注意 Multi-Agent 不是越多越好，需要控制状态同步、上下文传递、循环次数、成本和延迟。简单任务我会优先用单 Agent，只有需要明确角色分工或质量校验时才用 Multi-Agent。

####  21.Multi-Agent 系统里，多个 Agent 之间怎么共享上下文？是所有信息都共享吗？  
Multi-Agent 系统里多个 Agent 之间通常不会共享全部上下文，而是通过一个全局 State 或消息总线共享必要信息。每个 Agent 根据自己的职责，只读取和自己任务相关的字段。

比如 researcher agent 只需要看到用户问题和检索需求，writer agent 只需要看到整理后的研究结果和写作要求，reviewer agent 只需要看到最终草稿和评价标准。这样可以减少 token 成本，也能避免上下文污染和权限风险。

在 LangGraph 里，可以用 State 保存全局任务状态，Supervisor决定把哪些信息传给哪个 Agent, 在每个 node 内部构造 prompt 时，只取该 Agent 需要的字段。Agent 执行完后，把结构化输出写回 State，再由 supervisor 或 conditional edge 决定下一步。

```python
class MultiAgentState(TypedDict):
    user_query: str
    requirements: dict
    research_result: str | None
    analysis_result: str | None
    draft: str | None
    review_feedback: str | None
    final_answer: str | None
# research_agent：
# 读取 user_query、requirements
# 输出 research_result

# analysis_agent：
# 读取 user_query、research_result
# 输出 analysis_result

# writer_agent：
# 读取 user_query、requirements、analysis_result
# 输出 draft

# reviewer_agent：
# 读取 user_query、requirements、draft
# 输出 review_feedback
```

####  22.LangChain里面Multi-Agent 的不同实现。
LangChain 里 Multi-Agent 有不同实现层级。最基础的是用 `create_agent` 手动实现 agent-as-tool pattern：先创建 calendar agent、email agent 这类 specialized agents，然后用 `@tool` 把它们包装成高层工具，最后交给 supervisor agent 调用。这样 supervisor 只看到 `schedule_event`、`manage_email` 这种领域级工具，而不会暴露底层 API 工具。

```python
calendar_agent = create_agent(...)
email_agent = create_agent(...)

@tool
def schedule_event(request: str) -> str:
    result = calendar_agent.invoke(...)
    return result["messages"][-1].text

supervisor_agent = create_agent(
    model,
    tools=[schedule_event, manage_email],
    system_prompt=SUPERVISOR_PROMPT,
)
```

Deep Agents 的 `create_deep_agent` 是更高层的封装。它源码里最终还是调用 LangChain 的 `create_agent`，但在此之前自动组装了 DeepAgentState、默认 system prompt、TodoListMiddleware、FilesystemMiddleware、SubAgentMiddleware、SummarizationMiddleware、MemoryMiddleware 等。因此它天然支持 planning、filesystem、context compression 和 subagent delegation。

```python
from datetime import datetime

from deepagents import create_deep_agent
from langchain.chat_models import init_chat_model

max_concurrent_research_units = 3
max_researcher_iterations = 3

current_date = datetime.now().strftime("%Y-%m-%d")

INSTRUCTIONS = (
    RESEARCH_WORKFLOW_INSTRUCTIONS
    + "\n\n"
    + "=" * 80
    + "\n\n"
    + SUBAGENT_DELEGATION_INSTRUCTIONS.format(
        max_concurrent_research_units=max_concurrent_research_units,
        max_researcher_iterations=max_researcher_iterations,
    )
)

research_sub_agent = {
    "name": "research-agent",
    "description": "Delegate research to the sub-agent. Give one topic at a time.",
    "system_prompt": RESEARCHER_INSTRUCTIONS.format(date=current_date),
    "tools": [tavily_search],
}

model = init_chat_model(model="google_genai:gemini-3.5-flash", temperature=0.0)

agent = create_deep_agent(
    model=model,
    tools=[tavily_search],
    system_prompt=INSTRUCTIONS,
    subagents=[research_sub_agent],
)
# create_deep_agent 的源码最后返回的是
# return create_agent(
#     model,
#     system_prompt=final_system_prompt,
#     tools=_tools,
#     middleware=deepagent_middleware,
#     response_format=response_format,
#     context_schema=context_schema,
#     checkpointer=checkpointer,
#     store=store,
#     debug=debug,
#     name=name,
#     cache=cache,
#     state_schema=state_schema if state_schema is not None else DeepAgentState,
# ).with_config(...)
```

Middleware 是插在 Agent 执行循环里的扩展层，用来在不改 Agent 主逻辑的情况下控制模型调用、工具调用、状态更新、上下文管理和错误处理。 官方把 custom middleware 的 hook 分成两类  ：

 第一类是 **node-style hooks**，在固定时机运行 ， 第二类是 **wrap-style hooks**，包住某个调用，可以决定是否调用、调用几次、是否重试  

```python
before_agent：每次 agent 开始前运行一次
before_model：每次调用模型前运行
after_model：每次模型返回后运行
after_agent：agent 完成后运行一次

wrap_model_call：包住模型调用
wrap_tool_call：包住工具调用
```

```python
from typing import Any
from typing_extensions import NotRequired
from langchain.agents.middleware import after_model, AgentState
from langgraph.runtime import Runtime

class MyState(AgentState):
    model_call_count: NotRequired[int]

@after_model(state_schema=MyState)
def count_model_call(state: MyState, runtime: Runtime) -> dict[str, Any]:
    return {
        "model_call_count": state.get("model_call_count", 0) + 1
    }
agent = create_agent(
    model=model,
    tools=tools,
    middleware=[count_model_call],
    state_schema=MyState,
)
```

Deep Agents 之所以一行代码能支持 planning、filesystem、subagent 和 summarization，就是因为它在普通 `create_agent` 的基础上预装了一组 middleware，比如 TodoListMiddleware、FilesystemMiddleware、SubAgentMiddleware 和 SummarizationMiddleware。

所以普通 `create_agent` 多 Agent 更适合业务流程可控、想手动设计 supervisor 和工具边界的场景；`create_deep_agent` 更适合 deep research、coding agent、长任务这类需要默认规划、文件系统和上下文管理的复杂任务。

#### 23. Agent 工程里，如何评估一个 Agent 做得好不好？你会看哪些指标？  
Agent 的评估我会分两层：一层是最终任务成功率，一层是执行过程的成功率。因为 Agent 是多步系统，最终答案失败可能不是生成问题，也可能是规划错了、工具选错了、参数错了、检索结果不相关，或者循环控制有问题。

最终结果上，我会看**任务完成率、答案正确性、完整性、是否遵循指令、是否有幻觉**。如果是 RAG Agent，还要看答案是否有证据支持。

过程上，我会看每个节点的成功率，比如 planner 生成的计划是否合理，tool selection 是否正确，tool arguments 是否有效，工具调用是否成功，retrieval 是否相关，是否出现重复调用和死循环。

工程上还会看**延迟、token 成本、工具调用次数、重试次数、fallback 率和异常率**。这样才能定位问题到底出在 Agent 的哪一步，而不是只看最终回答对不对。

#### 24. 如果线上发现 Agent 最终回答错误，你会怎么定位问题？你会从哪些日志或 trace 里排查？  
 如果线上发现 Agent 最终回答错误，我会先通过 trace 回放完整执行链路，包括用户输入、prompt、planner 输出、路由决策、tool_calls、工具入参和返回、State 变化以及最终答案。然后按错误类型定位：如果是问题理解错误，就看 system prompt、few-shot 和 planner；如果是执行路径不符合预期，就看 router、conditional edge 和工具选择；如果是事实性错误，就看工具返回、RAG 检索结果和数据源；如果工具结果正确但最终回答错误，就看 answer node 的 prompt 和 evidence 使用情况。这样才能定位失败发生在哪一个节点，而不是只看最终输出。  

####  25.Agent 工程里，怎么做日志和可观测性？你会记录哪些信息？  
Agent 工程里的日志和可观测性，我会用 LangSmith 做 trace。因为 Agent 是多步系统，只看最终答案不够，需要看到完整执行轨迹，包括每轮 LLM 调用、tool_calls、工具入参和返回、LangGraph 节点流转、State 变化、路由决策和最终输出。

在使用上，如果是 LangChain `create_agent` 或 LangGraph，只需要配置 `LANGSMITH_TRACING=true`、`LANGSMITH_API_KEY` 和 `LANGSMITH_PROJECT`，正常运行 `agent.invoke()` 就会自动记录 trace。然后我会给每次调用加 tags 和 metadata，比如 user_id、thread_id、environment、app_version、agent_name，方便按版本和会话排查。

```python
response = agent.invoke(
    {
        "messages": [
            {
                "role": "user",
                "content": "帮我查知识库里 LangGraph checkpoint 的说明"
            }
        ]
    },
    config={
        "tags": ["production", "rag-agent", "v1.0"],
        "metadata": {
            "user_id": "user_123",
            "session_id": "session_456",
            "thread_id": "thread_789",
            "environment": "production",
            "app_version": "2026-06-10"
        }
    }
)
```

具体指标上，我会记录请求级别的延迟、token、成本和状态；模型级别的 prompt、输出、tool_calls；工具级别的 tool_name、tool_args、tool_output、latency、error、retry_count；LangGraph 级别的 node_name、state_before、state_after、route_decision 和 fallback。

如果是 Multi-Agent 或 Deep Agents，我还会按 subagent name 过滤 trace，单独看 researcher、writer、reviewer 等子 Agent 的表现。生产环境还要做敏感信息脱敏，避免把隐私数据直接写进日志。

#### 26.如果 Agent 线上成本太高、响应太慢，你会从哪些方面优化？  
我会先用 LangSmith trace 定位瓶颈，看每个节点的耗时、token 消耗、工具调用次数和重试次数。然后分层优化。

模型层面做 model routing，简单的 router、分类、参数抽取、子任务可以用小模型或本地 4B/8B 模型，复杂推理和最终生成再用大模型。上下文层面做裁剪和总结，只给每个节点传必要信息，避免 messages 和工具结果无限增长。

工具层面减少无效调用，优化 tool description，设置超时、重试上限和 fallback。RAG 里控制 top-k、rerank 成本和缓存检索结果。流程上可以并行执行独立任务，避免不必要的串行链路。

缓存层面使用 query cache、retrieval cache 和 tool result cache。  

最后通过 LangSmith 或日志持续看 p95 延迟、token 成本、工具调用次数、重试率和最终任务成功率，确保优化没有降低效果。

#### 27.如果 Agent 需要调用一个高风险工具，比如发邮件、下订单、删除数据，你会怎么设计安全机制？  
对于发邮件、下订单、删除数据这类高风险工具，我会先对工具按风险等级分类。低风险工具可以自动执行，比如搜索、读取文档；中风险工具需要更严格的参数校验；高风险工具则必须进入 human-in-the-loop。

在 LangGraph 里，可以通过 conditional edge 判断当前 tool call 是否属于高风险操作。如果是，就进入 review node，在节点里调用 `interrupt()` 暂停图执行，把工具名、工具参数、影响范围和预期结果展示给人类审批。LangGraph 会通过 checkpointer 保存当前 State，等人类返回 approve / edit / reject 后，再用 `Command(resume=...)` 恢复执行。官方文档也说明，`interrupt()` 可以暂停图执行、等待外部输入，并依赖持久化层保存状态后恢复。

```python
from typing import TypedDict, Literal
from langgraph.types import interrupt, Command

class AgentState(TypedDict):
    tool_name: str
    tool_args: dict
    approval_status: Literal["pending", "approved", "rejected", "edited"] | None


def human_review_node(state: AgentState):
    decision = interrupt({
        "action": "review_tool_call",
        "tool_name": state["tool_name"],
        "tool_args": state["tool_args"],
        "risk_level": "high",
        "message": "Please approve, edit, or reject this tool call."
    })

    return {
        "approval_status": decision["type"],
        "tool_args": decision.get("edited_args", state["tool_args"])
    }
# 恢复时
graph.invoke(
    Command(resume={
        "type": "approved"
    }),
    config={"configurable": {"thread_id": "user-123"}}
)
#edit时
graph.invoke(
    Command(resume={
        "type": "edited",
        "edited_args": {
            "to": "alice@example.com",
            "subject": "Updated subject",
            "body": "Updated body"
        }
    }),
    config={"configurable": {"thread_id": "user-123"}}
)
```

如果用 LangChain `create_agent`，可以直接用官方的 `HumanInTheLoopMiddleware`，对指定工具配置 `interrupt_on`。比如对 `send_email`、`delete_data`、`place_order` 这类工具要求人工审批。LangChain 官方也把 human-in-the-loop middleware 列为内置 middleware，用于在敏感 tool call 前暂停执行。

```python
from langchain.agents import create_agent
from langchain.agents.middleware import HumanInTheLoopMiddleware
from langgraph.checkpoint.memory import InMemorySaver

agent = create_agent(
    model=model,
    tools=[send_email, delete_record, search_docs],
    middleware=[
        HumanInTheLoopMiddleware(
            interrupt_on={
                "send_email": True,
                "delete_record": True,
            }
        )
    ],
    checkpointer=InMemorySaver(),
)
```

28. LangChain / LangGraph 官方实现的长短期记忆。

Agent 里的记忆我会分成短期记忆和长期记忆来实现。短期记忆是 thread 级别的对话状态，比如 messages、工具调用结果和当前任务状态，在 LangGraph 里通过 State + checkpointer 实现。开发环境可以用 `InMemorySaver`，生产环境我会用 `PostgresSaver`，通过 `thread_id` 保存和恢复同一个会话的 checkpoint。

```python
# InMemorySaver
from langgraph.checkpoint.memory import InMemorySaver
from langgraph.graph import StateGraph

checkpointer = InMemorySaver()

builder = StateGraph(...)
graph = builder.compile(checkpointer=checkpointer)

graph.invoke(
    {"messages": [{"role": "user", "content": "hi! i am Bob"}]},
    {"configurable": {"thread_id": "1"}},  # 关键
)
# PostgresSaver
from langgraph.checkpoint.postgres import PostgresSaver

DB_URI = "postgresql://postgres:postgres@localhost:5442/postgres?sslmode=disable"

with PostgresSaver.from_conn_string(DB_URI) as checkpointer:
    builder = StateGraph(...)
    graph = builder.compile(checkpointer=checkpointer)
```

长期记忆是跨会话的用户画像或稳定偏好，比如用户喜欢简洁回答、用户技术栈等。在 LangGraph 里用 Store 实现，数据以 JSON document 存储，通过 namespace 和 key 组织。开发可以用 `InMemoryStore`，生产用 `PostgresStore`，如果需要语义搜索可以加 pgvector 或配置 Store 的 embedding index。

```python
from langgraph.store.memory import InMemoryStore

store = InMemoryStore()

builder = StateGraph(...)
graph = builder.compile(store=store)
# PostgresStore
from langgraph.store.postgres import PostgresStore

DB_URI = "postgresql://postgres:postgres@localhost:5432/postgres?sslmode=disable"

with PostgresStore.from_conn_string(DB_URI) as store:
    store.setup()

    graph = builder.compile(store=store)
```

读长期记忆时，我会根据 `user_id` 组成 namespace，比如 `("memories", user_id)`，在每次 Agent 调用开始时用 `runtime.store.search()` 检索相关记忆，并注入 system prompt 或 State。写长期记忆可以有两种方式：一种是 hot path，用 LangMem 的 `create_manage_memory_tool` 和 `create_search_memory_tool` 让 Agent 自己管理记忆；另一种是任务结束后发 MQ 给后台 memory worker，异步抽取、去重、冲突检测后再写入 Store。生产系统里我更倾向第二种，因为可控性更好。
