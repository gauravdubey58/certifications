# ⚡ AI-900 Quick Reference Cheat Sheet

> ⬅️ [Back to AI-900 Index](./index.md)

---

## 🗺️ Azure AI Services — Use Case Map

```
What do you need to do?
│
├── UNDERSTAND IMAGES / VISION?
│   ├── Analyze general images (tags, captions, objects) → Azure AI Vision
│   ├── Extract text from images/docs → Azure AI Vision (OCR / Read API)
│   ├── Classify YOUR custom image types → Custom Vision
│   ├── Detect faces, verify identity → Azure AI Face
│   └── Extract fields from forms/invoices → Azure AI Document Intelligence
│
├── UNDERSTAND / PROCESS LANGUAGE?
│   ├── Sentiment, key phrases, NER, language detection → Azure AI Language
│   ├── Understand user intent (chatbot, virtual agent) → CLU (in Azure AI Language)
│   ├── Build FAQ / Q&A system → Custom Question Answering (Azure AI Language)
│   ├── Translate between languages → Azure AI Translator
│   └── Search intelligently across content → Azure AI Search
│
├── WORK WITH SPEECH / AUDIO?
│   ├── Speech → Text (transcription) → Azure AI Speech (STT)
│   ├── Text → Speech (read aloud) → Azure AI Speech (TTS)
│   ├── Translate spoken language → Azure AI Speech Translation
│   └── Identify a speaker by voice → Azure AI Speech (Speaker Recognition)
│
├── BUILD / TRAIN ML MODELS?
│   ├── No-code visual pipeline → Azure ML Designer
│   ├── Auto-select best algorithm → Azure ML AutoML
│   └── Full custom ML training → Azure Machine Learning
│
├── GENERATIVE AI?
│   ├── Text generation, chat, summarization → Azure OpenAI (GPT-4)
│   ├── Image generation from text → Azure OpenAI (DALL-E 3)
│   ├── Build custom Copilot / chatbot → Microsoft Copilot Studio
│   └── Enterprise AI development portal → Azure AI Foundry
│
└── DETECT ANOMALIES / CONTENT?
    ├── Find outliers in time series data → Azure AI Anomaly Detector
    └── Detect harmful content in text/images → Azure AI Content Safety
```

---

## 🧠 The 6 Responsible AI Principles

| Principle | Key Idea | Example |
|-----------|----------|---------|
| **Fairness** | Treat all people equitably | Loan AI shouldn't discriminate by race |
| **Reliability & Safety** | Perform reliably in all conditions | Self-driving AI must handle edge cases |
| **Privacy & Security** | Protect user data | Medical AI must secure patient records |
| **Inclusiveness** | Empower everyone | Voice AI works for all accents |
| **Transparency** | Be understandable | Explain why a loan was rejected |
| **Accountability** | Humans are responsible | Oversight processes for high-stakes AI |

**Mnemonic: Fair Results Promote Inclusive Transparent AI**

---

## 🤖 ML Types at a Glance

| Type | Needs Labels? | Task | Algorithm Examples |
|------|--------------|------|-------------------|
| **Supervised – Regression** | ✅ Yes (numeric) | Predict a number | Linear Regression, Decision Trees |
| **Supervised – Classification** | ✅ Yes (categories) | Predict a class | Logistic Regression, SVM, Random Forest |
| **Unsupervised – Clustering** | ❌ No | Group similar items | K-Means, DBSCAN |
| **Reinforcement** | ❌ No (uses rewards) | Learn by trial-and-error | Q-Learning, PPO |

---

## 📊 Evaluation Metrics Quick Reference

**For Classification:**
```
Accuracy  = (TP + TN) / Total predictions
Precision = TP / (TP + FP)
Recall    = TP / (TP + FN)
F1 Score  = 2 × (Precision × Recall) / (Precision + Recall)
```

**For Regression:**
- MAE, MSE, RMSE — lower is better
- R² — higher is better (max 1.0)

---

## 🏷️ All Azure AI Services at a Glance

| Service | What It Does |
|---------|-------------|
| Azure AI Vision | Image analysis, OCR, spatial analysis |
| Azure AI Custom Vision | Train custom image classifiers / object detectors |
| Azure AI Face | Face detection, verification, identification, liveness |
| Azure AI Document Intelligence | Extract structured data from documents and forms |
| Azure AI Language | NLP: sentiment, NER, CLU, QnA, summarization |
| Azure AI Translator | Text translation, transliteration (100+ languages) |
| Azure AI Speech | STT, TTS, translation, speaker recognition |
| Azure AI Search | Intelligent search with AI enrichment |
| Azure AI Anomaly Detector | Time-series anomaly detection |
| Azure AI Content Safety | Detect harmful content (text and images) |
| Azure AI Video Indexer | Insights extraction from video and audio |
| Azure OpenAI Service | GPT-4, DALL-E, embeddings, Codex |
| Azure Machine Learning | Full ML platform: training, AutoML, deployment |
| Microsoft Copilot Studio | Build custom copilots and chatbots |
| Azure AI Foundry | Portal for building enterprise AI apps |

---

## 💡 Top Exam Topics

| Topic | Remember This |
|-------|--------------|
| Responsible AI | 6 principles: Fairness, Reliability, Privacy, Inclusiveness, Transparency, Accountability |
| Supervised learning | Needs labeled data; regression (number) vs classification (category) |
| Unsupervised learning | No labels; clustering groups similar items |
| Reinforcement learning | Agent + environment + rewards; no predefined labels |
| AutoML | Automatically tries multiple algorithms; no code needed |
| Custom Vision | Train your own image classifier or object detector |
| CLU | Understand user intents and extract entities from conversational text |
| Azure AI Face | Verification needs approval; identification needs approval |
| LLM Hallucination | Model generates plausible but incorrect info; mitigate with RAG |
| RAG | Ground LLM answers in your own data using search |
| Azure OpenAI | Enterprise-grade OpenAI models: GPT-4, DALL-E, Whisper, Embeddings |
| Prompt engineering | Few-shot, chain-of-thought, system prompts improve output quality |
| Content Safety | Filters harmful content in 4 categories: Hate, Violence, Sexual, Self-harm |
| Copilot Studio | Low-code tool for building custom AI assistants |
| Temperature | 0 = deterministic; higher = more creative/random outputs |

---

> 📝 [Practice with 50 MCQs →](./mcqs.md) | ⬅️ [Back to Index](./index.md)
