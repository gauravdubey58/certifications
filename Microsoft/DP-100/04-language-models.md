# 🟣 Module 4: Optimize Language Models for AI Applications (25–30%)

📘 [← Back to DP-100 Index](./index.md)

> ⚠️ This domain is **25–30%** of the exam — equal to Train & Deploy. Focus significant study time here alongside Module 3.

---

## 4.1 Prepare for Model Optimization

### 🏪 Azure AI Foundry Model Catalog

Azure AI Foundry (formerly Azure AI Studio) is the unified platform for building, evaluating, and deploying AI applications, including language model optimization.

**Model Catalog Sources:**

| Source | Description | Examples |
|--------|-------------|---------|
| **Azure OpenAI** | Microsoft's OpenAI partnership models | GPT-4o, GPT-4, GPT-3.5 Turbo, DALL-E |
| **Hugging Face** | Open-source community models | Mistral, Llama 3, Phi-3 |
| **Meta** | Meta's open models | Llama 3, Code Llama |
| **Microsoft Research** | Microsoft's own models | Phi-3-mini, Phi-3-medium |
| **Mistral AI** | Efficient open models | Mistral-7B, Mixtral-8x7B |
| **Cohere** | Enterprise NLP models | Command R+, Embed |

---

### 📊 Comparing Models Using Benchmarks

Before selecting a model, compare using standardised benchmarks:

| Benchmark | Measures |
|-----------|---------|
| **MMLU** | Massive Multitask Language Understanding — broad knowledge |
| **HumanEval** | Code generation accuracy |
| **HellaSwag** | Commonsense reasoning |
| **TruthfulQA** | Factual accuracy and hallucination avoidance |
| **MT-Bench** | Multi-turn conversation quality |
| **MATH** | Mathematical reasoning |

**Selection Criteria:**

| Factor | Consideration |
|--------|--------------|
| Task fit | Does the benchmark align with your use case? |
| Context window | How much text can the model handle? |
| Cost | Tokens per dollar; provisioned vs pay-per-use |
| Latency | Response time requirements |
| Privacy | Can you use a cloud model, or do you need on-premises? |
| Fine-tuning support | Does the model support fine-tuning if needed? |

---

### 🚀 Deploying a Language Model

```python
from azure.ai.ml import MLClient
from azure.ai.ml.entities import ServerlessEndpoint
from azure.identity import DefaultAzureCredential

# Deploy from model catalog as a serverless endpoint (pay-per-token)
endpoint = ServerlessEndpoint(
    name="phi3-mini-endpoint",
    model_id="azureml://registries/azureml/models/Phi-3-mini-4k-instruct/versions/1"
)
ml_client.serverless_endpoints.begin_create_or_update(endpoint).result()
```

**Testing in the Playground:**
- Navigate to Azure AI Foundry → Deployments → select model → Open in Playground
- Test with different system prompts, temperature, and max tokens
- Compare responses side-by-side using the **compare** feature

**Selecting an Optimization Approach:**

| Scenario | Recommended Approach |
|----------|---------------------|
| General task with good base model | Zero/few-shot prompting |
| Consistent format or style needed | Prompt engineering |
| Retrieval from a large knowledge base | RAG (Retrieval-Augmented Generation) |
| Domain-specific vocabulary / style | Fine-tuning |
| Combine multiple LLM steps | Prompt Flow |

---

## 4.2 Optimize Through Prompt Engineering and Prompt Flow

### ✍️ Prompt Engineering

**Core Techniques:**

| Technique | Description | Example |
|-----------|-------------|---------|
| **Zero-shot** | Task instruction only | `"Classify this review as positive or negative:"` |
| **Few-shot** | 2–5 examples before the actual input | Show example input→output pairs |
| **Chain-of-thought** | Ask to reason step-by-step | Add `"Think step by step:"` |
| **System prompt** | Set persona, constraints, output format | `"You are a helpful assistant. Reply in JSON."` |
| **Grounding** | Provide context documents | Include retrieved passages in the prompt |

**Manual Evaluation in Azure AI Foundry:**
```
1. Go to Evaluations → Manual evaluation
2. Create a dataset of (input, expected_output) pairs
3. Run the model against each input
4. Score each response manually (pass/fail or 1–5 scale)
5. Iterate on system prompt based on failures
```

---

### 🔄 Prompt Flow

