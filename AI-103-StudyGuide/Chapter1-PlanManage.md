*← README.md*

# Chapter 1: Plan and Manage an Azure AI Solution (25–30%)

This domain establishes the foundation for all subsequent topics. Exam questions in this area account for **25–30%** of the total【9†L60-L66】 and test your ability to choose services, set up infrastructure, manage operations, and enforce Responsible AI practices across the entire solution lifecycle【9†L68-L94】.

---

## 1.1 Choose the Appropriate Foundry Services for Generative AI and Agents

**Exam Skills Mapped**【9†L68-L73】:
- Choose an appropriate model for each task (LLMs, small language models, multimodal models, and Foundry Tools)
- Choose the appropriate Foundry services for generative tasks, grounding, vector search, agent workflows, or multimodal processing
- Choose an appropriate method for retrieval and indexing
- Choose appropriate memory, tool, and knowledge integration services for agent solutions

### Model Selection Decision Framework

Match **task complexity and modality** to the **right-sized model**:

- **Large Language Models (LLMs)** (e.g., GPT-4 series) — Best for complex reasoning, open-ended generation, and multi-step planning. *Trade-off:* higher latency and cost.
- **Small Language Models** (e.g., `gpt-4.1-nano`) — Suitable for simpler text classification, entity extraction, or routine conversation where speed is more important than depth【39†L54】. *Trade-off:* limited reasoning depth and creativity.
- **Code Models** — Specialized for code generation and analysis tasks (e.g., writing functions from a specification or performing code review).
- **Multimodal Models** (e.g., `gpt-4.1` which accepts text and vision input)【39†L54】 — Required when the solution needs to process images alongside text (visual question answering, document analysis with embedded images).
- **Foundry Tools** (prebuilt cognitive services) — When an Azure service already handles the task (e.g., Azure Translator for translation, Azure Speech for STT/TTS, Document Intelligence for forms), use the dedicated tool rather than prompting an LLM. These tools are typically faster, cheaper, and more deterministic for their specific tasks【12†L48-L50】.

### Retrieval & Indexing Methods

| **Method** | **Best For** | **How It Works** |
| --- | --- | --- |
| **Keyword/BM25 Search** | Precise term matching, structured queries | Traditional text search on indexed keyword fields (exact matches) |
| **Semantic Search** | Intent-based retrieval when exact terms differ | ML-driven relevance ranking that understands context/meaning |
| **Vector Search** | Conceptually similar content or cross-language retrieval | Cosine similarity on numerical embeddings |
| **Hybrid Search** | Balancing precision and recall | Combines keyword and vector scores for robust retrieval |

> [!TIP]
> **Hybrid search** often provides the most robust results by combining precise keyword matching with semantic ranking. It is a strong default approach when requirements are uncertain.

### Memory, Tool, and Knowledge Integration

- **Foundry IQ** — A shared knowledge integration platform that multiple agents can access, delivering **citation-backed answers** grounded in enterprise or web content【13†L59】.
- **Memory** — Foundry retains contextual information across interactions without repeated input【13†L58】. The Agent Framework supports pluggable memory backends including **Redis, Pinecone, Qdrant, Weaviate, Elasticsearch, and Postgres**【17†L96】.
- **Tool Integration** — Foundry offers a catalog of over **1,400 tools** through public and private catalogs【13†L57】, plus any custom functions you define and register. Tools can be Azure services (Vision, Speech, Search, etc.), REST APIs, or custom code.

---

## 1.2 Set Up AI Solutions in Foundry

**Exam Skills Mapped**【9†L75-L80】:
- Design Azure infrastructure for AI apps and agent-based solutions
- Choose appropriate deployment options
- Configure model and agent deployments
- Integrate Foundry projects with continuous integration and continuous deployment (CI/CD) pipelines

### Azure Infrastructure Design

**Microsoft Foundry** is a unified Azure platform-as-a-service offering for enterprise AI operations, model builders, and application development【13†L18-L20】. A single **Foundry resource** contains one or more **projects** that manage models, agents, and tools under unified RBAC, networking, and policies【13†L20】. The Foundry portal is accessible at **ai.azure.com**【13†L81】.

Existing Azure OpenAI resources can be upgraded to Foundry resources while preserving endpoints, API keys, and existing state【13†L24】. The unified SDK (`azure-ai-projects` 2.x) plus `OpenAI()` connects against a **single project endpoint**, replacing the previous approach of multiple packages against 5+ endpoints【13†L41】.

### Deployment Options

- **Cloud (Managed)** — Deploy models and agents via the Foundry portal or SDK. Configure the deployment name, model version, and scale settings. For agents, configure the system prompt (instructions), available tools, and memory settings.
- **Foundry Local** — Run LLMs on local devices for free【12†L58-L60】, useful for latency-sensitive or data-sovereignty scenarios.

