# Domain 1 – AI Workloads and Considerations `15–20%`

> ⬅️ [Back to AI-900 Index](./index.md)

---

## 1.1 Common AI Workloads

AI systems are built to automate tasks that typically require human intelligence.

### Types of AI Workloads

| Workload | Description | Examples |
|----------|-------------|---------|
| **Prediction / Regression** | Predict a numeric value based on input features | House price prediction, demand forecasting, stock prices |
| **Classification** | Assign inputs to predefined categories | Spam detection, disease diagnosis, image tagging |
| **Clustering** | Group similar items without predefined labels | Customer segmentation, anomaly detection |
| **Computer Vision** | Understand and interpret visual information | Image classification, object detection, facial recognition, OCR |
| **Natural Language Processing (NLP)** | Understand and generate human language | Sentiment analysis, translation, chatbots, summarization |
| **Anomaly Detection** | Identify unusual patterns or outliers | Fraud detection, equipment failure prediction |
| **Knowledge Mining** | Extract structured information from large unstructured datasets | Azure AI Search, document intelligence |
| **Generative AI** | Create new content (text, images, code, audio) | ChatGPT, DALL-E, GitHub Copilot |
| **Conversational AI** | Interact with humans via natural language dialogue | Chatbots, virtual assistants |

---

## 1.2 Responsible AI

Microsoft defines **6 core principles** for responsible AI development. These are heavily tested on the exam.

### The 6 Principles

#### 1. 🤝 Fairness
- AI systems should treat all people **equitably**
- Should not discriminate based on gender, race, ethnicity, age, disability, or other characteristics
- Example: A loan approval model should not deny loans based on race
- Tool: **Fairlearn** (open-source toolkit for assessing and improving fairness)

#### 2. 🛡️ Reliability and Safety
- AI systems should perform **reliably and safely** as designed
- Should handle unexpected conditions gracefully without causing harm
- Example: An autonomous vehicle AI must be safe in edge cases like bad weather
- Systems must be rigorously tested before deployment

#### 3. 🔒 Privacy and Security
- AI systems should respect **user privacy** and protect sensitive data
- Data used to train and run models must be secured
- Example: Medical AI should not expose patient data
- Compliance with GDPR and other privacy regulations

#### 4. 🌍 Inclusiveness
- AI should **empower everyone** regardless of ability, culture, or background
- Systems should accommodate people with disabilities
- Example: Voice recognition should work for different accents
- Should not create barriers for underrepresented groups

#### 5. 🔍 Transparency
- AI systems should be **understandable** to the people who build and use them
- Users should know when they are interacting with AI
- Explanations should be available for AI decisions
- Example: Being told why a loan application was rejected

#### 6. ✅ Accountability
- People and organizations must be **accountable** for AI systems
- Clear governance and oversight structures
- Mechanisms to address harmful outcomes
- Example: Having a human review process for high-stakes AI decisions

> ⭐ **Exam Tip:** Memorize all 6 Responsible AI principles: **F**airness, **R**eliability, **P**rivacy, **I**nclusiveness, **T**ransparency, **A**ccountability. A helpful mnemonic: **"Fair Results Promote Inclusive Transparent AI"**

---

## 1.3 AI vs Machine Learning vs Deep Learning

```
Artificial Intelligence (AI)
└── Machine Learning (ML)
    └── Deep Learning (DL)
        └── Foundation Models / LLMs
```

| Term | Definition |
|------|-----------|
| **Artificial Intelligence (AI)** | Broad field — computers simulating human intelligence |
| **Machine Learning (ML)** | AI systems that learn from data without being explicitly programmed |
| **Deep Learning (DL)** | ML using neural networks with many layers; excels at images, audio, text |
| **Foundation Model** | Large pre-trained model (LLM, CLIP) that can be adapted to many tasks |

---

## 1.4 Azure AI Services Overview

Microsoft groups AI capabilities into the **Azure AI services** umbrella (formerly Cognitive Services):

| Category | Services |
|----------|---------|
| **Vision** | Azure AI Vision, Custom Vision, Face, Document Intelligence |
| **Speech** | Speech-to-Text, Text-to-Speech, Translation, Speaker Recognition |
| **Language** | Text Analytics, CLU (Conversational Language Understanding), QnA / Custom Q&A, Translator |
| **Decision** | Anomaly Detector, Content Safety, Personalizer |
| **Generative AI** | Azure OpenAI Service (GPT-4, DALL-E, Codex) |
| **Search** | Azure AI Search (with semantic + vector search) |

---

> ⬅️ [Back to Index](./index.md) | ➡️ [Domain 2 →](./domain-2-machine-learning.md)
