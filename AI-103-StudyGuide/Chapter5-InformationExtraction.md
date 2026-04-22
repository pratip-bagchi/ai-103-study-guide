*← README.md*

# Chapter 5: Implement Information Extraction Solutions (10–15%)

This domain covers building the data infrastructure that feeds knowledge into agents and applications. It accounts for **10–15%** of exam questions【9†L60-L66】.

---

## 5.1 Build Retrieval and Grounding Pipelines

**Exam Skills Mapped**【9†L168-L175】:
- Ingest and index content (documents, images, audio, video)
- Configure semantic search, hybrid search, and vector search for grounding
- Implement enrichment using custom or built-in skills for text, images, and layout
- Configure RAG ingestion flow, including documents and using OCR
- Connect retrieval pipelines directly to workflows and agent tools

### Azure AI Search

The primary service for building retrieval pipelines. LP4 Module 8 ("Create a knowledge mining solution with Azure AI Search", 1 hr 8 min, 9 units)【21†L165-L171】 covers end-to-end: indexing data, applying enrichment skills, and making content searchable for deeper analysis.

### Enrichment Pipeline

Azure AI Search indexers invoke **built-in cognitive skills** (OCR, text analytics, image analysis) or **custom skills** (Azure Functions or custom models) during ingestion to annotate content with extracted metadata【9†L172】.

**Example pipeline for scanned documents:**
1. Scanned PDF ingested from Azure Blob Storage
2. **OCR skill** extracts text from images/pages
3. **Entity recognition skill** identifies people, organizations, locations
4. **Key phrase extraction skill** captures main topics
5. **Results stored** in enriched search index fields

For structured forms, consider using **Azure AI Document Intelligence** (below) instead of or in addition to Search enrichment skills.

### Search Configuration for Grounding

- **Semantic Ranking:** Azure AI Search offers semantic capabilities that re-rank results using language models for better relevance
- **Vector Search:** Create embeddings of each document or chunk (via an embedding model) and store them in the index as a vector field. At query time, match by cosine similarity
- **Hybrid Search:** Azure Search supports combining keyword and vector search in a single query for fused result sets

### Connecting to Agents

Register the search index as a knowledge source via **Foundry IQ**, which provides citation-backed answers grounded in enterprise or web content【13†L59】. LP2 Module 4 ("Build knowledge-enhanced AI agents with Foundry IQ", 1 hr 11 min, 8 units)【19†L72-L78】 covers:

- How RAG solves the knowledge problem for agents
- How Foundry IQ provides a shared knowledge platform that multiple agents can access
- How to improve retrieval quality through data optimization
- How to configure agent instructions for consistent, cited responses

Alternatively, implement retrieval as an **agent tool** (e.g., a `searchDocs(query)` function that the agent can invoke when it needs information). This gives the agent control over when to search rather than always retrieving before every question.

---

## 5.2 Extract Content from Documents

**Exam Skills Mapped**【9†L176-L181】:
- Extract information using multimodal pipelines combining OCR, layout analysis, and field extraction
- Produce clean, grounded representations to use with agents and RAG by using Content Understanding
- Implement analyzers for generating structured or markdown outputs for downstream reasoning by using Content Understanding

### Azure AI Document Intelligence

Uses OCR and deep learning models to extract text, key-value pairs, tables, and structured data from forms and documents (LP4 Module 7: "Extract data with Azure Document Intelligence", 1 hr 11 min, 8 units)【21†L149-L155】. It provides:

- **Prebuilt models** — For invoices, receipts, business cards, IDs, and other standard document types
- **Custom models** — Trained on your own labeled forms for domain-specific extraction
- **General document mode** — Pulls key-value pairs and tables from any document
- **Layout mode** — Returns raw text with positional coordinates

### Content Understanding Pipelines

Content Understanding supports building **custom analyzers** that combine multiple extraction steps — OCR, layout analysis, and field extraction — all producing structured or markdown outputs ready for downstream reasoning by agents or LLMs【9†L180-L181】.

- **Single-task mode:** Run one pre-configured analysis (e.g., just OCR or just classification)
- **Pro mode:** Chain multiple steps in sequence (e.g., classify document type → extract fields using the appropriate model → translate extracted text)

### End-to-End Information Extraction Workflow

1. **Ingest** raw content (PDFs, scanned images, audio, video) into Azure storage
2. **Process** with Content Understanding or Document Intelligence to extract structured data
3. **Index** results in Azure AI Search with enrichment skills
4. **Connect** the search index to agents via Foundry IQ for citation-backed retrieval
5. **Monitor** ingestion quality and search relevance continuously (see Chapter 1 Section 1.3 for monitoring guidance)

---

**← Previous:** Chapter4-TextAnalysis.md | **README.md** | **Next →** Appendix-Resources.md