### CI/CD Integration

Foundry projects can be managed as Azure resources and integrated with CI/CD pipelines using Azure CLI, the Foundry SDK, or infrastructure-as-code tools【9†L80】. This enables:
- Automated model deployments and agent publishes as part of release pipelines
- Prompt versioning and agent configuration management under source control
- Automated smoke tests that call the model or agent with sample inputs after each deployment

---

## 1.3 Manage, Monitor, and Secure AI Systems

**Exam Skills Mapped**【9†L82-L87】:
- Manage quotas, scaling, rate limits, and cost footprints for model and agent workloads
- Monitor model performance, drift, safety events, and grounding quality
- Monitor data ingestion quality, search index health, and relevance performance
- Configure security, including managed identity, private networking, keyless credentials, and role policies

### Resource Management & Cost Control

Azure imposes quotas on Foundry model usage (requests per minute, token limits). Fine-tuning requires the **Azure AI Owner** role; AI Users may train models but only AI Owners may deploy them【39†L41】. Monitor token usage telemetry to attribute cost and identify excessively large prompts.

### Monitoring & Drift

Foundry provides **real-time observability** with built-in metrics and model tracking【13†L64】. Key monitoring targets:

- **Latency & Throughput** — Model response times and query-per-second capacity
- **Model Drift** — Periodic evaluation with test datasets to detect degrading quality over time
- **Safety Events** — Content filter triggers and unsafe output detections (when content filters remove or alter a model output)
- **Grounding Quality** — Whether agent responses are supported by source content. Foundry's Content Safety includes a **Groundedness detection** API (in preview) for this purpose【38†L42】
- **Search Index Health** — Indexer success rates, document counts, and relevance scoring

### Security Configuration

- **Managed Identity** — Authenticate agents and services to other Azure resources without embedded keys【9†L87】
- **Keyless Credentials** — Use `DefaultAzureCredential` for development; in production, switch to a specific credential (e.g., `ManagedIdentityCredential`) to avoid latency and security risks from fallback mechanisms【26†L88-L91】
- **Private Networking** — Use Private Endpoints and Virtual Network integration for isolated environments
- **RBAC Role Policies** — Assign appropriate Foundry roles (e.g., AI Owner, Contributor, Reader) to control who can deploy or consume models

---

## 1.4 Implement Responsible AI Across Generative AI and Agentic Systems

**Exam Skills Mapped**【9†L89-L94】:
- Configure safety filters, guardrails, risk detection, and content moderation
- Apply responsible AI instrumentation, including evaluators, safety evaluations, and explanation tooling
- Implement auditing through trace logging, provenance metadata, and approval workflows
- Govern agent behavior with oversight modes, constraints, and tool-access controls

### Azure AI Content Safety

| **Category** | **Features** |
| --- | --- |
| **AI Safety & Prompt Protection** | **Prompt Shields** (detect user input attacks on LLMs)【38†L41】, **Groundedness detection** (check if LLM responses are grounded in source materials)【38†L42】, **Protected material text detection** (scan for known copyrighted content)【38†L43】, **Task adherence API** (detect misaligned tool use by agents)【38†L44】 |
| **Content Analysis** | **Analyze text API** (classify sexual, violence, hate, self-harm at multi-severity levels)【38†L52】, **Analyze image API** (same categories for images)【38†L53】 |
| **Custom Detection** | **Custom categories (standard)** for training domain-specific classifiers【38†L61】, **Custom categories (rapid)** for emerging patterns across text and images【38†L62】 |

**Content Safety Studio** is an online tool at contentsafety.cognitive.azure.com for testing moderation, customizing workflows, and monitoring KPIs like block rate, severity distribution, and latency【38†L82-L88】.

### Auditing and Trace Logging

Use Foundry's **Trace SDK** for logging all agent interactions, decisions, and tool invocations【12†L40】. Record **provenance metadata** — which documents or sources grounded each response — so answers can be audited and explained【9†L93】.

### Human Oversight & Agent Governance

The exam specifically requires knowledge of oversight modes, constraints, and tool-access controls【9†L94】:

- **Human-in-the-Loop (HITL):** In the Agent Framework, set `approval_mode="always_require"` on sensitive tools. The workflow pauses and emits a `request_info` event when the agent tries to call that tool, requiring human confirmation before proceeding.
- **Iteration Limits:** Set maximum rounds or iterations to prevent runaway autonomous loops.
- **Tool Access Controls:** Register only the tools the agent needs. An agent that only retrieves information should not have tools that modify production data.
- **Continuous Improvement:** Gather feedback, retrain models if they show bias, and iterate on agent behavior proactively.

---

**← README.md** | **Next →** Chapter2-GenerativeAgents.md