*← README.md*

# Chapter 2: Implement Generative AI and Agentic Solutions (30–35%)

This is the **largest and most heavily weighted** exam domain at **30–35%** of total questions. It covers building LLM-powered applications, constructing intelligent agents, multi-agent orchestration, and optimization techniques.

---

## 2.1 Build Generative Applications by Using Foundry

**Exam Skills Mapped**:
- Deploy and consume LLMs, small models, code models, and multimodal models
- Implement retrieval-augmented generation (RAG) in an application
- Design workflows, tool-augmented flows, and multi-step reasoning pipelines
- Evaluate models and apps (detect fabrications, relevance, quality, safety)
- Integrate generative workflows into applications using Foundry SDKs and connectors
- Configure an application to connect to a Foundry project

### Implementing RAG (Retrieval-Augmented Generation)

RAG is a core pattern tested on this exam. The workflow:

1. **Ingest and index content** into Azure AI Search (documents, images, audio, video)
2. **Retrieve** relevant passages at query time using semantic, vector, or hybrid search
3. **Construct a prompt** combining retrieved content with the user's question
4. **Generate** an answer grounded in the retrieved context

LP1 Module 5 ("Optimize generative AI model performance with Microsoft Foundry", 2 hr 11 min, 8 units) covers prompt engineering, grounding with RAG, and fine-tuning as complementary strategies — and when to combine these approaches.

### Tool-Augmented Flows

Through **function calling**, an LLM returns structured JSON specifying which function to invoke and with what parameters. This enables integration of APIs, databases, and custom logic into generative workflows. LP1 Module 4 ("Develop generative AI apps that use tools", 1 hr, 9 units) covers this pattern.

### Evaluation

The exam expects the ability to detect **fabrications** (hallucinations), measure **relevance** and **quality**, and check **safety** of outputs. Foundry provides evaluation capabilities through its Control Plane, including evaluators for agentic workflows.

### Application Integration

To connect an application to a Foundry project, use the **unified project client** (`azure-ai-projects` 2.x) against a single project endpoint. Authentication uses Azure AD (via `DefaultAzureCredential` or `ManagedIdentityCredential`). Foundry SDKs are available for **Python, C#, JavaScript/TypeScript (preview), and Java (preview)**.

---

## 2.2 Build Agents by Using Foundry

**Exam Skills Mapped**:
- Define agent roles, goals, conversation-tracking approach, and tool schemas
- Build agents that integrate retrieval, function-calling, and conversation memory
- Integrate agent tools (APIs, knowledge stores, search, content understanding, custom functions)
- Implement orchestrated multi-agent solutions
- Build autonomous or semiautonomous workflows with safeguards and approval flow controls
- Integrate monitoring into deployed agents, evaluate agent behavior, and perform error analysis

### Agent Definition

Every agent starts with a clear **role** (via system instructions), a **goal** (the task to accomplish), and a **conversation-tracking approach** (how context is maintained):

```python
from agent_framework import Agent
from agent_framework.foundry import FoundryChatClient
from azure.identity import AzureCliCredential

client = FoundryChatClient(
    project_endpoint=os.environ["FOUNDRY_PROJECT_ENDPOINT"],
    model=os.environ["FOUNDRY_MODEL"],
    credential=AzureCliCredential(),
)
agent = Agent(
    client=client,
    instructions="You are a helpful travel assistant who helps users plan trips.",
    name="travel_assistant",
)
```

This pattern comes from the official Microsoft Agent Framework samples.

### Tool Integration

Agents integrate tools through multiple mechanisms:

- **APIs** — REST endpoints auto-imported via OpenAPI specs
- **Knowledge stores** — Azure AI Search indexes, Foundry IQ
- **Content Understanding** — Multimodal analysis capabilities
- **Custom functions** — Any Python function wrapped as an `AIFunction`
- **MCP servers** — Dynamically discover and invoke external tools or data via Model Context Protocol

Dedicated learning modules cover each integration type: **custom tools** (LP2 Module 2, 53 min), **MCP tools** (LP2 Module 3, 51 min), and **Foundry IQ knowledge** (LP2 Module 4, 1 hr 11 min — covering how RAG solves the knowledge problem for agents and how Foundry IQ provides a shared knowledge platform).

