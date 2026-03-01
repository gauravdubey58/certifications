# Domain 5 – Generative AI Workloads on Azure `20–25%`

> ⬅️ [Back to AI-900 Index](./index.md)

This is the **newest and fastest-growing domain** — updated in 2023 to include Azure OpenAI and Generative AI topics.

---

## 5.1 What is Generative AI?

Generative AI refers to AI systems that can **create new content** — text, images, code, audio, video — based on a prompt or input.

### Types of Generative AI

| Type | Output | Example Models |
|------|--------|---------------|
| **Text generation** | Human-like text, answers, summaries | GPT-4, GPT-3.5 |
| **Image generation** | Original images from text descriptions | DALL-E 3, Stable Diffusion |
| **Code generation** | Working code from natural language | GitHub Copilot, Codex |
| **Audio generation** | Speech, music | Whisper, TTS |
| **Multimodal** | Multiple output types | GPT-4o (text + images) |

---

## 5.2 Large Language Models (LLMs)

### How LLMs Work (High Level)

1. **Pre-training** — trained on massive text datasets (internet, books, code) to predict the next token
2. **Fine-tuning** — further trained on specific tasks or domains
3. **RLHF** (Reinforcement Learning from Human Feedback) — aligned with human preferences for helpfulness and safety
4. **Inference** — user sends a prompt → model generates a response token by token

### Key Concepts

| Concept | Description |
|---------|-------------|
| **Token** | The basic unit of text (roughly 3/4 of a word on average) |
| **Context window** | Maximum tokens the model can process at once (prompt + response) |
| **Temperature** | Controls randomness: 0 = deterministic, 1+ = more creative |
| **Top-p (nucleus sampling)** | Controls diversity of generated text |
| **Hallucination** | Model generates plausible but incorrect information confidently |
| **Grounding** | Connecting LLM responses to verified, real-world data sources |
| **Embedding** | Numeric vector representation of text for similarity search |

### Transformer Architecture (simplified)
- LLMs are based on the **Transformer** architecture (2017)
- Key mechanism: **Attention** — model learns which parts of input to focus on
- Enables understanding of long-range dependencies in text

---

## 5.3 Azure OpenAI Service

Azure OpenAI Service provides **OpenAI's models** deployed securely within Azure's infrastructure with enterprise features.

### Available Models

| Model Family | Capabilities |
|-------------|-------------|
| **GPT-4o, GPT-4** | Advanced text and multimodal reasoning |
| **GPT-3.5-Turbo** | Fast, cost-effective chat completion |
| **DALL-E 3** | Text-to-image generation |
| **Whisper** | Speech-to-text transcription |
| **Embeddings (text-embedding-ada-002)** | Text embeddings for search and similarity |
| **Codex** | Code generation (now part of GPT-4) |

### Azure OpenAI vs OpenAI API

| | Azure OpenAI | OpenAI API |
|--|-------------|------------|
| Where data goes | Stays within Azure | Goes to OpenAI servers |
| Enterprise features | Private networking, RBAC, compliance | Limited |
| Content filtering | Built-in Azure AI Content Safety | Moderation API |
| SLA | Azure SLAs | OpenAI terms |
| Cost | Azure pricing | OpenAI pricing |

> ⭐ **Exam Tip:** Azure OpenAI is the **enterprise version** of OpenAI's models. It adds Azure-native security, compliance, and private networking.

### Azure AI Foundry (formerly Azure AI Studio)
- The **portal and SDK platform** for building, testing, and deploying AI apps
- Access Azure OpenAI models, fine-tune them, and deploy to endpoints
- Build AI solutions with a unified interface
- URL: https://ai.azure.com

---

## 5.4 Prompt Engineering

Prompt engineering is the practice of **crafting effective inputs** to get better outputs from AI models.

### Core Techniques

| Technique | Description | Example |
|-----------|-------------|---------|
| **Zero-shot** | Ask directly with no examples | "Translate this to Spanish: Hello" |
| **Few-shot** | Provide a few examples before the actual request | Show 3 examples then ask |
| **Chain-of-thought** | Ask the model to "think step by step" | "Let's think through this..." |
| **System prompt** | Set context and role for the model | "You are a helpful medical assistant" |
| **Temperature setting** | Adjust randomness of output | temperature=0 for factual; 0.7 for creative |

### Good Prompt Principles
- Be **specific** and clear about what you want
- Provide **context** and background
- Specify **format** of desired output (bullet list, JSON, table)
- Set **constraints** (length, language, tone)
- Use **examples** when needed

---

## 5.5 Retrieval-Augmented Generation (RAG)

RAG is a technique that **grounds LLM responses in your own data** to reduce hallucinations.

### How RAG Works
```
User Query
    ↓
Search your knowledge base (Azure AI Search)
    ↓
Retrieve relevant documents/chunks
    ↓
Send retrieved context + original query to LLM
    ↓
LLM generates answer grounded in your data
```

### Why RAG?
- Reduces hallucinations
- Keeps responses up-to-date (no retraining)
- Cites sources (more transparent)
- Works with private/proprietary data

---

## 5.6 Microsoft Copilot and Copilot Stack

### Microsoft Copilot
- AI assistant embedded across Microsoft 365 (Word, Excel, Teams, Outlook)
- Powered by GPT-4 + Microsoft Graph (your organizational data)
- Examples: summarize an email thread, generate a Word document, analyze Excel data

### Copilot Stack Architecture

```
Your App / Copilot
    ↓
Orchestrator (Semantic Kernel / Copilot Studio)
    ↓
Azure OpenAI Models (LLMs)
    ↓
Plugins / Tools / Data (RAG, APIs, databases)
```

### Microsoft Copilot Studio
- Low-code/no-code tool to **build custom Copilots** (chatbots + agents)
- Connect to data sources (SharePoint, Dataverse, Blob)
- Previously called Power Virtual Agents
- Deploy to Teams, websites, mobile apps

---

## 5.7 Responsible Generative AI

### Key Risks of Generative AI

| Risk | Description | Mitigation |
|------|-------------|-----------|
| **Hallucination** | Model generates false information confidently | Use RAG; add disclaimers |
| **Harmful content** | Model produces violent, hateful, or illegal content | Content filters; system prompts |
| **Jailbreaking** | Users bypass safety systems with clever prompts | Robust safety training; monitoring |
| **Copyright** | Model generates content similar to copyrighted works | Legal review; attribution |
| **Bias** | Model reflects biases in training data | Fairness evaluation; diverse data |
| **Privacy** | Model may recall training data with private info | Data governance; PII detection |

### Azure AI Content Safety
- Detects and filters **harmful content** in text and images
- Categories: Hate, Sexual, Violence, Self-harm
- Severity levels: 0 (safe) to 7 (severe)
- Used in Azure OpenAI Service by default

### Microsoft's Responsible AI Approach for Generative AI (4 Layers)
1. **Model** — responsible model development (RLHF, safety training)
2. **Safety system** — content filters, meta-prompts (Azure AI Content Safety)
3. **Application** — responsible UX design, transparency
4. **Operations** — monitoring, feedback loops, incident response

---

> ⬅️ [← Domain 4](./domain-4-nlp.md) | [Back to Index](./index.md) | [⚡ Quick Reference →](./quick-reference.md)
