# 📝 AI-900 Practice MCQs — 50 Questions with Answers

> ⬅️ [Back to AI-900 Index](./index.md)

**Instructions:** Try each question before checking the answer. Aim for ≥ 80% to be exam-ready.

---

## Domain 1 – AI Workloads and Considerations (Questions 1–12)

**Q1.** Which Responsible AI principle ensures that an AI system does not discriminate based on gender or ethnicity?
- A) Reliability
- B) Fairness
- C) Transparency
- D) Inclusiveness

> ✅ **Answer: B — Fairness**
> Fairness means AI systems must treat all people equitably, without discrimination based on protected characteristics.

---

**Q2.** A bank's loan approval AI provides explanations to customers about why their application was rejected. This is an example of which Responsible AI principle?
- A) Fairness
- B) Privacy
- C) Transparency
- D) Accountability

> ✅ **Answer: C — Transparency**
> Transparency means AI systems should be understandable and users should be able to understand how decisions are made.

---

**Q3.** Which AI workload type would be used to identify fraudulent transactions by finding unusual patterns?
- A) Classification
- B) Regression
- C) Anomaly Detection
- D) Clustering

> ✅ **Answer: C — Anomaly Detection**
> Anomaly detection identifies unusual patterns that deviate from expected behavior, ideal for fraud detection.

---

**Q4.** A chatbot that can answer customer questions in natural language is an example of which type of AI workload?
- A) Computer Vision
- B) Anomaly Detection
- C) Conversational AI
- D) Regression

> ✅ **Answer: C — Conversational AI**
> Conversational AI enables natural language dialogue between humans and AI systems, like chatbots and virtual assistants.

---

**Q5.** Which Responsible AI principle requires that organizations have clear processes to address harmful outcomes from AI systems?
- A) Inclusiveness
- B) Transparency
- C) Fairness
- D) Accountability

> ✅ **Answer: D — Accountability**
> Accountability means people and organizations must be responsible for their AI systems and have oversight mechanisms in place.

---

**Q6.** An AI system reads receipts and automatically categorizes expenses in an accounting app. Which AI workload is this?
- A) Anomaly Detection
- B) Computer Vision with OCR
- C) Clustering
- D) Reinforcement Learning

> ✅ **Answer: B — Computer Vision with OCR**
> Reading text from images (receipts) uses OCR, which is a computer vision capability.

---

**Q7.** Which Responsible AI principle ensures that AI systems work well for people with disabilities?
- A) Reliability
- B) Privacy
- C) Inclusiveness
- D) Transparency

> ✅ **Answer: C — Inclusiveness**
> Inclusiveness means AI should empower everyone, including people with disabilities, different cultures, and backgrounds.

---

**Q8.** What is the difference between AI and Machine Learning?
- A) They are the same thing
- B) AI is a subset of Machine Learning
- C) Machine Learning is a subset of AI
- D) Machine Learning requires labeled data; AI does not

> ✅ **Answer: C — Machine Learning is a subset of AI**
> AI is the broad field. ML is one approach within AI where systems learn from data.

---

**Q9.** A company wants to ensure its AI system performs reliably even in unexpected scenarios like power outages. Which principle does this address?
- A) Fairness
- B) Reliability and Safety
- C) Privacy
- D) Transparency

> ✅ **Answer: B — Reliability and Safety**
> Reliability and Safety ensures AI systems perform as expected in all conditions including edge cases.

---

**Q10.** Which workload uses AI to generate new text, images, or code from a prompt?
- A) Classification
- B) Knowledge Mining
- C) Anomaly Detection
- D) Generative AI

> ✅ **Answer: D — Generative AI**
> Generative AI creates new content (text, images, code, audio) from a prompt or description.

---

**Q11.** An organization uses AI to group customers into segments based on purchasing behavior without predefined categories. This is which type of AI?
- A) Supervised Classification
- B) Supervised Regression
- C) Unsupervised Clustering
- D) Reinforcement Learning

