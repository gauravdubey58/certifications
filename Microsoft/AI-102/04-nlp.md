# 🟡 Module 4: Implement Natural Language Processing Solutions (30–35%)

📘 [← Back to AI-102 Index](./index.md)

> ⚠️ This is the **largest domain** — 30–35% of the exam. Focus significant study time here.

---

## 4.1 Azure AI Language Service

Azure AI Language (formerly Text Analytics + LUIS + QnA Maker) is the unified NLP service.

### 🔍 Pre-built Text Analysis Features

**Key Features:**

| Feature | Description | Input |
|---------|-------------|-------|
| **Language Detection** | Identifies the language of a text snippet | Text |
| **Sentiment Analysis** | Positive, negative, neutral, or mixed; with confidence scores | Text |
| **Opinion Mining** | Aspect-based sentiment — sentiment about specific entities within text | Text |
| **Key Phrase Extraction** | Main topics and concepts in the text | Text |
| **Named Entity Recognition (NER)** | Identifies entities (people, places, dates, organisations, etc.) | Text |
| **Entity Linking** | Resolves ambiguous entities to Wikipedia/knowledge base entries | Text |
| **PII Detection** | Detects and can redact Personally Identifiable Information | Text |
| **Text Summarisation** | Abstractive and extractive document summarisation | Document |
| **Custom NER** | Train custom entity types on your own labelled data | Custom |
| **Custom Text Classification** | Single-label or multi-label text classifiers | Custom |

```python
from azure.ai.textanalytics import TextAnalyticsClient
from azure.core.credentials import AzureKeyCredential

client = TextAnalyticsClient(endpoint, AzureKeyCredential(key))

documents = ["I love the product but the delivery was terrible."]

# Sentiment + Opinion Mining
response = client.analyze_sentiment(documents, show_opinion_mining=True)
for doc in response:
    print(f"Sentiment: {doc.sentiment}")
    for sentence in doc.sentences:
        for opinion in sentence.mined_opinions:
            print(f"  Aspect: {opinion.target.text} — {opinion.target.sentiment}")
            for assessment in opinion.assessments:
                print(f"    Assessment: {assessment.text} — {assessment.sentiment}")

# Named Entity Recognition
ner_response = client.recognize_entities(documents)
for doc in ner_response:
    for entity in doc.entities:
        print(f"Entity: {entity.text} | Category: {entity.category} | "
              f"Confidence: {entity.confidence_score:.2f}")

# Key Phrases
kp_response = client.extract_key_phrases(documents)
for doc in kp_response:
    print(f"Key phrases: {doc.key_phrases}")
```

---

### 💬 Conversational Language Understanding (CLU)

CLU is the **replacement for LUIS (Language Understanding)**. It classifies user utterances into intents and extracts entities.

**Core Concepts:**

| Concept | Description |
|---------|-------------|
| **Intent** | What the user wants to do (e.g., `BookFlight`, `CancelOrder`) |
| **Entity** | Specific data extracted from the utterance (e.g., destination city, date) |
| **Utterance** | An example of what users might say for a given intent |
| **None intent** | Catches utterances that don't match any defined intent |
| **Confidence threshold** | Minimum score to accept a prediction |

**Entity Types:**

| Type | Description | Example |
|------|-------------|---------|
| **Machine-learned** | Trained from your labelled examples | Order ID, product name |
| **List** | Exact matches from a predefined list | "New York", "NYC" → `New York` |
| **Regex** | Pattern-based matching | Email addresses, phone numbers |
| **Prebuilt** | Built-in recognisers (DateTime, Number, Email, URL) | "next Tuesday" → datetime |

**CLU Workflow:**
1. Create Language resource
2. Create CLU project in Language Studio
3. Add intents and label utterance examples
4. Add entities and label them in utterances
5. Train the model
6. Evaluate (review precision/recall per intent)
7. Deploy to an endpoint
8. Call the prediction API

```python
from azure.ai.language.conversations import ConversationAnalysisClient
from azure.core.credentials import AzureKeyCredential

client = ConversationAnalysisClient(endpoint, AzureKeyCredential(key))

result = client.analyze_conversation(
    task={
        "kind": "Conversation",
        "analysisInput": {
            "conversationItem": {
                "id": "1",
                "participantId": "user",
                "text": "Book a flight to London tomorrow"
            }
        },
        "parameters": {
            "projectName": "MyBookingApp",
            "deploymentName": "production",
            "stringIndexType": "Utf16CodeUnit"
        }
    }
)

prediction = result["result"]["prediction"]
print(f"Top intent: {prediction['topIntent']}")
print(f"Confidence: {prediction['intents'][0]['confidence']:.2f}")
for entity in prediction["entities"]:
    print(f"Entity: {entity['category']} = {entity['text']}")
```

---

### ❓ Custom Question Answering

Replaces QnA Maker. Builds question-answering systems from knowledge bases (FAQs, manuals, documents).

**Knowledge Base Sources:**
- URLs (FAQ pages, documentation)
- Files (PDF, Word, Excel, TXT)
- Manual Q&A pairs

**Key Features:**

| Feature | Description |
|---------|-------------|
| **Chit-chat** | Pre-built conversational responses (friendly, witty, caring, etc.) |
| **Multi-turn** | Follow-up prompts for guided conversation flows |
| **Active Learning** | Automatically suggests new Q&A pairs from unanswered questions |
| **Confidence threshold** | Reject answers below a minimum confidence score |
| **Short answer extraction** | Returns a precise span from within a longer answer |