Prompt Flow is Azure AI Foundry's orchestration framework for building, evaluating, and deploying LLM-powered applications as **directed acyclic graphs (DAGs)** of nodes.

**Node Types:**

| Node Type | Description |
|-----------|-------------|
| **LLM** | Calls a language model with a Jinja2 prompt template |
| **Python** | Runs a Python function (data transformation, API calls) |
| **Prompt** | Renders a Jinja2 template into a string |
| **Conditional** | Branches flow based on a condition |

**Flow Types:**

| Type | Use Case |
|------|---------|
| **Standard** | Single-turn query → response |
| **Chat** | Multi-turn conversation with history |
| **Evaluation** | Scores a flow's outputs for quality metrics |

**Creating a Flow (SDK):**
```python
from promptflow.azure import PFClient

pf = PFClient(ml_client)

# Run a flow locally for testing
result = pf.test(
    flow="./my_flow",
    inputs={"question": "What is the capital of France?"}
)
print(result)

# Run a flow as a batch job in Azure ML
run = pf.run(
    flow="./my_flow",
    data="./data/test_questions.jsonl",
    stream=True
)
print(pf.get_details(run))
```

**Prompt Template (Jinja2):**
```jinja2
system:
You are a helpful assistant. Answer questions concisely using only the provided context.
If the answer is not in the context, say "I don't know."

Context:
{{context}}

user:
{{question}}
```

---

### 📈 Tracking and Evaluating Prompt Variants

**Define and compare prompt variants:**
```python
# In a flow YAML, define multiple variants of the same LLM node
# variant_0: conservative system prompt
# variant_1: more creative system prompt

# Run variant comparison
run_variant_0 = pf.run(flow="./my_flow", data="./data.jsonl", variant="${llm_node.variant_0}")
run_variant_1 = pf.run(flow="./my_flow", data="./data.jsonl", variant="${llm_node.variant_1}")

# Compare metrics
pf.visualize([run_variant_0, run_variant_1])
```

**Tracing:**
Prompt Flow captures detailed execution traces — every node's inputs, outputs, latency, and token usage.

```python
# Enable tracing
from promptflow.tracing import start_trace
start_trace()

# All subsequent flow executions will be traced
# View in Azure AI Foundry → Tracing tab
```

**Built-in Evaluation Metrics:**

| Metric | Description |
|--------|-------------|
| `groundedness` | Is the answer supported by the context? (RAG) |
| `relevance` | Does the answer address the question? |
| `coherence` | Is the answer logically structured? |
| `fluency` | Is the answer well-written? |
| `similarity` | Cosine similarity to a reference answer |
| `f1_score` | Token-level F1 against reference answer |

---

## 4.3 Optimize Through Retrieval Augmented Generation (RAG)

### 🔄 RAG Architecture

RAG grounds LLM responses in your own data by retrieving relevant context at inference time.

```
User Query
    ↓
Embed query → vector
    ↓
Semantic search against vector store
    ↓
Retrieve top-K relevant chunks
    ↓
Build prompt: [System] + [Context chunks] + [User query]
    ↓
LLM generates grounded answer
    ↓
Return answer + citations to user
```

---

### 📄 Preparing Data for RAG

**Steps: Clean → Chunk → Embed → Index**

**1. Cleaning:**
```python
import re

def clean_document(text: str) -> str:
    text = re.sub(r'\s+', ' ', text)         # Normalise whitespace
    text = re.sub(r'[^\w\s.,!?-]', '', text) # Remove special chars
    text = text.strip()
    return text
```

**2. Chunking Strategies:**

| Strategy | Description | Best For |
|----------|-------------|---------|
| **Fixed-size** | Split every N tokens/characters | Simple; fast |
| **Sentence** | Split on sentence boundaries | Preserves sentence context |
| **Paragraph** | Split on paragraph breaks | Narrative documents |
| **Semantic** | Split based on topic shifts | Long, complex documents |
| **Sliding window** | Overlapping chunks | Prevents context loss at boundaries |

```python
from langchain.text_splitter import RecursiveCharacterTextSplitter

splitter = RecursiveCharacterTextSplitter(
    chunk_size=512,
    chunk_overlap=64,        # Overlap to prevent context loss at boundaries
    separators=["\n\n", "\n", ".", " "]
)
chunks = splitter.split_text(document_text)
```

