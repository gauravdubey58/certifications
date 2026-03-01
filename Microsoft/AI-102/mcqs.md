# 📝 AI-102: Designing and Implementing a Microsoft Azure AI Solution — 50 Practice MCQs

> Mapped to official exam domains:
> - **Domain 1:** Plan and Manage an Azure AI Solution — 15–20%
> - **Domain 2:** Implement Decision-Support Solutions — 10–15%
> - **Domain 3:** Implement Azure AI Vision Solutions — 15–20%
> - **Domain 4:** Implement Natural Language Processing Solutions — 30–35%
> - **Domain 5:** Implement Knowledge Mining and Document Intelligence — 10–15%
> - **Domain 6:** Implement Generative AI Solutions — 10–15%

📘 [← Back to AI-102 Index](./index.md)

---

## 🔵 Domain 1: Plan and Manage an Azure AI Solution (Q1–Q8)

---

**Q1.** A company wants to use multiple Azure AI Services across different departments, each requiring separate billing and quota management. Which resource type should be created for each department?

- A) A single multi-service Azure AI Services resource shared by all departments
- B) A separate single-service resource per department per capability
- C) One Azure OpenAI resource shared across departments
- D) A multi-service resource per department

✅ **Answer: B**  
*Single-service resources provide isolated endpoints, keys, quotas, and billing per service per department. Multi-service resources consolidate under one bill — useful for development, but not for isolated billing/access control per department.*

---

**Q2.** A developer hard-codes an Azure AI Services subscription key directly in application code. A security audit flags this as a risk. What is the recommended remediation?

- A) Rotate the key monthly and update the code each time
- B) Use a Managed Identity with `Cognitive Services User` RBAC role — no key required in code
- C) Encrypt the key using AES-256 before embedding it in code
- D) Store the key in a configuration file outside the repository

✅ **Answer: B**  
*Managed Identities eliminate the need for credentials in code entirely. The `Cognitive Services User` role grants the identity permission to call the service API. Keys should be stored in Key Vault at minimum, but Managed Identity is the preferred keyless approach.*

---

**Q3.** An organisation needs to deploy Azure AI Services in an environment where data must never leave their private network — not even for API calls. Which configuration achieves this?

- A) Restrict access to selected VNet subnets using service endpoints
- B) Deploy a private endpoint for the Azure AI Services resource and disable public access
- C) Enable firewall rules to restrict access to specific IP ranges
- D) Use container deployment with the `Eula=accept` flag

✅ **Answer: B**  
*Private endpoints route all traffic over the Azure backbone — no public internet traversal. Disabling public access ensures no external endpoint exposure. Service endpoints (A) still use public IPs; IP filtering (C) still uses the public endpoint.*

---

**Q4.** A containerised Azure AI Services deployment needs to process data offline without internet connectivity. During startup the container fails with an authentication error. What is the most likely cause?

- A) The container image is outdated and must be updated
- B) The container cannot reach the Azure billing endpoint to validate the API key at startup
- C) Containers do not support Azure AI Services
- D) The `Eula` environment variable was not set correctly

✅ **Answer: B**  
*Azure AI Services containers require internet connectivity at startup to validate billing credentials with the Azure endpoint. While data processing is local, the `Billing` and `ApiKey` environment variables must point to a valid Azure endpoint reachable at launch.*

---

**Q5.** You need to monitor the rate of failed API calls to an Azure AI Vision resource over time. Which Azure Monitor metric should you track?

- A) `TotalCalls`
- B) `Latency`
- C) `TotalErrors`
- D) `BlockedCalls`

✅ **Answer: C**  
*`TotalErrors` captures all 4xx and 5xx HTTP error responses from the AI Services resource. `BlockedCalls` are throttled requests (429). `TotalCalls` includes all calls (successful and failed). `Latency` measures response time.*

---

**Q6.** Which Microsoft Responsible AI principle is being upheld when an AI system provides explanations for its decisions so that users can understand and challenge outcomes?

- A) Fairness
- B) Accountability
- C) Transparency
- D) Reliability

✅ **Answer: C**  
*Transparency means people should be able to understand how and why AI makes decisions. Providing explainability and interpretability directly upholds this principle. Accountability refers to humans being responsible for AI systems and outcomes.*

---

**Q7.** A developer wants to call the Azure Face API's `Identify` feature to recognise individuals from a known group. What must they do before this capability is available?

