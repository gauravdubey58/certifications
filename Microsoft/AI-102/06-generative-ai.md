# 🟢 Module 6: Implement Generative AI Solutions (10–15%)

📘 [← Back to AI-102 Index](./index.md)

---

## 6.1 Azure OpenAI Service

### 🧠 Overview

Azure OpenAI Service provides access to OpenAI's powerful models (GPT-4, GPT-3.5 Turbo, DALL-E, Whisper, Embeddings) through Azure's secure, enterprise-grade platform — with compliance, private networking, and RBAC.

**Key Differentiators from OpenAI.com:**

| Feature | Azure OpenAI | OpenAI.com |
|---------|-------------|-----------|
| Network | Private endpoints, VNet | Public only |
| Auth | Azure RBAC + Managed Identity | API Key only |
| Compliance | Azure compliance (ISO, SOC, HIPAA) | Limited |
| Data residency | Region-specific | US-centric |
| Content filtering | Configurable, auditable | Standard |
| SLA | Yes | No |

---

### 📦 Models Available

| Model | Best For |
|-------|---------|
| **GPT-4o** | Best quality multimodal — text and vision |
| **GPT-4** | Complex reasoning, longer context |
| **GPT-3.5 Turbo** | Cost-effective chat completions |
| **text-embedding-ada-002** | Document and query embeddings (legacy) |
| **text-embedding-3-small/large** | Improved embeddings with variable dimensions |
| **DALL-E 3** | Text-to-image generation |
| **Whisper** | Speech-to-text transcription |
| **GPT-4 Vision** | Image + text input (multimodal) |

---

### 🚀 Deployments

Before calling a model, you must create a **Deployment** within your Azure OpenAI resource.

**Deployment Types:**

| Type | Description | Use Case |
|------|-------------|---------|
| **Standard** | Shared capacity; pay-per-token | Development, variable load |
| **Provisioned** | Reserved throughput (PTUs) | Predictable, high-volume workloads |
| **Batch** | Large-scale offline processing | Bulk summarisation, embeddings |
| **Global-Standard** | Routes across Azure regions for higher limits | Global apps needing scale |

```bash
# Create a deployment via CLI
az cognitiveservices account deployment create \
  --name myOpenAIResource \
  --resource-group myRG \
  --deployment-name gpt4-deployment \
  --model-name gpt-4 \
  --model-version "0613" \
  --model-format OpenAI \
  --sku-capacity 10 \
  --sku-name "Standard"
```

---

### 💬 Chat Completions API

```python
from openai import AzureOpenAI

client = AzureOpenAI(
    azure_endpoint="https://myopenai.openai.azure.com/",
    api_key="<your-key>",
    api_version="2024-02-01"
)

response = client.chat.completions.create(
    model="gpt4-deployment",   # Your deployment name, not model name
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Summarise the key features of Azure AI Search."}
    ],
    max_tokens=500,
    temperature=0.7,         # 0 = deterministic, 2 = very creative
    top_p=0.95,
    frequency_penalty=0,     # Penalise repeated tokens
    presence_penalty=0       # Encourage talking about new topics
)

print(response.choices[0].message.content)
```

**Message Roles:**

| Role | Purpose |
|------|---------|
| `system` | Sets the assistant's persona, instructions, and constraints |
| `user` | Human's message or question |
| `assistant` | AI's previous responses (for multi-turn conversations) |
| `tool` | Results from tool/function calls |

---

### 🔧 Prompt Engineering

**Core Techniques:**

| Technique | Description | When to Use |
|-----------|-------------|-------------|
| **Zero-shot** | No examples; just the instruction | Simple, well-defined tasks |
| **Few-shot** | Include examples in the prompt | Formatting or classification tasks |
| **Chain-of-thought** | Ask the model to reason step by step | Maths, logic, complex reasoning |
| **System prompt** | Set persona, tone, constraints upfront | All production applications |
| **Grounding** | Provide context/data in the prompt | RAG, document Q&A |

**System Prompt Best Practices:**
```
You are a helpful customer support assistant for Contoso Electronics.
- Only answer questions related to Contoso products.
- If you don't know an answer, say "I'm not sure — let me connect you with a specialist."
- Be concise and professional.
- Never make up product specifications.
```

**Temperature and Top-P:**

| Setting | Low Value | High Value |
|---------|-----------|-----------|
| `temperature` | Deterministic, repetitive | Creative, unpredictable |
| `top_p` | Only highly probable tokens | More vocabulary diversity |

> 💡 Typically adjust **either** temperature **or** top_p — not both.

---

### 📐 Embeddings

Embeddings are vector representations of text used to measure semantic similarity.

```python
response = client.embeddings.create(
    input="What is Azure AI Search?",
    model="text-embedding-3-small"   # Your embedding deployment name
)

vector = response.data[0].embedding  # List of floats (e.g., 1536 dimensions)
```