### Agent Publishing

Foundry supports publishing agents to **Microsoft Teams and Microsoft 365 Copilot**, accessing workplace data with Work IQ, and testing integrated agents (LP2 Module 5, 1 hr 14 min, 9 units).

### Autonomous vs. Semiautonomous Workflows

Safeguards for agent behavior include:
- **Round or iteration limits** to prevent runaway loops
- **Tool approval modes** (e.g., `approval_mode="always_require"`)
- **Monitoring and killswitch mechanisms** to halt misbehaving agents
- **Fallback to human** when the agent encounters uncertainty or errors

---

## 2.3 Optimize and Operationalize Generative AI Systems

**Exam Skills Mapped**:
- Tune generation behavior (prompt engineering and adjusting model parameters)
- Implement model reflection, chain-of-thought evaluations, and self-critique loops
- Set up observability by implementing tracing, token analytics, safety signals, and latency breakdowns
- Orchestrate multiple models, flows, or hybrid LLM and rules engines

### Prompt Engineering & Parameter Tuning

Key parameters:

- **Temperature** (0.0–1.0) — Lower for deterministic outputs (factual Q&A); higher for creativity (brainstorming)
- **Max tokens** — Controls output length; set appropriately to avoid truncation or excessive verbosity
- **Frequency/presence penalties** — Reduce repetition in generated text
- **Stop sequences** — Prevent the model from generating text beyond its turn

### Fine-Tuning in Foundry

Fine-tuning improves results beyond prompt engineering, particularly for domain-specific behavior, shorter prompts (token savings), and lower-latency inference from smaller models. Foundry uses **LoRA (Low-Rank Adaptation)** for efficient fine-tuning.

**Supported fine-tuning models**:

| **Model ID** | **Methods** | **Status** | **Modality** |
| --- | --- | --- | --- |
| `gpt-4o-mini` (2024-07-18) | SFT | GA | Text to text |
| `gpt-4o` (2024-08-06) | SFT, DPO | GA | Text and vision to text |
| `gpt-4.1` (2025-04-14) | SFT, DPO | GA | Text and vision to text |
| `gpt-4.1-mini` (2025-04-14) | SFT, DPO | GA | Text to text |
| `gpt-4.1-nano` (2025-04-14) | SFT, DPO | GA | Text to text |
| `o4-mini` (2025-04-16) | RFT | GA | Text to text |
| `gpt-5` (2025-08-07) | RFT | Private preview | Text to text |
| `Ministral-3B` (2411) | SFT | Public preview | Text to text |
| `Qwen-32B` | SFT | Public preview | Text to text |
| `Llama-3.3-70B-Instruct` | SFT | Public preview | Text to text |
| `gpt-oss-20b` | SFT | Public preview | Text to text |

Training data must be formatted as **JSON Lines (JSONL)** in the Chat Completions conversational format, encoded in **UTF-8 with a byte-order mark (BOM)**, and each file must be **less than 512 MB**.

### Model Reflection and Self-Critique

Advanced techniques for improving output quality:

- **Chain-of-thought prompting:** Instruct the model to reason step by step, making errors more visible and correctable
- **Self-reflection:** Prompt the model to check its own answer: *"Verify your response for correctness and completeness before finalizing."*
- **Evaluator model:** Use a second model call to judge the first model's answer, providing a second opinion before delivering to the user

> [!NOTE]
> *These techniques double model usage (and thus cost and latency). Use them when accuracy is more important than speed — such as financial calculations or compliance checks.*

### Hybrid LLM + Rules Engines

Use LLMs for understanding intent and generating natural language; delegate deterministic tasks (math, database lookups, structured validation) to traditional code or APIs. This combination improves reliability, reduces cost, and avoids hallucination where precision is required. The exam may present scenarios where a purely AI solution is suboptimal, expecting you to incorporate a deterministic component.

---

## 2.4 Agent Orchestration Patterns — Deep Dive