```python
from azure.ai.language.questionanswering import QuestionAnsweringClient

client = QuestionAnsweringClient(endpoint, AzureKeyCredential(key))

response = client.get_answers(
    question="What is the return policy?",
    project_name="MyKnowledgeBase",
    deployment_name="production"
)

for answer in response.answers:
    print(f"Answer: {answer.answer}")
    print(f"Confidence: {answer.confidence:.2f}")
    print(f"Source: {answer.source}")
```

---

## 4.2 Azure AI Speech Service

### 🎙️ Speech Service Capabilities

| Capability | Description |
|------------|-------------|
| **Speech-to-Text (STT)** | Transcribe audio to text in real time or from a file |
| **Text-to-Speech (TTS)** | Synthesise natural-sounding speech from text |
| **Speech Translation** | Translate spoken audio to another language |
| **Speaker Recognition** | Identify or verify speakers by their voice |
| **Keyword Recognition** | Detect a specific wake word or phrase in an audio stream |
| **Custom Speech** | Fine-tune STT for domain-specific vocabulary or acoustic conditions |
| **Custom Neural Voice** | Create a custom synthetic voice (requires approval) |

**Speech-to-Text SDK:**
```python
import azure.cognitiveservices.speech as speechsdk

speech_config = speechsdk.SpeechConfig(
    subscription=key,
    region="eastus"
)
speech_config.speech_recognition_language = "en-US"

# From microphone
audio_config = speechsdk.audio.AudioConfig(use_default_microphone=True)
recogniser = speechsdk.SpeechRecognizer(speech_config, audio_config)

result = recogniser.recognize_once_async().get()

if result.reason == speechsdk.ResultReason.RecognizedSpeech:
    print(f"Recognised: {result.text}")
elif result.reason == speechsdk.ResultReason.NoMatch:
    print("No speech recognised")

# From audio file
audio_config = speechsdk.audio.AudioConfig(filename="audio.wav")
```

**Text-to-Speech SDK:**
```python
speech_config = speechsdk.SpeechConfig(subscription=key, region="eastus")
speech_config.speech_synthesis_voice_name = "en-US-JennyNeural"

synthesiser = speechsdk.SpeechSynthesizer(speech_config)
result = synthesiser.speak_text_async("Hello, welcome to Azure AI Speech.").get()
```

**SSML (Speech Synthesis Markup Language):**
Fine-grained control over pronunciation, pauses, pitch, and rate:
```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-US-JennyNeural">
    <prosody rate="slow" pitch="+10%">
      Hello! Welcome to <emphasis level="strong">Azure AI Speech</emphasis>.
    </prosody>
    <break time="500ms"/>
    How can I help you today?
  </voice>
</speak>
```

**Custom Speech:**
- Upload domain-specific audio + transcripts to improve accuracy
- Useful for medical, legal, or technical vocabularies
- Acoustic models handle noise/accents; language models handle jargon

---

## 4.3 Azure AI Translator

### 🌍 Translator Capabilities

| Capability | Description |
|------------|-------------|
| **Text Translation** | Translate text between 100+ languages |
| **Transliteration** | Convert text from one script to another (e.g., Arabic to Latin) |
| **Language Detection** | Identify the source language |
| **Dictionary** | Lookup word-for-word translations and examples |
| **Document Translation** | Translate entire documents preserving formatting (async) |
| **Custom Translator** | Train custom models for domain-specific terminology |

**Text Translation API:**
```python
import requests, uuid

url = "https://api.cognitive.microsofttranslator.com/translate"
params = {
    "api-version": "3.0",
    "from": "en",
    "to": ["fr", "de", "ja"]
}
headers = {
    "Ocp-Apim-Subscription-Key": key,
    "Ocp-Apim-Subscription-Region": "eastus",
    "Content-Type": "application/json",
    "X-ClientTraceId": str(uuid.uuid4())
}
body = [{"text": "Hello, how are you?"}]

response = requests.post(url, params=params, headers=headers, json=body)
for translation in response.json()[0]["translations"]:
    print(f"{translation['to']}: {translation['text']}")
```

**Custom Translator:**
- Train models on parallel corpora (source + translated text pairs)
- Best for industry-specific terminology (legal, medical, engineering)
- Deployed in the same Translator endpoint — reference by `Category` parameter

**Pricing:**
- Free tier (F0): 2 million characters/month
- Standard (S1): Per million characters

---

## 4.4 Language Understanding — Additional Topics

### 🔗 Orchestration Workflow

Combines multiple language services (CLU, Custom Q&A, LUIS) behind a single API endpoint.

**Use case:** A single utterance might be answered by QnA (factual question) or CLU (action intent) — orchestration routes automatically.

```python
# Orchestration project routes to CLU or Q&A based on confidence
result = client.analyze_conversation(
    task={
        "kind": "Conversation",
        "parameters": {
            "projectName": "MyOrchestrationProject",  # Orchestration type project
            "deploymentName": "production"
        },
        "analysisInput": {
            "conversationItem": {
                "id": "1", "participantId": "user",
                "text": "What is your return policy?"
            }
        }
    }
)
# Result includes which sub-project answered and with what confidence
```

---

### 📝 Text Analysis for Health

A specialised pre-built model for extracting medical entities from clinical text.

**Entity Categories:**

| Category | Examples |
|----------|---------|
| `Diagnosis` | Diabetes, Hypertension |
| `MedicationName` | Metformin, Lisinopril |
| `Dosage` | 500mg, twice daily |
| `BodyStructure` | Left knee, myocardium |
| `Symptom` | Chest pain, shortness of breath |
| `Procedure` | CT scan, blood test |

> ⚠️ Health text analytics is for **informational/analytical purposes only** — NOT a medical device and must not be used for clinical diagnosis.

---

📘 [← Back to AI-102 Index](./index.md)
