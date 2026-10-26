# Memory

| Scope | Behavior |
|-------|----------|
| session | Memory exists only for the current session ✅ |
| {{$conversationId}} | Memory tied to a conversation/thread |
| {{$userId}} | Memory persists across sessions for a user |
| global | Shared/global memory |

# Deployment Types

### Quick Decision Guide

| Requirement | Recommended Deployment Type |
|------------|----------------------------|
| General-purpose production chatbot | **Global Standard** |
| Highest scale with no compliance restrictions | **Global Standard** |
| Consistent heavy traffic with predictable performance | **Global Provisioned** |
| Millions of records processed overnight | **Global Batch** |
| EU/US data residency requirement | **Data Zone Standard** |
| EU/US data residency + guaranteed throughput | **Data Zone Provisioned** |
| EU/US data residency + overnight batch jobs | **Data Zone Batch** |
| Must stay in a specific Azure region | **Standard (Regional)** |
| Specific region + guaranteed throughput | **Regional Provisioned** |
| Testing fine-tuned models | **Developer** |

---

### Easy Memory Tricks

| Keyword | Meaning |
|----------|---------|
| **Standard** | Shared capacity, pay-as-you-go |
| **Provisioned** | Reserved capacity, predictable performance |
| **Batch** | Large asynchronous workloads, lower cost |
| **Global** | Maximum scale across Azure infrastructure |
| **Data Zone** | Data remains within a geographic zone (EU/US) |
| **Regional** | Data remains in a specific Azure region |
| **Developer** | Testing and evaluation only |

---

### Certification Exam Cheatsheet

| Scenario | Answer |
|-----------|--------|
| "Normal chatbot" | **Global Standard** |
| "Highest quota available" | **Global Standard** |
| "Predictable flash-sale traffic" | **Global Provisioned** |
| "Analyze millions of reviews overnight" | **Global Batch** |
| "EU compliance" | **Data Zone Standard** |
| "EU compliance + flash-sale traffic" | **Data Zone Provisioned** |
| "EU compliance + overnight processing" | **Data Zone Batch** |
| "Healthcare/Government regulation requiring a specific Azure region" | **Regional Standard or Regional Provisioned** |
| "Evaluate a fine-tuned model" | **Developer** |

### Choosing an endpoint and SDK

| Endpoint | Foundry Project<br>`https://{resource-name}.services.ai.azure.com/api/projects/{project-name}` | Azure OpenAI<br>`https://{resource-name}.openai.azure.com/openai/v1` |
| :--- | :--- | :--- |
| **SDK** | **Microsoft Foundry SDK**<br>`pip install azure-ai-projects openai` | **OpenAI SDK**<br>`pip install openai` |
| **Authentication** | **Microsoft Entra ID**<br>`pip install azure-identity` | **Microsoft Entra ID or API key**<br>`pip install azure-identity` |
| **Chat API** | **Responses API**<br>Foundry direct models only | **Responses or ChatCompletions API**<br>All Foundry models |
| **Best for** | Foundry-specific capabilities with OpenAI-compatible interfaces for Foundry direct models | Latest OpenAI SDK models and features with the full OpenAI API surface |

### Code Example
#### Foundry Code
```python
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient
from openai import OpenAI

project_client = AIProjectClient(
    credential=DefaultAzureCredential(),
    endpoint=FOUNDRY_PROJECT_ENDPOINT
)

openai_client = project_client.get_openai_client(
    api_version="2024-10-21"
)
```

#### OpenAI
```python
from openai import OpenAI

openai_client = OpenAI(
    api_key=API_KEY_OR_TOKEN,
    base_url=AZURE_OPENAI_ENDPOINT
)
```


### Compare ChatCompletions with Responses API

| Feature | ChatCompletions API | Responses API |
| :--- | :--- | :--- |
| **State management** | Client-side - developer stores and sends the full messages array | Server-side - API stores the thread automatically |
| **System prompt** | First entry in messages array:<br>`{"role": "system", "content": "..."}` | Dedicated parameter:<br>`instructions="..."` |
| **Payload per request** | Grows with conversation length | Constant - only latest input + an ID |
| **Accessing the response** | `completion.choices[0].message.content` | `response.output_text` |

### Choosing Between Foundry SDK and OpenAI SDK

| Aspect | Foundry SDK | OpenAI SDK |
| :--- | :--- | :--- |
| **Primary Focus** | Deep integration with Foundry-specific platform capabilities | Maximum OpenAI API compatibility and code portability |
| **Core Capabilities & Services** | • Foundry Agent Service for creating and managing AI agents<br>• Tool invocation and approval workflows<br>• Project metadata, connections, and governance | • Native support for Chat Completions, Responses, and Images APIs<br>• Minimal dependency on Foundry-specific concepts |
| **Model Support** | Foundry direct models (non-Azure OpenAI models available via the model catalog) | Models compatible with the standard OpenAI API surface |
| **Evaluation & Observability** | • Cloud evaluations for testing and validating AI responses<br>• Built-in tracing and observability for application monitoring | Relies on standard OpenAI tooling or third-party observability solutions |
| **Portability & Integration** | Best for applications deeply coupled to Azure/Foundry infrastructure | High portability between standard OpenAI and Azure OpenAI deployments |
| **Recommended Use Cases** | Building multi-agent systems, automated workflows, enterprise governance, or apps requiring native evaluations | Migrating existing OpenAI codebases, lightweight API calls, or multi-cloud deployments |

