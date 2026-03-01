# Domain 4 – Natural Language Processing on Azure `15–20%`

> ⬅️ [Back to AI-900 Index](./index.md)

---

## 4.1 NLP Concepts

Natural Language Processing (NLP) enables computers to **understand, interpret, and generate human language**.

### Core NLP Tasks

| Task | Description | Example |
|------|-------------|---------|
| **Tokenization** | Break text into words/subwords | "Hello world" → ["Hello", "world"] |
| **Stemming / Lemmatization** | Reduce words to their root form | "running" → "run" |
| **Named Entity Recognition (NER)** | Identify entities (people, places, dates, orgs) | "Microsoft founded in 1975" → [ORG, DATE] |
| **Sentiment Analysis** | Detect positive, negative, neutral, or mixed sentiment | Review is "positive" (confidence: 0.95) |
| **Key Phrase Extraction** | Extract the most important phrases | "Azure cloud platform" |
| **Language Detection** | Identify the language of a text | "Bonjour" → French (confidence: 0.99) |
| **Text Classification** | Categorize text into predefined classes | News article → Sports / Politics |
| **Summarization** | Generate a shorter version of text | Extractive or abstractive |
| **Translation** | Convert text from one language to another | English → Spanish |

---

## 4.2 Azure AI Language

Azure AI Language (formerly Text Analytics + LUIS + QnA Maker) is the unified NLP service.

### Features

| Feature | What It Does |
|---------|-------------|
| **Sentiment Analysis** | Returns positive/negative/neutral/mixed with confidence scores |
| **Opinion Mining** | Identifies sentiment toward specific aspects ("The camera is excellent") |
| **Key Phrase Extraction** | Extracts main topics from text |
| **Named Entity Recognition (NER)** | Detects and categorizes entities (Person, Location, Date, Org, etc.) |
| **Entity Linking** | Links entities to Wikipedia articles for disambiguation |
| **Language Detection** | Identifies the language of input text |
| **Text Summarization** | Extractive (picks sentences) or abstractive (generates summary) |
| **Custom Text Classification** | Train your own text categories |
| **Custom NER** | Train to extract your own entity types |
| **Conversational Language Understanding (CLU)** | Understand intent and extract entities from conversational queries |
| **Custom Question Answering** | Build FAQ-style bots from documents or knowledge bases |

### Conversational Language Understanding (CLU)
Formerly known as **LUIS (Language Understanding Intelligent Service)**.

| Concept | Description | Example |
|---------|-------------|---------|
| **Utterance** | A sample phrase the user might say | "Book a flight to Paris" |
| **Intent** | The user's goal / action | `BookFlight` |
| **Entity** | Specific information extracted from the utterance | Destination = "Paris" |

**CLU Workflow:**
1. Define **intents** (what users want to do)
2. Add **example utterances** per intent
3. Label **entities** in utterances
4. **Train** the model
5. **Publish** and test
6. Integrate via REST API

> ⭐ **Exam Tip:** CLU (Custom Question Answering) = you provide Q&A pairs or documents. CLU understands user intent from conversational language. Both are part of Azure AI Language.

---

## 4.3 Azure AI Speech

The Azure AI Speech service handles **spoken language** tasks.

### Speech Capabilities

| Capability | Description | Use Case |
|-----------|-------------|---------|
| **Speech-to-Text (STT)** | Convert audio to text in real time or batch | Transcription, voice commands |
| **Text-to-Speech (TTS)** | Convert text to natural-sounding audio | Virtual assistants, accessibility |
| **Speech Translation** | Real-time speech translation across languages | International meetings |
| **Speaker Recognition** | Identify or verify a speaker by their voice | Authentication, call center analytics |
| **Custom Speech** | Fine-tune STT for your domain vocabulary | Medical, legal, technical terms |
| **Custom Voice** | Create a custom synthetic voice | Brand voice for virtual assistants |

### Speech-to-Text Use Cases
- Customer service call transcription
- Meeting note generation
- Accessibility for hearing-impaired users
- Voice-controlled applications

---

## 4.4 Azure AI Translator

- **Neural machine translation** for 100+ languages
- **Transliteration** — convert text between scripts (e.g., Arabic to Latin)
- **Language detection** — automatically detect source language
- **Custom Translator** — train custom translation models for domain-specific terminology

---

## 4.5 Azure AI Search (Knowledge Mining)

Azure AI Search enables **intelligent search** over large amounts of unstructured and structured content.

### Key Concepts

| Concept | Description |
|---------|-------------|
| **Index** | Searchable store of documents |
| **Indexer** | Automates data ingestion from sources (Blob, SQL, Cosmos DB) |
| **Skillset** | AI enrichment pipeline (OCR, NER, key phrases applied during indexing) |
| **Knowledge Store** | Persists AI-enriched content for downstream analysis |
| **Semantic Search** | Ranks results by semantic relevance (not just keywords) |
| **Vector Search** | Enables similarity search using embeddings |

### AI Enrichment with Azure AI Search
During indexing, you can apply cognitive skills:
- Extract text from images (OCR)
- Detect language
- Extract entities and key phrases
- Translate content
- Apply custom ML models

---

> ⬅️ [← Domain 3](./domain-3-computer-vision.md) | [Back to Index](./index.md) | [Domain 5 →](./domain-5-generative-ai.md)