**Use cases:** Semantic search, document clustering, recommendation engines, anomaly detection in text.

---

## 6.2 Retrieval-Augmented Generation (RAG)

### 🔄 RAG Pattern

RAG combines a **retrieval system** (typically AI Search) with a **generation model** (GPT) to answer questions grounded in your own data.

**Architecture:**
```
User Query
    ↓
1. Embed query → vector
    ↓
2. Search AI Search index (vector + keyword hybrid)
    ↓
3. Retrieve top-K relevant document chunks
    ↓
4. Build prompt: [System] + [Retrieved chunks] + [User query]
    ↓
5. Send to Azure OpenAI GPT model
    ↓
6. Return grounded answer with citations
```

**Why RAG?**
- GPT models have a training cutoff — they don't know your proprietary data
- Avoids hallucination by providing source documents as context
- More cost-effective than fine-tuning for knowledge injection

**Azure OpenAI "On Your Data" (built-in RAG):**
```python
response = client.chat.completions.create(
    model="gpt4-deployment",
    messages=[{"role": "user", "content": "What is the refund policy?"}],
    extra_body={
        "data_sources": [{
            "type": "azure_search",
            "parameters": {
                "endpoint": "https://mysearch.search.windows.net",
                "index_name": "policies-index",
                "authentication": {
                    "type": "api_key",
                    "key": search_key
                }
            }
        }]
    }
)
```

---

## 6.3 Azure AI Content Safety

### 🛡️ Content Safety in Generative AI

Azure AI Content Safety detects and filters harmful content in both **inputs** (user prompts) and **outputs** (model responses).

**Content Filtering (Azure OpenAI):**

Every Azure OpenAI deployment has a content filter applied by default. Filters can be customised:

| Category | Severity Levels |
|----------|----------------|
| **Hate** | Safe, Low, Medium, High |
| **Violence** | Safe, Low, Medium, High |
| **Sexual** | Safe, Low, Medium, High |
| **Self-harm** | Safe, Low, Medium, High |

**Jailbreak Detection:** Detects attempts to bypass the model's safety instructions.

**Groundedness Detection:** Checks whether the model's response is grounded in the provided context (RAG applications).

```python
from azure.ai.contentsafety import ContentSafetyClient
from azure.ai.contentsafety.models import AnalyzeTextOptions

safety_client = ContentSafetyClient(safety_endpoint, AzureKeyCredential(safety_key))

# Check user input before sending to GPT
request = AnalyzeTextOptions(
    text=user_message,
    categories=["Hate", "Violence", "Sexual", "SelfHarm"]
)
response = safety_client.analyze_text(request)

for category in response.categories_analysis:
    if category.severity >= 4:  # Block if medium or above
        raise ValueError(f"Content blocked: {category.category}")
```

---

## 6.4 DALL-E Image Generation

```python
response = client.images.generate(
    model="dall-e-3",       # Your DALL-E 3 deployment name
    prompt="A futuristic city skyline at sunset, digital art style",
    n=1,                    # DALL-E 3 only supports n=1
    size="1024x1024",       # 1024x1024, 1024x1792, or 1792x1024
    quality="hd",           # standard or hd
    style="vivid"           # vivid (dramatic) or natural (realistic)
)

image_url = response.data[0].url
revised_prompt = response.data[0].revised_prompt  # DALL-E may modify prompt for safety
```

---

## 6.5 Azure AI Content Understanding

**What it is:** A newer Azure AI service for extracting structured information from multimodal documents — combining vision, speech, and language understanding in a single pipeline.

**Key Scenarios:**
- Extract data from complex documents containing text + tables + images
- Process audio/video for structured insight extraction
- Build document processing pipelines with a single API

**Difference from Document Intelligence:**

| | Document Intelligence | Content Understanding |
|--|---------------------|----------------------|
| Best for | Structured forms, fixed templates | Complex multimodal documents |
| Input | Documents, forms | Documents, audio, video |
| Models | Prebuilt + Custom | Prebuilt analysers |
| Use case | Invoice/receipt extraction | Rich media analysis |

---

## 📝 Quick Revision — Generative AI Key Facts

| Fact | Detail |
|------|--------|
| Azure OpenAI auth | API Key OR Managed Identity (preferred) |
| Deployment vs Model | You call the **deployment name**, not the model name |
| Temperature = 0 | Deterministic output |
| Temperature = 1–2 | More creative/random |
| RAG benefit | Grounds GPT responses in your own data |
| Content filter default | Applied to all Azure OpenAI deployments |
| DALL-E 3 max `n` | 1 (only 1 image per request) |
| `text-embedding-3-small` | 1536 dimensions (default), can reduce to 256 |
| Fine-tuning vs RAG | RAG = retrieve at inference time; Fine-tuning = bake knowledge into weights |

---

📘 [← Back to AI-102 Index](./index.md)