- A) Upgrade their Azure AI Services resource to Premium tier
- B) Submit an application for limited access approval from Microsoft
- C) Enable the Face API identify feature in the Azure Portal under the resource settings
- D) Train a Custom Vision model first, then link it to the Face resource

✅ **Answer: B**  
*Face identification (1:N) is a Limited Access feature. Microsoft requires organisations to apply and be approved before using it, due to privacy and misuse concerns. This applies to both identify and verify features.*

---

**Q8.** An Azure AI Services resource needs to be accessed by a Logic App, an Azure Function, and a third-party application. Each must have independent keys for auditing purposes. What is the best approach?

- A) Create one resource with three separate subscription keys
- B) Create three separate Azure AI Services resources, one per consumer
- C) Use one resource; assign different RBAC roles to each identity
- D) Generate separate SAS tokens for each consumer

✅ **Answer: C**  
*Azure AI Services supports RBAC — each consumer (Logic App MSI, Function MSI, service principal) can be granted the `Cognitive Services User` role independently, enabling per-identity access tracking via Azure Monitor/diagnostic logs without managing separate keys.*

---

## 🟠 Domain 2: Implement Decision-Support Solutions (Q9–Q14)

---

**Q9.** A manufacturing company monitors 50 sensor metrics simultaneously and wants to detect when the sensors together indicate an anomalous system state — even if each individual sensor appears normal. Which Anomaly Detector capability should be used?

- A) Univariate batch detection
- B) Univariate streaming detection
- C) Multivariate anomaly detection
- D) Metrics Advisor with 50 data feeds

✅ **Answer: C**  
*Multivariate anomaly detection analyses the correlations between multiple variables together. An anomaly may only be detectable when several metrics deviate simultaneously from their joint expected behaviour — invisible to univariate analysis of each sensor.*

---

**Q10.** An e-commerce website uses Azure AI Personalizer to recommend products. The system is newly deployed and has little usage data. Which learning mode should be used initially to learn from existing business logic without affecting the live customer experience?

- A) Online mode
- B) Apprentice mode
- C) Offline evaluation mode
- D) Exploration mode with 100% exploration rate

✅ **Answer: B**  
*Apprentice mode allows Personalizer to observe the existing system's decisions and learn from them without influencing outcomes. Once performance approaches the baseline, you switch to Online mode to let Personalizer take control of decisions.*

---

**Q11.** In Azure AI Personalizer, after showing a recommended article to a user, the app detects the user read it for more than 60 seconds. The app awards a reward score of 0.9. Which API endpoint is called to submit this feedback?

- A) The Rank API with a `rewardValue` field
- B) The Reward API with the `eventId` and reward value
- C) The Events API to log the interaction
- D) The Evaluate API to update the model

✅ **Answer: B**  
*After receiving the Rank response with an `eventId`, the app must call the Reward API with the same `eventId` and a score between 0.0 and 1.0. This signal is used by the reinforcement learning engine to improve future recommendations.*

---

**Q12.** Azure Content Moderator's text moderation API detects a phone number in user-submitted content. In which section of the JSON response will the phone number appear?

- A) `Terms`
- B) `PII.Phone`
- C) `NormalizedText`
- D) `Classification`

✅ **Answer: B**  
*The Content Moderator text moderation API returns PII (Personally Identifiable Information) — including phone numbers, email addresses, and SSNs — in the `PII` object of the response, with a `Phone` array containing the detected values and their character positions.*

---

**Q13.** A data operations team wants to view dashboards showing anomaly trends, drill down into root cause dimensions, and provide feedback to mark known seasonal patterns as non-anomalous. Which Azure service best supports this workflow?

- A) Azure Anomaly Detector API
- B) Azure Metrics Advisor
- C) Azure Monitor with custom workbooks
- D) Azure Machine Learning anomaly detection pipeline

✅ **Answer: B**  
*Metrics Advisor provides a full web portal with incident dashboards, diagnostic trees for root cause analysis, and a feedback mechanism to mark events (like holidays or scheduled maintenance) as expected — capabilities not available in the raw Anomaly Detector API.*

---

**Q14.** What is the minimum number of data points required to train an Azure Anomaly Detector **multivariate** model?

- A) 100 data points per variable
- B) 1,000 data points per variable
- C) At least 15,000 data points total across all variables (minimum 1 month of data)
- D) 500 data points per variable

