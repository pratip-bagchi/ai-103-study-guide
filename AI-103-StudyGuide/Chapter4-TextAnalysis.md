*← README.md*

# Chapter 4: Implement Text Analysis Solutions (10–15%)

This domain covers natural language processing using both Azure's prebuilt services and LLM-based approaches, plus speech capabilities. It accounts for **10–15%** of exam questions.

---

## 4.1 Apply Language Model Text Analysis

**Exam Skills Mapped**:
- Implement solutions to extract entities, topics, summaries, and structured JSON outputs by using generative prompting and Foundry Tools
- Configure detection of sentiment, tone, safety issues, and sensitive content
- Build solutions that translate text using Azure Translator in Foundry Tools or LLM-powered translation flows
- Customize language model outputs for domain tasks (compliance summarization, domain extraction)

### Azure Language in Foundry Tools

Pre-built NLP capabilities (LP3 Module 1: "Analyze text with Azure Language in Foundry Tools", 46 min, 8 units) include:

- **Language detection** — Identify the language of input text
- **Entity extraction** — Find named entities (people, organizations, locations)
- **PII recognition** — Detect personally identifiable information for redaction

### Prebuilt Tools vs. LLM-Based Analysis

| **Consideration** | **Foundry Tools (Prebuilt APIs)** | **LLM Prompting** |
| --- | --- | --- |
| **Speed** | Faster (specialized inference) | Slower (general-purpose model) |
| **Cost** | Typically cheaper per call | Higher cost per token |
| **Determinism** | Highly deterministic outputs | Varies across runs |
| **Flexibility** | Fixed task definitions | Handles custom/multi-task extraction in one call |
| **Best for** | Well-defined, repeatable tasks | Flexible, domain-specific, or novel extraction needs |

> [!TIP]
> If the task aligns with an existing Azure service (entity recognition, PII detection, translation), prefer the dedicated tool. Use an LLM when the task requires reasoning that the API cannot provide, or when you need multiple analyses in a single call.

### MCP Integration for Text Analysis

LP3 Module 2 ("Develop a text analysis agent with the Azure Language MCP server", 52 min, 6 units) teaches building an AI agent that uses the **Azure Language MCP server** to perform text analysis tasks like language detection, entity recognition, and personal information redaction — demonstrating how agents invoke Foundry Tools through the Model Context Protocol.

### Translation

- **Azure Translator** (Foundry Tool) — Broad language coverage and high accuracy for direct translation
- **LLM-powered translation** — Better when translation must be combined with contextual rewriting or summarization in a single step

LP3 Module 7 ("Translate text and speech with Microsoft Foundry Tools", 48 min, 7 units) covers both approaches.

### Customizing Outputs for Domain Tasks

For domain-specific extraction (e.g., compliance summarization, extracting chemical names):
- **Few-shot prompt an LLM** with examples of the desired output format, especially for structured JSON
- **Fine-tune a model** if volume justifies the investment (see Chapter 2 Section 2.3 for supported fine-tuning models)
- **Post-process** prebuilt API outputs to fit your schema (e.g., mapping generic "Product" entities to domain-specific "DrugName" categories)

---

## 4.2 Implement Speech Solutions

**Exam Skills Mapped**:
- Implement workflows to convert speech to text and text to speech for agentic interactions
- Integrate speech as an agent modality, including custom speech models
- Enable multimodal reasoning from audio inputs
- Translate speech into other languages using language models and Foundry Tools

### Azure Speech in Foundry Tools

LP3 Modules 3–6 cover the full speech capability set:

| **Module** | **Focus** | **Duration** | **Units** |
| --- | --- | --- | --- |
| Develop a speech-capable generative AI application | Models that transcribe and synthesize speech | 43 min | 7 |
| Create speech-enabled apps with Azure Speech | STT and TTS APIs | 53 min | 9 |
| Develop a speech agent with the Azure Speech MCP server | Building agents with Speech MCP for STT/TTS | 52 min | 6 |
| Develop an Azure Speech Voice Live Agent | Voice Live API and SDK for conversational agents | 52 min | 7 |

### Custom Speech Models

**Custom speech models** improve recognition accuracy for domain-specific terms by training on specialized audio data or providing custom vocabulary lists. Use them when transcriptions frequently miss company-specific proper nouns, industry jargon, or accented speech patterns.

### Speech Translation Pipeline

The standard pipeline: **Speech-to-Text → Azure Translator (or LLM) → Text-to-Speech**. Azure also provides a direct Speech Translation API. The exam explicitly states both LLM-powered and Foundry Tools approaches are in scope.

> [!NOTE]
> *Direct LLM speech translation (audio in → text out in another language) is not available as a single end-to-end step. Speech must always be transcribed to text first (via STT), then processed by the LLM or Translator, then optionally synthesized back to speech (via TTS).*

---

**← Previous:** Chapter3-ComputerVision.md | **README.md** | **Next →** Chapter5-InformationExtraction.md