> ✅ **Answer: C — Unsupervised Clustering**
> Clustering groups similar data points without predefined labels — classic unsupervised learning.

---

**Q12.** Which Azure tool helps assess and improve an AI model's fairness across demographic groups?
- A) Azure Monitor
- B) Fairlearn
- C) Azure Policy
- D) Azure Blueprints

> ✅ **Answer: B — Fairlearn**
> Fairlearn is an open-source toolkit integrated with Azure ML for assessing and mitigating AI model bias.

---

## Domain 2 – Machine Learning Principles (Questions 13–25)

**Q13.** A data scientist trains a model to predict the price of a house based on size, location, and age. What type of ML is this?
- A) Classification
- B) Clustering
- C) Regression
- D) Reinforcement Learning

> ✅ **Answer: C — Regression**
> Regression predicts a continuous numeric value (house price). It's supervised learning with a numeric label.

---

**Q14.** Which ML type does NOT require labeled training data?
- A) Supervised learning
- B) Regression
- C) Classification
- D) Unsupervised learning

> ✅ **Answer: D — Unsupervised learning**
> Unsupervised learning finds patterns in unlabeled data. No predefined answers are needed.

---

**Q15.** A model achieves 99% accuracy on training data but only 60% on new test data. What problem does this indicate?
- A) Underfitting
- B) Overfitting
- C) Data leakage
- D) Low bias

> ✅ **Answer: B — Overfitting**
> Overfitting occurs when a model memorizes training data too well and fails to generalize to new data.

---

**Q16.** Which Azure ML feature automatically selects the best machine learning algorithm for your data?
- A) Azure ML Designer
- B) Azure ML Notebooks
- C) Automated ML (AutoML)
- D) Azure ML Compute Cluster

> ✅ **Answer: C — Automated ML (AutoML)**
> AutoML automatically tries multiple algorithms and hyperparameters to find the best-performing model.

---

**Q17.** Which compute type in Azure ML is used for interactive development and running Jupyter notebooks?
- A) Compute Cluster
- B) Compute Instance
- C) Inference Cluster
- D) Serverless Compute

> ✅ **Answer: B — Compute Instance**
> A Compute Instance is a managed VM used for interactive development (running notebooks, IDE work).

---

**Q18.** What is a "feature" in the context of machine learning?
- A) The output the model predicts
- B) An input variable used to make predictions
- C) The accuracy of the model
- D) The training dataset

> ✅ **Answer: B — An input variable used to make predictions**
> Features are the input variables (columns) that the model uses to learn and make predictions.

---

**Q19.** Which metric measures, of all the positive predictions made, how many were actually correct?
- A) Recall
- B) Accuracy
- C) Precision
- D) F1 Score

> ✅ **Answer: C — Precision**
> Precision = TP / (TP + FP). It answers: "Of everything I predicted as positive, how many were right?"

---

**Q20.** Which metric measures how many actual positives the model correctly identified?
- A) Precision
- B) Accuracy
- C) Recall
- D) AUC

> ✅ **Answer: C — Recall**
> Recall = TP / (TP + FN). It answers: "Of all the actual positives, how many did I find?"

---

**Q21.** A game-playing AI learns by trial and error, receiving points when it wins and losing points when it loses. What type of ML is this?
- A) Supervised learning
- B) Unsupervised learning
- C) Reinforcement learning
- D) Transfer learning

> ✅ **Answer: C — Reinforcement learning**
> Reinforcement learning uses an agent that interacts with an environment, learning from rewards and penalties.

---

**Q22.** Which Azure ML tool provides a drag-and-drop visual interface to build ML pipelines without code?
- A) AutoML
- B) Azure ML Designer
- C) Compute Cluster
- D) MLflow

> ✅ **Answer: B — Azure ML Designer**
> Azure ML Designer is the no-code/low-code visual pipeline builder for Azure Machine Learning.

---