✅ **Answer: C**  
*The multivariate Anomaly Detector requires a minimum of approximately 15,000 data points across all variables, typically equivalent to at least one month of historical data, to learn the normal correlation patterns between variables.*

---

## 🟣 Domain 3: Implement Azure AI Vision Solutions (Q15–Q22)

---

**Q15.** A developer submits an image to the Azure AI Vision Image Analysis API with `visualFeatures=Tags,Objects` and receives the response. What does the `Objects` feature return that `Tags` does not?

- A) Object names with higher confidence scores
- B) Bounding box coordinates (location) for each detected object in the image
- C) More object categories than Tags
- D) Colour information for each object

✅ **Answer: B**  
*`Objects` detects specific items and returns their **bounding box coordinates** (location within the image). `Tags` returns a list of descriptive keywords for the whole image without spatial location. Both detect objects, but only `Objects` tells you where they are.*

---

**Q16.** You submit a multi-page PDF to the Azure AI Vision Read API. When you poll for results, what is the correct status value that indicates the operation is complete and results are available?

- A) `running`
- B) `completed`
- C) `succeeded`
- D) `done`

✅ **Answer: C**  
*The Read API's async operation returns a status of `"notStarted"`, `"running"`, `"succeeded"`, or `"failed"`. You must poll until the status is `"succeeded"` before accessing the results.*

---

**Q17.** A Custom Vision model is trained to classify product images as "damaged" or "intact". The model will run on edge IoT devices with limited compute. Which training domain should be selected?

- A) General (A1)
- B) General (A2)
- C) Compact (S1)
- D) Retail

✅ **Answer: C**  
*Compact domains are specifically optimised for **export and edge deployment**. They produce smaller models that can be exported to ONNX, TensorFlow Lite, CoreML, or Docker for offline use on constrained edge devices.*

---

**Q18.** A Custom Vision object detection project needs to detect logos in marketing images. After training with 20 labelled images, the model's precision is low. What is the most effective way to improve it?

- A) Increase the training iterations from 100 to 1000
- B) Switch from object detection to image classification
- C) Add more labelled training images, especially with varied backgrounds and scales
- D) Change the prediction threshold from 50% to 10%

✅ **Answer: C**  
*More diverse, high-quality labelled training data is the most effective way to improve Custom Vision model accuracy. Varying backgrounds, lighting, scales, and orientations help the model generalise. Lowering the threshold (D) increases recall but reduces precision.*

---

**Q19.** Which Face API operation determines whether two face images belong to the same person, returning a boolean result and a confidence score?

- A) `Detect`
- B) `Find Similar`
- C) `Verify`
- D) `Identify`

✅ **Answer: C**  
*`Verify` (1:1) compares two specific faces and returns `isIdentical: true/false` with a confidence score. `Identify` (1:N) matches a face against a group of known people. `Find Similar` finds faces similar to a query face without a yes/no answer.*

---

**Q20.** Azure Video Indexer has transcribed a customer support call. A manager wants to understand which specific product issues drove negative sentiment in the call. Which Video Indexer insight should be analysed?

- A) Keyframe extraction
- B) Scene detection
- C) Transcript with sentiment and topics
- D) Speaker diarisation only

✅ **Answer: C**  
*Video Indexer extracts a transcript with timestamps, per-segment sentiment labels, and topic tags. The manager can review which segments and topics correlate with negative sentiment — identifying specific product issues discussed in the call.*

---

**Q21.** A Video Indexer account needs to accurately recognise domain-specific technical jargon in speech transcriptions (e.g., medical terminology). Which custom model should be configured?

- A) Custom Person model
- B) Custom Brand model
- C) Custom Language model
- D) Custom Acoustic model

✅ **Answer: C**  
*A custom **Language model** (also called Language Adaptation) is trained with domain-specific text data to improve transcription accuracy for specialised vocabulary. The Brand model (B) detects company/product names; the Person model (A) identifies individuals.*

---

**Q22.** The Image Analysis v4.0 API's `smartCrops` feature is requested for an image. What does this feature return?

- A) Colour-adjusted versions of the image at different aspect ratios
- B) Suggested bounding boxes for cropping the image to focus on the most visually important content
- C) The image segmented into foreground and background layers
- D) A list of crop-safe regions that avoid faces

✅ **Answer: B**  
*Smart Crops returns one or more suggested crop rectangles (bounding boxes) based on the image's visual content importance — helping applications crop images to standard aspect ratios while preserving the most meaningful subject matter.*

---