**3. Generating Embeddings:**
```python
from openai import AzureOpenAI

openai_client = AzureOpenAI(
    azure_endpoint="https://myopenai.openai.azure.com/",
    api_key=api_key,
    api_version="2024-02-01"
)

embeddings = []
for chunk in chunks:
    response = openai_client.embeddings.create(
        input=chunk,
        model="text-embedding-3-small"
    )
    embeddings.append(response.data[0].embedding)
```

---

### 🗄️ Configure a Vector Store

**Vector Store Options:**

| Store | Type | Best For |
|-------|------|---------|
| **Azure AI Search** | Managed cloud service | Production, hybrid search |
| **FAISS** | In-memory / on-disk | Local dev, small datasets |
| **Chroma** | Local persistent | Prototyping |
| **Azure Cosmos DB for MongoDB vCore** | Managed | Existing Cosmos DB workloads |
| **pgvector (Azure Database for PostgreSQL)** | SQL + vector | Relational + vector combined |

---

### 🔍 Azure AI Search Index for RAG

```python
from azure.search.documents.indexes import SearchIndexClient
from azure.search.documents.indexes.models import (
    SearchIndex, SearchField, SearchFieldDataType,
    VectorSearch, HnswAlgorithmConfiguration, VectorSearchProfile,
    SemanticConfiguration, SemanticSearch, SemanticPrioritizedFields,
    SemanticField
)
from azure.core.credentials import AzureKeyCredential

index_client = SearchIndexClient(search_endpoint, AzureKeyCredential(search_key))

fields = [
    SearchField(name="id", type=SearchFieldDataType.String, key=True),
    SearchField(name="content", type=SearchFieldDataType.String, searchable=True),
    SearchField(name="source", type=SearchFieldDataType.String, filterable=True),
    SearchField(
        name="contentVector",
        type=SearchFieldDataType.Collection(SearchFieldDataType.Single),
        searchable=True,
        vector_search_dimensions=1536,
        vector_search_profile_name="myHnswProfile"
    )
]

vector_search = VectorSearch(
    algorithms=[HnswAlgorithmConfiguration(name="myHnsw")],
    profiles=[VectorSearchProfile(name="myHnswProfile", algorithm_configuration_name="myHnsw")]
)

semantic_config = SemanticConfiguration(
    name="my-semantic-config",
    prioritized_fields=SemanticPrioritizedFields(
        content_fields=[SemanticField(field_name="content")]
    )
)

index = SearchIndex(
    name="rag-documents",
    fields=fields,
    vector_search=vector_search,
    semantic_search=SemanticSearch(configurations=[semantic_config])
)
index_client.create_or_update_index(index)
```

**Uploading documents with embeddings:**
```python
from azure.search.documents import SearchClient

search_client = SearchClient(search_endpoint, "rag-documents", AzureKeyCredential(search_key))

documents = []
for i, (chunk, embedding) in enumerate(zip(chunks, embeddings)):
    documents.append({
        "id": str(i),
        "content": chunk,
        "source": "policy_document.pdf",
        "contentVector": embedding
    })

search_client.upload_documents(documents)
```

---

### 📊 Evaluate Your RAG Solution

**Evaluation Metrics for RAG:**

| Metric | Description | Measures |
|--------|-------------|---------|
| **Groundedness** | Is the answer supported by retrieved context? | Hallucination rate |
| **Retrieval Relevance** | Are the retrieved chunks relevant to the query? | Retriever quality |
| **Answer Relevance** | Does the answer address the question? | Generator quality |
| **Context Precision** | What fraction of retrieved chunks are useful? | Retrieval efficiency |
| **Context Recall** | Were all necessary chunks retrieved? | Retrieval completeness |

```python
# Evaluate using Prompt Flow evaluation flow
eval_run = pf.run(
    flow="./rag_eval_flow",      # Built-in or custom evaluation flow
    data="./eval_dataset.jsonl", # Questions + ground truth answers
    column_mapping={
        "question": "${data.question}",
        "answer": "${data.answer}",
        "context": "${data.context}"
    }
)
metrics = pf.get_metrics(eval_run)
print(metrics)  # {"groundedness": 4.2, "relevance": 4.7, "coherence": 4.5}
```

---

## 4.4 Optimize Through Fine-Tuning

### 🎯 When to Fine-Tune vs RAG