### Configure data sources

| Category | Features / Badges | Data Source | Best For |
| :--- | :--- | :--- | :--- |
| **Indexed** | Pre-processed · High relevance · Custom | **Azure AI Search Index** | Enterprise search with custom ingestion pipelines and AI enrichment |
| | | **SharePoint (Indexed)** | Advanced search on SharePoint content with custom query pipelines |
| **Direct** | No index needed · Raw file access | **Azure Blob Storage** | Document files in Azure Storage — PDFs, DOCX, text, and more |
| | | **OneLake** | Unstructured data and documents in Microsoft Fabric's unified data lake |
| **Real-time** | Always current · Live retrieval · No data copy | **Web** | Current public information retrieved via Bing at query time |
| | | **SharePoint (Remote)** | Live SharePoint content with Microsoft 365 governance and permissions |

# Computer Vision
### Spotlighting 
Spotlighting marks and separates untrusted third-party content so the model treats it as lower-trust data, helping defend against indirect prompt injection attacks embedded in documents or images.

### Prompt Shields vs Spotlighting 
Prompt Shields for Documents acts as a detection system that scans and blocks hidden attacks, while Spotlighting transforms the document's content to reduce the AI model's trust in it.

### input_fidelity
When you see requirements such as:
- Preserve a product, logo, or subject
- Maintain visual characteristics from a reference image
- Keep identity consistent across generated images

Choose input_fidelity = high

### Image moderation
Image moderation is designed to automatically analyze uploaded images for harmful content and classify them into safety categories with severity levels. You can then configure the system to block content based on those severity thresholds before the images are processed by your application.

### Spatial Analysis

Requirements:
 - Monitor a live video stream
 - Determine whether more than one person is present
 - Detect occupancy/presence
- Minimize development effort

Designed to analyze video streams and can:
- Detect people
- Count people
- Track movement
- Monitor occupancy in an area

### Formula

![Precision Formula](https://latex.codecogs.com/svg.latex?\text{Precision}=\frac{\text{True%20Positives}}{\text{True%20Positives}+\text{False%20Positives}})

![Recall Formula](https://latex.codecogs.com/svg.latex?\text{Recall}=\frac{\text{True%20Positives}}{\text{True%20Positives}+\text{False%20Negatives}})

### Project Type vs Domains
| Requirement | Project Type | Domain |
|-------------|-------------|---------|
| Cat vs Dog Classification | Classification | General |
| Defective vs Good Product | Classification | General |
| Locate Defect Position | Object Detection | General |
| Famous Buildings / Landmarks | Classification | Landmarks |
| Restaurant Dishes / Food Recognition | Classification | Food |
| Shopping Catalog Products | Classification | Retail |
| Detect Company Logos | Object Detection | Logo |
| Retail Shelf Inventory | Object Detection | Products on Shelves |
| Offline / Edge Deployment | Any Project Type | Compact Domain |

# Speech
### Services
- Live audio → text = Real-time Speech-to-Text
- Text → spoken response = Text-to-Speech
- Recorded audio files = Batch Transcription
- Audio translated to another language = Speech Translation

# Content Understanding
### Analyzers
 - OCR only → prebuilt-read
 - Tables, structure, sections, layout preservation → prebuilt-layout
 - Schema discovery → prebuilt-documentFieldSchema
 - RAG ingestion/search → prebuilt-documentSearch

### Advanced data parsing
Advanced data parsing is specifically intended for complex documents and uses layout-aware processing to preserve document structure, including headings, tables, and metadata, while supporting OCR for scanned content. This creates higher-quality chunks for RAG retrieval

### Text Split
Text Split is used to chunk large documents into smaller passages suitable for indexing and embedding generation. It is commonly used before vectorization in RAG pipelines.

### Azure OpenAI Embedding
Azure OpenAI Embedding generates vector embeddings for those chunks, enabling vector similarity search and semantic retrieval.

### Evalautors
- Groundedness: measures whether the generated response is supported by the retrieved context and detects content that is not grounded in the source documents.
- Retrieval: assesses the quality and relevance of the retrieved context for a query. It is designed to evaluate whether retrieval results match the expected grounding data and is part of the RAG evaluation triad
- Fluency: Measures readability and language quality
- Response Completeness: Measures whether the response includes all expected information

# OCR and Document Processing Decision Guide

| Requirement | Recommended Service |
|-------------|---------------------|
| OCR from images/scanned pages | ✅ Read API |
| Extract text from photographs | ✅ Read API |
| Extract text from PDFs | ✅ Read API or Document Intelligence |
| Extract fields from invoices | ✅ Document Intelligence |
| Extract fields from receipts | ✅ Document Intelligence |
| Extract fields from forms | ✅ Document Intelligence |
| Extract tables from documents | ✅ Document Intelligence |
| Image tagging | ✅ Vision Image Analysis |
| Image caption generation | ✅ Vision Image Analysis |
| Object detection in images | ✅ Vision Image Analysis |
| Sentiment analysis | ✅ Language |
| Named Entity Recognition (NER) | ✅ Language |
| Key phrase extraction | ✅ Language |
| Language detection | ✅ Language |
| Question Answering | ✅ Language |
