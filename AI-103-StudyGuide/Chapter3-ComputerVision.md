*← README.md*

# Chapter 3: Implement Computer Vision Solutions (10–15%)

This exam domain covers AI solutions that create or analyze visual content — images and videos — and ensuring they are used responsibly. It accounts for **10–15%** of exam questions【9†L60-L66】.

---

## 3.1 Design and Implement Image- and Video-Generation Solutions

**Exam Skills Mapped**【9†L125-L132】:
- Implement solutions that generate images from text prompts and reference media
- Implement solutions that generate videos from text prompts and reference media
- Configure image-editing workflows, including inpainting, mask-based edits, and prompt-driven modifications
- Implement workflows to edit generated videos
- Select and apply appropriate generation and editing controls provided by the platform

### Image Generation Models in Foundry

> [!WARNING]
> The DALL-E 3 model (`dall-e-3`) was **retired on March 4, 2026** and is no longer available for new deployments【34†L20】. Use the GPT-Image family instead.

| **Aspect** | **GPT-Image-1.5** | **GPT-Image-1** | **GPT-Image-1-Mini** |
| --- | --- | --- | --- |
| **Best for** | Realism, instruction following, speed/cost balance | Realism, instruction following, multimodal context | Fast prototyping, bulk generation, cost-sensitive |
| **Input/Output** | Text + image → base64 | Text + image → base64 | Text + image → base64 |
| **Resolutions** | 1024×1024, 1024×1536, 1536×1024 | 1024×1024, 1024×1536, 1536×1024 | 1024×1024, 1024×1536, 1536×1024 |
| **Quality settings** | low, medium, high (default=high) | low, medium, high (default=high) | low, medium, high (default=medium) |
| **Images/Request** | 1–10 (`n` parameter) | 1–10 | 1–10 |
| **Inpainting** | ✅ mask + prompt | ✅ mask + prompt | ✅ mask + prompt |
| **Face Preservation** | ✅ Advanced | ✅ Advanced | ❌ No dedicated face preservation |

*Source: Official Microsoft Foundry documentation*【34†L34-L44】

**Inpainting** allows editing specific regions of an image: provide an existing image, a mask (which pixels to replace), and a text prompt describing the desired modification. All three GPT-Image models support this workflow【34†L42】.

If a prompt violates content policy, the API returns a `contentFilter` error code instead of generating an image【34†L75-L78】.

### Video Generation with Sora 2

**Sora 2** is OpenAI's video generation model available in **public preview** in Azure AI Foundry【36†L26】:

- **Realistic video generation** powered by advanced world simulation and physics【36†L47】
- **Input modalities:** text, images, and video for reference【36†L48】
- **Synchronized audio and dialogue** for immersive storytelling in multiple languages【36†L49-L50】
- **Enhanced creative control** including detailed prompt understanding for studio shots, scene details, and camera angles【36†L51】
- **Enterprise-grade safety:** content filters for both inputs (text, image, video prompts) and outputs (video frames and audio)【36†L58-L62】

| **Model** | **Sizes** | **Price** |
| --- | --- | --- |
| Sora 2 | Portrait: 720×1280, Landscape: 1280×720 | **$0.10 per second** (USD) |

Available via **Standard Global deployment** in Azure AI Foundry【36†L66】【36†L70-L71】.

---

## 3.2 Design and Implement Multimodal Understanding Workflows

**Exam Skills Mapped**【9†L133-L143】:
- Build solutions that analyze visual context by using multimodal models
- Configure apps to produce concise or detailed captions for single or multiple images
- Implement question-answering grounded in visual evidence
- Configure alt-text and extended image descriptions aligned to accessibility guidelines
- Implement visual understanding by configuring Azure Content Understanding in Foundry Tools
- Implement video analysis workflows to process and interpret video segments
- Configure single-task and pro-mode Content Understanding pipelines
- Implement solutions that identify objects, components, or regions within images or video

### Azure Content Understanding

A Foundry Tool that analyzes and comprehends various media content — documents, images, audio, and video — transforming it into structured, organized, and searchable data【37†L4】. It supports:

- **Standard mode:** Single pre-configured analysis tasks (object detection, OCR, captioning)
- **Pro mode (preview):** Multi-step custom analyzer pipelines chaining multiple analysis steps【37†L28】

Content Understanding supports four modalities: **Document, Image, Audio, and Video**【37†L36】, with SDKs for **Python, .NET, JavaScript, and Java**【37†L46-L48】. Tutorials include building a **RAG solution** and a **robotic process automation solution**【37†L40】.

The learning path covers this through LP4 Modules 4–6: **Analyze images with Content Understanding** (43 min, 6 units)【21†L101-L107】, **Create a multimodal analysis solution** (1 hr, 7 units)【21†L117-L123】, and **Create a Content Understanding client application** (1 hr, 7 units)【21†L133-L139】.

### Visual Question Answering

Multimodal models like `gpt-4.1` (which supports "text and vision to text" input)【39†L54】 can answer questions about images directly. Alternatively, Content Understanding can extract structured information from images, which is then fed to an LLM for reasoning.

---

## 3.3 Implement Responsible AI for Multimodal Content

**Exam Skills Mapped**【9†L144-L149】:
- Implement filters to classify unsafe or disallowed visual content
- Detect and mitigate indirect prompt injection by using embedded text in images
- Enforce visual policy rules (watermarks, prohibited symbols, brand usage, inappropriate content)

### Visual Content Moderation

**Image content moderation** uses the **Analyze image API** (Content Safety), which scans images for sexual content, violence, hate, and self-harm at multi-severity levels【38†L53】. For generated images, models have built-in content filtering — if a prompt violates policy, the API returns a `contentFilter` error code【34†L75-L78】. Sora 2 includes content filters for both inputs (screening text, image, and video prompts) and outputs (analyzing video frames and audio)【36†L58-L60】.

### Indirect Prompt Injection via Images

This attack occurs when malicious text is embedded within images (e.g., "ignore previous instructions" printed on a photo). The LLM may process this embedded text via an OCR step and follow the malicious instructions. **Mitigation:** Use Content Safety or OCR-based preprocessing to detect embedded text, then sanitize it before it reaches the LLM.

### Visual Policy Enforcement

Enforcement actions include: applying **watermarks** to AI-generated content for transparency, **flagging prohibited symbols** or brand logos, and **upholding brand usage requirements**【9†L148】.

---

**← Previous:** Chapter2-GenerativeAgents.md | **README.md** | **Next →** Chapter4-TextAnalysis.md