## 🟡 Domain 4: Implement NLP Solutions (Q23–Q36)

---

**Q23.** A customer service platform needs to detect when a user is complaining about slow delivery (aspect: "delivery", sentiment: "negative") versus praising product quality (aspect: "product", sentiment: "positive") — both in the same review. Which Azure AI Language feature should be used?

- A) Sentiment Analysis (basic)
- B) Key Phrase Extraction
- C) Opinion Mining (Aspect-based Sentiment Analysis)
- D) Named Entity Recognition

✅ **Answer: C**  
*Opinion Mining (a feature of Sentiment Analysis) identifies specific **targets** (aspects like "delivery", "product quality") within the text and the **sentiment** expressed toward each one independently — enabling granular, aspect-level feedback analysis.*

---

**Q24.** You are building a CLU model. During testing you notice that utterances like "What's the weather?" are being incorrectly classified into your application's intents. What is the correct way to handle these irrelevant utterances?

- A) Delete them from the test set
- B) Add them as examples to a `None` intent
- C) Lower the confidence threshold for all intents
- D) Create a new intent called `WeatherQuery` and add them there

✅ **Answer: B**  
*The `None` intent is specifically designed to catch utterances that don't match any of your application's intended intents. Adding out-of-scope examples to `None` trains the model to confidently reject them, preventing false classifications.*

---

**Q25.** A CLU model has an entity `flightDestination` that needs to accept any free-form city name typed by the user. Which entity type is most appropriate?

- A) List entity
- B) Regex entity
- C) Prebuilt entity (geolocation)
- D) Machine-learned entity

✅ **Answer: D**  
*A machine-learned entity learns to extract values from context using labelled examples — ideal for open-ended, free-form values like city names where you can't enumerate all possibilities (List) and there's no fixed pattern (Regex).*

---

**Q26.** A knowledge base is built from a company's FAQ webpage using Custom Question Answering. After deployment, users report they are getting no answers for questions about the company's new return policy which was added to the FAQ page last week. What must be done?

- A) Retrain the language model on the new page
- B) Sync the knowledge base by re-importing/refreshing the source URL and retrain
- C) Delete and recreate the knowledge base from scratch
- D) Add the new Q&A pairs directly to the knowledge base as manual entries

✅ **Answer: B**  
*Custom Question Answering does not automatically sync with source URLs. When source content changes, you must manually refresh/re-import the source to pull in updates, then retrain and redeploy the knowledge base.*

---

**Q27.** A multi-turn Custom Question Answering conversation guides users through troubleshooting steps. After an initial answer, the service should prompt the user with "Is your device plugged in?" as a follow-up. What feature enables this?

- A) Active Learning
- B) Chit-chat
- C) Follow-up prompts (multi-turn)
- D) Short answer extraction

✅ **Answer: C**  
*Follow-up prompts enable multi-turn conversations by defining child Q&A pairs that appear as suggested responses after an answer. This creates guided, branching dialogue flows — such as troubleshooting wizards.*

---

**Q28.** Which Azure AI Speech feature would you use to generate subtitles for a live video stream in a language different from the original spoken language?

- A) Custom Speech
- B) Speech Translation
- C) Text-to-Speech
- D) Speaker Recognition

✅ **Answer: B**  
*Speech Translation converts real-time spoken audio from one language directly into text in a target language — in a single streaming operation. This is the right solution for live subtitle generation in a different language.*

---

**Q29.** A speech synthesis output sounds robotic when reading medical drug names. Which approach will most improve the pronunciation of these specialised terms?

- A) Use a Custom Neural Voice
- B) Lower the `prosody rate` in SSML
- C) Use SSML `<phoneme>` tags to specify the correct pronunciation using phonetic notation
- D) Switch to a different pre-built neural voice

✅ **Answer: C**  
*SSML's `<phoneme>` element allows specifying exact phonetic pronunciation (using IPA or IETF standards) for words that TTS models mispronounce — such as drug names, acronyms, or technical terms.*

---

**Q30.** Which Azure AI Translator feature allows you to create domain-specific translation models trained on your own bilingual content — for example, to improve translation accuracy for legal contracts?

- A) Dictionary lookup
- B) Document Translation
- C) Custom Translator
- D) Neural Machine Translation with the `category` parameter

✅ **Answer: C**  
*Custom Translator allows training custom NMT models on your own parallel bilingual corpora (source + translation pairs). Once trained, you reference the custom model via the `Category` parameter in translation requests.*