| Situation | Fine-Tuning | RAG |
|-----------|------------|-----|
| Domain-specific style/tone | ✅ | ❌ |
| Frequent knowledge updates | ❌ | ✅ |
| Specific output format | ✅ | Partially |
| Large proprietary knowledge base | ❌ | ✅ |
| Reduce prompt length (few-shot examples) | ✅ | ❌ |
| Factual grounding with citations | ❌ | ✅ |

---

### 📋 Prepare Data for Fine-Tuning

Fine-tuning data must be in **JSONL** format (one JSON object per line):

**Chat completion format (most common):**
```jsonl
{"messages": [{"role": "system", "content": "You are a legal document summariser."}, {"role": "user", "content": "Summarise this contract clause: ..."}, {"role": "assistant", "content": "This clause states that..."}]}
{"messages": [{"role": "system", "content": "You are a legal document summariser."}, {"role": "user", "content": "What is the termination policy in this contract?"}, {"role": "assistant", "content": "The termination policy allows..."}]}
```

**Data Requirements:**

| Model | Min Examples | Recommended |
|-------|-------------|-------------|
| GPT-3.5 Turbo | 10 | 50–100+ |
| GPT-4 (preview) | 10 | 50–100+ |
| Phi-3 / open source | 100+ | 500–1000+ |

**Data Quality Checklist:**
- ✅ Diverse examples covering all intended use cases
- ✅ Consistent format and tone
- ✅ Accurate, factually correct completions
- ✅ No duplicates or near-duplicates
- ✅ Balanced representation across scenarios

---

### 🏃 Run a Fine-Tuning Job

**Via Azure AI Foundry (UI):**
1. Go to Fine-tuning → Create fine-tuning job
2. Select base model (e.g., `gpt-35-turbo-0125`)
3. Upload training JSONL and optional validation JSONL
4. Set hyperparameters (epochs, batch size, learning rate multiplier)
5. Start training
6. Monitor loss curves

**Via Python SDK:**
```python
from openai import AzureOpenAI

client = AzureOpenAI(
    azure_endpoint=azure_openai_endpoint,
    api_key=api_key,
    api_version="2024-02-01"
)

# Upload training file
with open("training_data.jsonl", "rb") as f:
    training_file = client.files.create(file=f, purpose="fine-tune")

# Create fine-tuning job
job = client.fine_tuning.jobs.create(
    training_file=training_file.id,
    model="gpt-35-turbo-0125",
    hyperparameters={
        "n_epochs": 3,
        "batch_size": 4,
        "learning_rate_multiplier": 1.0
    }
)

print(f"Fine-tuning job ID: {job.id}")
```

**Key Hyperparameters:**

| Parameter | Description | Guidance |
|-----------|-------------|---------|
| `n_epochs` | Training passes through data | 3–5 typical; more = overfitting risk |
| `batch_size` | Samples per gradient update | Larger = more stable; smaller = more responsive |
| `learning_rate_multiplier` | Scales the default LR | 0.1–2.0; lower for small datasets |

---

### 📊 Evaluate Your Fine-Tuned Model

**Training Metrics to Monitor:**

| Metric | Description | Goal |
|--------|-------------|------|
| `training_loss` | Cross-entropy loss on training data | Decreasing |
| `validation_loss` | Loss on validation set | Decreasing (not diverging from training) |
| `training_token_accuracy` | Token-level accuracy on training data | Increasing |
| `validation_token_accuracy` | Token-level accuracy on validation data | Increasing |

> ⚠️ If `validation_loss` increases while `training_loss` decreases → **overfitting**. Reduce epochs or add more diverse training data.

**Evaluation After Training:**
```python
# Deploy fine-tuned model
deployment = client.deployments.create(
    name="my-finetuned-model",
    model=job.fine_tuned_model  # e.g., "ft:gpt-35-turbo-0125:myorg::abc123"
)

# Run evaluation dataset through the fine-tuned model
eval_results = []
for example in eval_dataset:
    response = client.chat.completions.create(
        model="my-finetuned-model",
        messages=example["messages"][:-1]  # All but the expected answer
    )
    eval_results.append({
        "expected": example["messages"][-1]["content"],
        "actual": response.choices[0].message.content
    })

# Compute metrics (BLEU, ROUGE, exact match, or use Prompt Flow eval)
```

**A/B Testing:**
Compare fine-tuned model vs base model on the same evaluation set before promoting to production.

---

📘 [← Back to DP-100 Index](./index.md)