**Q23.** In Azure Machine Learning, what is a "workspace"?
- A) The compute resource for training models
- B) The top-level resource that contains all ML assets and experiments
- C) The dataset used for training
- D) The deployed model endpoint

> ✅ **Answer: B — The top-level resource that contains all ML assets**
> An Azure ML Workspace is the root resource that contains all experiments, models, datasets, and compute resources.

---

**Q24.** What is R² (R-squared) used to measure in regression models?
- A) The number of incorrect predictions
- B) The percentage of training time saved
- C) The proportion of variance in the target explained by the model
- D) The cost of running the model

> ✅ **Answer: C — The proportion of variance in the target explained by the model**
> R² of 1.0 = perfect fit; R² of 0 = model explains nothing. Higher is better for regression.

---

**Q25.** Which type of endpoint would you use for a deployed ML model that needs to return instant predictions for a web application?
- A) Batch endpoint
- B) Real-time / Online endpoint
- C) Scheduled endpoint
- D) Pipeline endpoint

> ✅ **Answer: B — Real-time / Online endpoint**
> Real-time endpoints are always-on REST APIs that return instant predictions for individual requests.

---

## Domain 3 – Computer Vision (Questions 26–34)

**Q26.** Which Azure AI service would you use to detect objects and their locations within an image using bounding boxes?
- A) Azure AI Language
- B) Azure AI Vision (Image Analysis)
- C) Azure AI Face
- D) Azure AI Document Intelligence

> ✅ **Answer: B — Azure AI Vision (Image Analysis)**
> Azure AI Vision's Image Analysis feature can detect objects and return their bounding box coordinates.

---

**Q27.** You need to extract key-value pairs (like "Total: $42.50") from scanned invoices. Which Azure service should you use?
- A) Azure AI Vision OCR
- B) Azure AI Language
- C) Azure AI Document Intelligence
- D) Azure AI Custom Vision

> ✅ **Answer: C — Azure AI Document Intelligence**
> Document Intelligence (formerly Form Recognizer) is specifically designed to extract structured fields from forms and documents.

---

**Q28.** A retail company wants to classify their product images into their own custom categories (not standard ones). Which service should they use?
- A) Azure AI Vision
- B) Azure AI Face
- C) Azure AI Custom Vision
- D) Azure AI Document Intelligence

> ✅ **Answer: C — Azure AI Custom Vision**
> Custom Vision allows you to train your own image classification or object detection models with your own custom categories.

---

**Q29.** Which Azure AI Face capability requires special Microsoft approval before use?
- A) Face detection
- B) Face analysis (headpose, blur)
- C) Liveness detection
- D) Face identification

> ✅ **Answer: D — Face identification**
> Identifying a specific person requires approval due to responsible AI concerns. Detection and basic analysis are available without approval.

---

**Q30.** What is the difference between Image Classification and Object Detection in Custom Vision?
- A) Image Classification is faster; Object Detection is more accurate
- B) Image Classification labels the whole image; Object Detection finds objects and their locations
- C) Object Detection only works with Azure AI Vision; Classification works with Custom Vision
- D) They are the same thing with different names

> ✅ **Answer: B**
> Image Classification assigns category labels to the whole image. Object Detection finds individual objects with bounding boxes.

---

**Q31.** Which Azure service would you use to count the number of people in a retail store in real time using a security camera feed?
- A) Azure AI Custom Vision
- B) Azure AI Face
- C) Azure AI Vision (Spatial Analysis)
- D) Azure AI Document Intelligence

> ✅ **Answer: C — Azure AI Vision (Spatial Analysis)**
> Spatial Analysis analyzes live video streams to understand people's presence, movement, and counts in physical spaces.

---

**Q32.** OCR stands for:
- A) Object Classification Recognition
- B) Optical Character Recognition
- C) Output Content Reading
- D) Online Content Retrieval

> ✅ **Answer: B — Optical Character Recognition**
> OCR is the technology that converts images of text into machine-readable text.

---