---

**Q31.** An application calls the Azure AI Language service to detect PII and must ensure that detected phone numbers and email addresses are replaced with placeholder text before storing the output. Which feature and operation should be used?

- A) Named Entity Recognition with `category=Phone,Email`
- B) PII Detection with the `redactedText` output field
- C) Key Phrase Extraction filtered for PII categories
- D) Custom Named Entity Recognition with PII labels

✅ **Answer: B**  
*The PII Detection feature returns both the detected PII entities AND a `redactedText` field where each detected PII span is replaced with a placeholder (e.g., `*****`). This is available directly in the SDK response without additional processing.*

---

**Q32.** You are using the Azure AI Language SDK to classify support tickets into one of six categories, where each ticket belongs to exactly one category. Which project type should you create in Language Studio?

- A) Custom multi-label text classification
- B) Custom single-label text classification
- C) Conversational Language Understanding
- D) Named Entity Recognition

✅ **Answer: B**  
*Single-label classification assigns each document to exactly one class. Multi-label classification assigns one or more classes per document. Since each ticket belongs to exactly one category, single-label is the correct choice.*

---

**Q33.** A user submits the utterance "Turn the living room lights off". The CLU model correctly identifies the intent as `TurnOffLight`. However, the entity `roomName` shows low confidence for "living room". What is the most effective way to improve entity extraction confidence for this value?

- A) Add "living room" to a List entity
- B) Add more labelled utterances to the same intent that contain room names in various positions
- C) Lower the entity extraction confidence threshold
- D) Add "living room" as a Regex entity

✅ **Answer: B**  
*For machine-learned entities, adding more labelled utterance examples with the entity appearing in different positions and contexts helps the model learn the extraction pattern more robustly. The entity "living room" should be labelled in multiple utterances across different phrasings.*

---

**Q34.** Which Azure AI Language feature automatically generates concise summaries from long documents by selecting the most representative sentences — without generating new text?

- A) Abstractive summarisation
- B) Key phrase extraction
- C) Extractive summarisation
- D) Document classification

✅ **Answer: C**  
*Extractive summarisation selects and returns the most important sentences directly from the original document — no new text is generated. Abstractive summarisation (A) generates new summary text by paraphrasing the document.*

---

**Q35.** A company wants to combine both a CLU model (for action intents) and a Custom Question Answering knowledge base (for FAQ responses) behind a single prediction endpoint. Which Azure AI Language feature enables this?

- A) Custom multi-intent CLU project
- B) Orchestration Workflow project
- C) Knowledge Base chaining
- D) CLU with fallback to QnA

✅ **Answer: B**  
*An Orchestration Workflow project in Azure AI Language acts as a router — it receives utterances and routes them to the most appropriate connected project (CLU or Custom QnA) based on which one has the highest confidence response.*

---

**Q36.** The Azure AI Language service returns the following sentiment result for a sentence: `"sentiment": "mixed"`. What does this indicate?

- A) The confidence scores for positive and negative are both exactly 50%
- B) The sentence contains both positive and negative sentiment toward different aspects
- C) The model was unable to determine a clear sentiment
- D) The sentence is in a language not supported by the model

✅ **Answer: B**  
*A "mixed" sentiment result means the sentence expresses both positive and negative opinions — for example, "The food was great but the service was awful." It reflects genuine ambivalence, not uncertainty or a model limitation.*

---

## 🟤 Domain 5: Knowledge Mining and Document Intelligence (Q37–Q43)

---

**Q37.** An AI Search index has a `Description` field set as `searchable` and `retrievable`, but NOT `filterable`. A user query includes `$filter=Description eq 'ocean view'`. What will happen?

- A) The filter works because the field is searchable
- B) The query returns all documents regardless of the filter
- C) The query returns an error because `Description` is not filterable
- D) The filter is silently ignored and all results are returned

✅ **Answer: C**  
*`$filter` expressions can only be applied to fields that have the `filterable` attribute set to `true`. Attempting to filter on a non-filterable field returns an error from the search service.*

---

**Q38.** You need to index PDFs stored in Azure Blob Storage with Azure AI Search and extract text from scanned (image-based) PDFs. Which skillset skill must be included?

- A) `EntityRecognitionSkill`
- B) `OcrSkill`
- C) `DocumentExtractionSkill`
- D) `ImageAnalysisSkill`