The **Microsoft Agent Framework** provides five multi-agent orchestration patterns. Implementing orchestrated multi-agent solutions is explicitly tested on the exam.

---

### Pattern 1: Sequential Orchestration — *"Assembly Line"*

**What it is:** Agents are organized in a pipeline. Each agent processes the task in turn, passing its output to the next. The full conversation history from previous agents is passed to each subsequent agent.

**When to use it:** Tasks that break into clear ordered stages — document review, data processing pipelines, or multi-stage reasoning.

> [!NOTE]
> *Analogy (conceptual — not from official docs): An assembly line where each station adds something new to the product.*

**What you'll learn** (per official docs):
- Create a sequential pipeline of agents
- Chain agents where each builds upon previous output
- Add human-in-the-loop approval for sensitive tool calls
- Mix agents with custom executors for specialized tasks
- Track conversation flow through the pipeline

**Python example** (from the official Agent Framework repository):

```python
import asyncio, os
from typing import cast
from agent_framework import Agent, Message
from agent_framework.foundry import FoundryChatClient
from agent_framework.orchestrations import SequentialBuilder
from azure.identity import AzureCliCredential

async def main() -> None:
    client = FoundryChatClient(
        project_endpoint=os.environ["FOUNDRY_PROJECT_ENDPOINT"],
        model=os.environ["FOUNDRY_MODEL"],
        credential=AzureCliCredential(),
    )
    writer = Agent(client=client,
        instructions="You are a concise copywriter. Provide a single, punchy marketing sentence.",
        name="writer")
    reviewer = Agent(client=client,
        instructions="You are a thoughtful reviewer. Give brief feedback on the previous message.",
        name="reviewer")

    workflow = SequentialBuilder(participants=[writer, reviewer]).build()

    outputs = []
    async for event in workflow.run("Write a tagline for a budget-friendly eBike.", stream=True):
        if event.type == "output":
            outputs.append(cast(list[Message], event.data))

    if outputs:
        for i, msg in enumerate(outputs[-1], start=1):
            name = msg.author_name or ("assistant" if msg.role == "assistant" else "user")
            print(f"  {i:02d} [{name}]\n  {msg.text}")

if __name__ == "__main__":
    asyncio.run(main())
```

---

### Pattern 2: Concurrent Orchestration — *"Team in Parallel"*

**What it is:** Multiple agents work on the same task **in parallel**. Each processes the input independently, and their results are collected and aggregated.

**When to use it:** Diverse perspectives, brainstorming, ensemble reasoning, or voting systems.

> [!NOTE]
> *Analogy (conceptual): A panel of experts working simultaneously on the same question, then comparing answers.*

**What you'll learn**:
- Define multiple agents with different expertise
- Orchestrate agents to work concurrently on a single task
- Collect and process aggregated results

```python
from agent_framework import Agent
from agent_framework.orchestrations import ConcurrentBuilder

french = Agent(client=client, instructions="Translate the input to French.", name="french")
spanish = Agent(client=client, instructions="Translate the input to Spanish.", name="spanish")
german = Agent(client=client, instructions="Translate the input to German.", name="german")

workflow = ConcurrentBuilder(participants=[french, spanish, german]).build()

async for event in workflow.run("Hello, how are you today?", stream=True):
    if event.type == "output":
        print(event.data)
```

> [!NOTE]
> *This code is illustrative, adapted from the concurrent translation example in the official orchestration documentation.*

---

### Pattern 3: Handoff Orchestration — *"Expert Routing"*

**What it is:** Agents transfer control to one another based on context. Internally uses a **mesh topology** where agents connect directly without a central orchestrator.

**When to use it:** Customer support (general → specialist), expert systems, dynamic delegation.

> [!NOTE]
> *Analogy (conceptual): A call center where a customer is transferred from a general agent to a billing specialist.*

**Key differences from Agent-as-Tools** (important for the exam):

| **Dimension** | **Handoff** | **Agent-as-Tools** |
| --- | --- | --- |
| **Control Flow** | Control explicitly passed between agents; no central authority | Primary agent delegates subtasks; control returns to primary |
| **Task Ownership** | Receiving agent takes **full ownership** | Primary agent retains **overall responsibility** |
| **Context Management** | Full conversation handed over entirely | Primary agent manages context; provides only relevant info |