**Q33.** Which Azure AI service should you use to extract text from a scanned handwritten form?
- A) Azure AI Language
- B) Azure AI Vision (Read API / OCR)
- C) Azure AI Document Intelligence
- D) Azure AI Speech

> ✅ **Answer: B — Azure AI Vision (Read API)**
> The Azure AI Vision Read API handles both printed and handwritten text extraction from images.

---

**Q34.** A model is trained to identify whether fruit in images is ripe or unripe. What type of computer vision task is this?
- A) Object Detection
- B) Semantic Segmentation
- C) Image Classification
- D) OCR

> ✅ **Answer: C — Image Classification**
> Classifying the whole image into a category (ripe/unripe) is image classification.

---

## Domain 4 – NLP (Questions 35–42)

**Q35.** Which Azure AI Language feature determines whether customer reviews are positive, negative, or neutral?
- A) Key Phrase Extraction
- B) Named Entity Recognition
- C) Sentiment Analysis
- D) Language Detection

> ✅ **Answer: C — Sentiment Analysis**
> Sentiment Analysis evaluates text to determine the emotional tone (positive, negative, neutral, or mixed).

---

**Q36.** You want to build a chatbot that understands "Book a table for 4 people at 7 PM" and extracts the intent and entities. Which service should you use?
- A) Azure AI Translator
- B) Conversational Language Understanding (CLU)
- C) Azure AI Speech
- D) Custom Question Answering

> ✅ **Answer: B — Conversational Language Understanding (CLU)**
> CLU identifies user intent (BookTable) and extracts entities (NumberOfPeople=4, Time=7PM).

---

**Q37.** Which Azure service would you use to automatically detect the language of a user's message?
- A) Azure AI Translator
- B) Azure AI Language (Language Detection feature)
- C) Azure AI Speech
- D) Both A and B

> ✅ **Answer: D — Both A and B**
> Both Azure AI Translator and Azure AI Language include language detection capabilities.

---

**Q38.** A company wants to build a system where users can ask questions about its product manual and get instant answers. Which service is best suited?
- A) CLU (Conversational Language Understanding)
- B) Azure AI Search
- C) Custom Question Answering
- D) Azure AI Translator

> ✅ **Answer: C — Custom Question Answering**
> Custom Question Answering (part of Azure AI Language) lets you build FAQ bots from documents and Q&A pairs.

---

**Q39.** Which Azure AI Speech feature converts spoken audio into written text?
- A) Text-to-Speech
- B) Speech Translation
- C) Speech-to-Text
- D) Speaker Recognition

> ✅ **Answer: C — Speech-to-Text**
> Speech-to-Text (STT) transcribes spoken audio into written text in real time or batch.

---

**Q40.** A global company needs to automatically translate customer support emails from 20 different languages into English. Which Azure service should they use?
- A) Azure AI Language
- B) Azure AI Speech
- C) Azure AI Translator
- D) Azure OpenAI

> ✅ **Answer: C — Azure AI Translator**
> Azure AI Translator is the dedicated neural machine translation service supporting 100+ languages.

---

**Q41.** What is Named Entity Recognition (NER)?
- A) Detecting sentiment in text
- B) Identifying and categorizing real-world entities (people, places, dates) in text
- C) Classifying the entire text into a category
- D) Translating text between languages

> ✅ **Answer: B — Identifying and categorizing real-world entities in text**
> NER detects entities like Person, Location, Organization, Date, and more from unstructured text.

---

**Q42.** In CLU (Conversational Language Understanding), what is an "utterance"?
- A) The user's goal or action
- B) A specific piece of information extracted from input
- C) A sample phrase the user might say
- D) The AI model's response

> ✅ **Answer: C — A sample phrase the user might say**
> An utterance is a training example showing how users might express an intent ("Book me a flight to London").

---

## Domain 5 – Generative AI (Questions 43–50)

**Q43.** What is Azure OpenAI Service?
- A) A search engine powered by OpenAI
- B) Microsoft's managed deployment of OpenAI models within Azure with enterprise features
- C) An open-source alternative to OpenAI
- D) A service exclusively for image generation