✅ **Answer: B**  
*`OcrSkill` runs Optical Character Recognition on image content (including scanned PDFs rendered as images), extracting the text so it can be further enriched and indexed. Without OCR, image-based PDFs yield no text content.*

---

**Q39.** An AI Search indexer processes documents nightly. A new document is added to the Blob Storage source between runs. When will this document appear in search results?

- A) Immediately, because Azure AI Search syncs in real time
- B) At the next scheduled indexer run
- C) Only after the index is manually rebuilt
- D) After 24 hours, due to propagation delay

✅ **Answer: B**  
*An indexer processes data on its schedule (or when triggered manually). New documents added between runs are not indexed until the next run completes. For near real-time indexing, use the `push` API to write documents directly to the index.*

---

**Q40.** A Document Intelligence custom model was trained on 10 invoices from one supplier but performs poorly on invoices from other suppliers with different layouts. What is the recommended solution?

- A) Train a custom Template model with more diverse training examples from all suppliers
- B) Use the prebuilt invoice model instead
- C) Switch to a Custom Neural model and add more diverse training examples from all suppliers
- D) Both B and C are valid approaches

✅ **Answer: D**  
*The prebuilt invoice model (B) handles diverse invoice layouts without custom training. A Custom Neural model (C) can be trained for specialised requirements. Both are valid — Neural models handle layout variation better than Template models for multi-supplier scenarios.*

---

**Q41.** Which Azure AI Search query type re-ranks search results using language models to surface the most semantically relevant documents, even if they don't contain the exact query keywords?

- A) Full Lucene query with fuzzy matching
- B) Simple query with wildcard
- C) Semantic search with semantic ranking
- D) Vector search with cosine similarity

✅ **Answer: C**  
*Semantic search applies a language model re-ranker on top of the initial BM25 keyword results, improving relevance by understanding meaning rather than just keyword matching. It also generates captions and answers for the top results.*

---

**Q42.** An organisation uses Azure AI Search with AI enrichment. They want to persist the AI-extracted metadata (entities, key phrases, image tags) to Azure Table Storage for Power BI analysis — separate from the search index. Which feature enables this?

- A) Skillset output field mappings
- B) Knowledge Store with table projections
- C) Index projections
- D) Azure Synapse Link for AI Search

✅ **Answer: B**  
*The Knowledge Store feature in a skillset definition allows enriched document data to be persisted as projections into Azure Blob Storage (files/objects) or Azure Table Storage (tables). This enables independent downstream analysis without using the search index.*

---

**Q43.** A Document Intelligence `prebuilt-layout` model is used to analyse a purchase order. Which specific data type will this model return that `prebuilt-read` will NOT?

- A) Handwritten text
- B) Printed text in multiple languages
- C) Table structure with row and column information
- D) Page dimensions and rotation

✅ **Answer: C**  
*`prebuilt-layout` returns the full document structure including **tables** (with cell coordinates, row/column span), selection marks (checkboxes), and paragraph roles (headings, sections). `prebuilt-read` returns only text lines and words without structural table data.*

---

## 🟢 Domain 6: Implement Generative AI Solutions (Q44–Q50)

---

**Q44.** A company wants to use Azure OpenAI but requires that all API traffic stay within their private Azure network. Which Azure feature enables this?

- A) Azure OpenAI content filtering
- B) Azure Private Endpoint for the Azure OpenAI resource
- C) VPN Gateway connecting on-premises to Azure
- D) Azure Front Door for traffic routing

✅ **Answer: B**  
*A Private Endpoint creates a private IP address in your VNet for the Azure OpenAI resource. Combined with disabling public network access, all traffic to the service stays within the Azure backbone — never traversing the public internet.*

---

**Q45.** When calling the Azure OpenAI Chat Completions API, a developer sets `temperature: 0`. What effect does this have on the model's output?

- A) The model returns no output
- B) The model output is maximally random and creative
- C) The model output is deterministic — it always selects the most probable next token
- D) The model uses a slower but more accurate reasoning process

✅ **Answer: C**  
*`temperature: 0` makes the model greedy — it always selects the single most probable token, producing deterministic and consistent output. This is ideal for factual, structured tasks where consistency is required.*

---

**Q46.** A developer needs to deploy a Retrieval-Augmented Generation (RAG) solution where Azure OpenAI answers questions about proprietary company documents. What is the primary purpose of the **embedding model** in this architecture?