**What you'll learn**:
- Create specialized agents for different domains
- Configure handoff rules between agents
- Build interactive workflows with dynamic agent routing
- Handle multi-turn conversations with agent switching
- Implement tool approval for sensitive operations (HITL)
- Use checkpointing for durable handoff workflows

> [!IMPORTANT]
> Handoff orchestration only supports `Agent` types and agents must support local tool execution.

---

### Pattern 4: Group Chat Orchestration — *"Roundtable Discussion"*

**What it is:** Multiple agents in a collaborative conversation, coordinated by an **orchestrator** that determines speaker selection and conversation flow. Internally uses a **star topology** with an orchestrator in the middle.

**When to use it:** Iterative refinement, collaborative problem-solving, multi-perspective analysis.

**Key differences from other patterns**:
- **Centralized Coordination:** Uses an orchestrator (unlike handoff's direct transfers)
- **Iterative Refinement:** Agents review and build on each other's responses across multiple rounds
- **Flexible Speaker Selection:** Round-robin, prompt-based, or custom logic
- **Shared Context:** All agents see the full conversation history

**Two speaker selection approaches** (mutually exclusive — use one or the other, not both):

*Pattern A — Function-based selection*:

```python
from agent_framework import GroupChatBuilder, GroupChatStateSnapshot

def select_next_speaker(state: GroupChatStateSnapshot) -> str | None:
    if state["round_index"] >= 5:
        return None  # Finish after 5 rounds
    last_speaker = state["history"][-1].speaker if state["history"] else None
    if last_speaker == "researcher":
        return "writer"
    return "researcher"

workflow = (
    GroupChatBuilder()
    .set_select_speakers_func(select_next_speaker)
    .participants([researcher_agent, writer_agent])
    .build()
)
```

*Pattern B — LLM-based manager selection*:

```python
manager_agent = AzureOpenAIChatClient().create_agent(
    instructions="Coordinate the conversation and pick the next speaker.",
    name="Coordinator", temperature=0.3, seed=42, max_tokens=500,
)
workflow = (
    GroupChatBuilder()
    .set_manager(manager_agent, display_name="Coordinator")
    .participants([researcher, writer])
    .with_max_rounds(10)
    .build()
)
```

> [!WARNING]
> Function-based and manager-based approaches are **mutually exclusive**. Using both will conflict.

---

### Pattern 5: Magentic Orchestration — *"Dynamic Manager"*

**What it is:** A **manager agent** dynamically coordinates specialized agents (and sometimes humans) for complex, open-ended problems. The manager builds and refines a **dynamic task ledger**, assigning subtasks as needed.

**When to use it:** Very complex tasks with unknown sub-problems requiring dynamic planning and decomposition.

> [!NOTE]
> *Analogy (conceptual): A project manager who hires consultants on the fly — figuring out what needs to be done, bringing in specialists, and compiling their work dynamically.*

**Status:** The Python `MagenticBuilder` is included in the `agent-framework-orchestrations` package. The .NET version will support Magentic in an upcoming release.

---

### Orchestration Pattern Comparison

| **Pattern** | **Topology** | **Communication** | **Best For** | **Exam Keyword** |
| --- | --- | --- | --- | --- |
| **Sequential** | Pipeline | Linear; each sees full history | Multi-stage processing | *"pipeline, step-by-step"* |
| **Concurrent** | Fan-out/fan-in | Independent; results aggregated | Diverse perspectives | *"parallel, brainstorming"* |
| **Handoff** | Mesh (peer-to-peer) | Direct transfer of full context | Expert routing | *"escalation, specialist"* |
| **Group Chat** | Star (orchestrator center) | All see shared conversation | Iterative refinement | *"collaborative, roundtable"* |
| **Magentic** | Manager + workers | Manager assigns dynamically | Complex open-ended goals | *"dynamic planning, task ledger"* |

---

**← Previous:** Chapter1-PlanManage.md | **README.md** | **Next →** Chapter3-ComputerVision.md