> ✅ **Answer: B**
> Azure OpenAI Service provides access to OpenAI's models (GPT-4, DALL-E, etc.) within Azure's secure, enterprise-grade infrastructure.

---

**Q44.** A generative AI model states confidently that a historical event occurred in 1754, but the actual date was 1864. What AI risk does this demonstrate?
- A) Bias
- B) Jailbreaking
- C) Hallucination
- D) Overfitting

> ✅ **Answer: C — Hallucination**
> Hallucination is when an AI model generates plausible-sounding but factually incorrect information with confidence.

---

**Q45.** Which technique grounds LLM responses in your own documents to reduce hallucinations?
- A) Fine-tuning
- B) Retrieval-Augmented Generation (RAG)
- C) Temperature reduction
- D) Few-shot prompting

> ✅ **Answer: B — Retrieval-Augmented Generation (RAG)**
> RAG retrieves relevant content from your knowledge base and provides it as context to the LLM, grounding responses in real data.

---

**Q46.** You want a language model to always respond formally and present itself as "Azure Support Assistant." Where should you put these instructions?
- A) In the user prompt
- B) In the system prompt
- C) As a few-shot example
- D) In the temperature setting

> ✅ **Answer: B — In the system prompt**
> The system prompt sets the model's persona, tone, and instructions before any user interaction begins.

---

**Q47.** Which Azure service allows you to build a custom AI assistant (copilot) for your business using a low-code interface?
- A) Azure Machine Learning
- B) Azure OpenAI Service
- C) Microsoft Copilot Studio
- D) Azure AI Language

> ✅ **Answer: C — Microsoft Copilot Studio**
> Copilot Studio (formerly Power Virtual Agents) is the low-code/no-code tool for building custom AI-powered chatbots and copilots.

---

**Q48.** What does a higher temperature setting do in an LLM?
- A) Makes responses more deterministic and predictable
- B) Speeds up response generation
- C) Makes responses more creative and varied
- D) Reduces the context window size

> ✅ **Answer: C — Makes responses more creative and varied**
> Temperature controls randomness. Higher = more creative/diverse outputs. Lower (near 0) = more predictable/consistent outputs.

---

**Q49.** Azure AI Content Safety filters content across four harm categories. Which of the following is NOT one of them?
- A) Hate
- B) Violence
- C) Spam
- D) Sexual

> ✅ **Answer: C — Spam**
> The four categories in Azure AI Content Safety are: Hate, Violence, Sexual, and Self-harm. Spam is not one of them.

---

**Q50.** Which of the following BEST describes the purpose of Azure AI Foundry (formerly Azure AI Studio)?
- A) A code editor for Python scripts
- B) A portal and SDK platform for building, testing, and deploying enterprise AI applications using Azure AI models
- C) A service for training custom ML models from scratch
- D) A database for storing AI model outputs

> ✅ **Answer: B**
> Azure AI Foundry is the unified platform for building enterprise AI solutions using Azure OpenAI and other Azure AI services.

---

## 📊 Score Tracker

| Domain | Questions | Your Score |
|--------|-----------|-----------|
| Domain 1 – AI Workloads | Q1–Q12 (12 questions) | /12 |
| Domain 2 – Machine Learning | Q13–Q25 (13 questions) | /13 |
| Domain 3 – Computer Vision | Q26–Q34 (9 questions) | /9 |
| Domain 4 – NLP | Q35–Q42 (8 questions) | /8 |
| Domain 5 – Generative AI | Q43–Q50 (8 questions) | /8 |
| **Total** | **50 questions** | **/50** |

**Scoring guide:**
- 45–50 ✅ Excellent — You're ready!
- 38–44 🟡 Good — Review weak areas
- Below 38 🔴 Keep studying — revisit the notes

---

> ⬅️ [Back to AI-900 Index](./index.md) | ⚡ [Quick Reference](./quick-reference.md)