- A) To summarise documents before storing them in AI Search
- B) To convert text (documents and queries) into numerical vectors for semantic similarity search
- C) To fine-tune the GPT model on company documents
- D) To translate documents into a standard language before indexing

✅ **Answer: B**  
*Embedding models (like `text-embedding-3-small`) convert text into dense numerical vectors. In RAG, both documents and user queries are embedded, enabling semantic similarity search — finding contextually relevant passages even without exact keyword matches.*

---

**Q47.** An Azure OpenAI deployment's content filter returns a `"filtered": true` response for the `"violence"` category when processing a user message. What should the application do?

- A) Retry the request with a lower `temperature` setting
- B) Log the incident and return an appropriate error or alternative response to the user
- C) Switch to a different model deployment that has less restrictive filtering
- D) Ignore the filter result and display the response anyway

✅ **Answer: B**  
*When content is filtered, the application must handle it gracefully — logging the event for compliance/audit purposes and returning a safe, user-appropriate message. Bypassing or ignoring content filters violates Azure OpenAI usage policies.*

---

**Q48.** A GPT-4 model is producing inconsistent answers to the same factual question across multiple requests. The developer wants strictly consistent outputs. Which two settings should be adjusted?

- A) Set `temperature: 0` and `top_p: 1`
- B) Set `temperature: 0` and `seed: <fixed_integer>`
- C) Set `max_tokens: 1` and `frequency_penalty: 1`
- D) Set `top_p: 0` and `presence_penalty: -2`

✅ **Answer: B**  
*Setting `temperature: 0` makes the model deterministic (always picks the highest-probability token). The `seed` parameter (when supported) further ensures reproducibility by fixing the random seed. Together they maximise output consistency across calls.*

---

**Q49.** An application sends a user question and retrieves 5 relevant document chunks from AI Search. These chunks are passed to GPT-4 in the prompt. The GPT response contains a claim that is NOT in any of the retrieved chunks. What Responsible AI concern does this represent?

- A) Privacy violation
- B) Hallucination — the model generated unsupported information
- C) A content safety filter bypass
- D) Insufficient context length

✅ **Answer: B**  
*Hallucination occurs when a language model generates factual claims not grounded in its provided context or training data. In RAG applications, this is a critical Responsible AI concern — the model should be instructed to only answer from the provided context and flag when information is unavailable.*

---

**Q50.** An application uses DALL-E 3 to generate product imagery. The developer notices the `revised_prompt` in the response differs from the original prompt. Why does this occur?

- A) DALL-E 3 does not support the prompt format and defaulted to a generic image
- B) The content safety filter rewrote the prompt to remove harmful elements
- C) DALL-E 3 automatically enhances and refines prompts to improve image quality and safety
- D) The API version being used does not support custom prompts

✅ **Answer: C**  
*DALL-E 3 automatically enhances user prompts to produce higher-quality, more detailed images. The `revised_prompt` field shows what prompt was actually used for generation. This can also involve safety rewriting, but the primary reason is quality enhancement.*

---

## 📊 Summary by Domain

| Domain | Questions | Exam Weight |
|--------|-----------|------------|
| 1 – Plan and Manage | Q1–Q8 | 15–20% |
| 2 – Decision Support | Q9–Q14 | 10–15% |
| 3 – Vision | Q15–Q22 | 15–20% |
| 4 – NLP | Q23–Q36 | 30–35% |
| 5 – Knowledge Mining | Q37–Q43 | 10–15% |
| 6 – Generative AI | Q44–Q50 | 10–15% |
| **Total** | **50** | **100%** |

---

## 🔗 Study Resources

| Resource | Link |
|----------|------|
| Official Study Guide | [AI-102 Study Guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-102) |
| Free Practice Assessment | [Practice Questions](https://learn.microsoft.com/en-us/credentials/certifications/exams/ai-102/practice/assessment?assessment-type=practice&assessmentId=61) |
| Azure AI Services Docs | [Azure AI Services](https://learn.microsoft.com/en-us/azure/ai-services/) |
| Azure OpenAI Docs | [Azure OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/) |
| Language Studio | [Language Studio](https://language.cognitive.azure.com/) |
| Vision Studio | [Vision Studio](https://portal.vision.cognitive.azure.com/) |

---

📘 [← Back to AI-102 Index](./index.md)

*Good luck with AI-102! 🚀 NLP is 30–35% of the exam — make sure to spend significant time on CLU, Language Service, and Speech.*
