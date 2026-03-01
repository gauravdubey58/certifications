# 🔵 Module 1: Plan and Manage an Azure AI Solution (15–20%)

📘 [← Back to AI-102 Index](./index.md)

---

## 1.1 Select the Appropriate Azure AI Service

### 🧠 Azure AI Services Overview

Azure AI Services (formerly Cognitive Services) are cloud-based APIs that add AI capabilities to applications without requiring machine learning expertise.

**Service Categories:**

| Category | Services |
|----------|---------|
| **Vision** | Azure AI Vision, Custom Vision, Face API, Video Indexer |
| **Speech** | Speech-to-Text, Text-to-Speech, Speaker Recognition, Speech Translation |
| **Language** | Language Service, Translator, CLU, QnA Maker → now in Language |
| **Decision** | Anomaly Detector, Content Moderator, Personalizer |
| **OpenAI** | GPT-4, GPT-3.5, DALL-E, Embeddings, Whisper |
| **Search** | Azure AI Search (knowledge mining) |
| **Document Intelligence** | Form Recognizer (now Document Intelligence) |

**Choosing the Right Service — Decision Guide:**

| Scenario | Service to Use |
|----------|---------------|
| Detect objects in images | Azure AI Vision — Image Analysis |
| Identify people in photos | Face API |
| Transcribe audio to text | Azure Speech — Speech-to-Text |
| Translate between languages | Azure Translator |
| Extract key phrases from text | Azure Language — Key Phrase Extraction |
| Build a question-answering chatbot | Azure Language — Custom Question Answering |
| Classify customer intents | Azure Language — CLU (Conversational Language Understanding) |
| Extract data from invoices/forms | Azure AI Document Intelligence |
| Search and index documents | Azure AI Search |
| Generate text / chat completions | Azure OpenAI Service |
| Detect anomalies in time-series data | Azure Anomaly Detector |
| Moderate user-generated content | Azure Content Moderator / Content Safety |
| Analyse video for insights | Azure Video Indexer |

---

### 🏗️ Azure AI Services Resource Types

| Resource Type | Description |
|--------------|-------------|
| **Single-service** | One endpoint/key per service (e.g., only Vision). Best for fine-grained billing and access control |
| **Multi-service (AI Services)** | One endpoint/key for multiple services. Simpler management, one bill |

> 💡 Use **multi-service** for development and prototyping. Use **single-service** in production when you need isolated billing, quotas, or compliance per service.

---

## 1.2 Plan, Create, and Deploy an Azure AI Service

### 📋 Planning Considerations

**Before creating a resource, consider:**

| Consideration | Options |
|--------------|---------|
| Region | Choose based on data residency, latency, and feature availability |
| Pricing tier | Free (F0), Standard (S0), Commitment tiers |
| Commitment tiers | Pre-purchase usage at discounted rates for predictable workloads |
| Network access | Public, selected networks (VNet + service endpoints), private endpoint |

**Creating via CLI:**
```bash
# Create a multi-service Azure AI Services resource
az cognitiveservices account create \
  --name myAIService \
  --resource-group myRG \
  --kind CognitiveServices \
  --sku S0 \
  --location eastus \
  --yes

# Get the endpoint and keys
az cognitiveservices account show \
  --name myAIService --resource-group myRG \
  --query properties.endpoint

az cognitiveservices account keys list \
  --name myAIService --resource-group myRG
```

**Deploying with ARM / Bicep:**
```bicep
resource aiService 'Microsoft.CognitiveServices/accounts@2023-05-01' = {
  name: 'myAIService'
  location: 'eastus'
  sku: { name: 'S0' }
  kind: 'CognitiveServices'
  properties: {
    publicNetworkAccess: 'Disabled'
    networkAcls: {
      defaultAction: 'Deny'
      virtualNetworkRules: []
    }
  }
}
```

---

### 🌐 Accessing Azure AI Services

**Authentication Methods:**

| Method | How |
|--------|-----|
| **Subscription Key** | Pass `Ocp-Apim-Subscription-Key: <key>` in the request header |
| **Token-based** | Exchange key for a 10-minute JWT token; pass as `Authorization: Bearer <token>` |
| **Managed Identity** | Assign `Cognitive Services User` RBAC role to the identity — no keys in code |

**REST API Example (Image Analysis):**
```python
import requests

endpoint = "https://myaiservice.cognitiveservices.azure.com/"
key = "<your-key>"

headers = {
    "Ocp-Apim-Subscription-Key": key,
    "Content-Type": "application/json"
}

body = {"url": "https://example.com/photo.jpg"}

response = requests.post(
    f"{endpoint}vision/v3.2/analyze?visualFeatures=Description,Tags",
    headers=headers,
    json=body
)
print(response.json())
```

**SDK Example (.NET):**
```csharp
var client = new ImageAnalysisClient(
    new Uri(endpoint),
    new AzureKeyCredential(key));

var result = await client.AnalyzeAsync(
    new Uri("https://example.com/photo.jpg"),
    VisualFeatures.Caption | VisualFeatures.Tags);

Console.WriteLine(result.Value.Caption.Text);
```

---

## 1.3 Manage, Monitor, and Secure an Azure AI Service

### 🔐 Security

**Network Security:**

| Option | Description |
|--------|-------------|
| Public endpoint | Default — accessible from anywhere |
| Selected networks | Allow specific VNets and IP ranges only |
| Private endpoint | Fully private access via Azure Private Link |
| Service tags | Use `CognitiveServicesManagement` tag in NSG rules |

**Key Rotation:**
```bash
# Regenerate key1
az cognitiveservices account keys regenerate \
  --name myAIService --resource-group myRG --key-name key1
```

> 💡 Always store keys in **Azure Key Vault** and access them via Managed Identity — never hard-code keys in source code.

**RBAC Roles:**

| Role | Permissions |
|------|------------|
| `Cognitive Services Contributor` | Create and manage resources |
| `Cognitive Services User` | Use the service (call APIs) |
| `Cognitive Services Custom Vision Contributor` | Train custom vision models |
| `Cognitive Services OpenAI User` | Call Azure OpenAI APIs |
| `Cognitive Services OpenAI Contributor` | Deploy and manage OpenAI models |

---

### 📊 Monitoring

**Diagnostic Settings:** Route metrics and logs to Log Analytics, Storage Account, or Event Hubs.

```bash
az monitor diagnostic-settings create \
  --name myDiagSettings \
  --resource /subscriptions/.../providers/Microsoft.CognitiveServices/accounts/myAIService \
  --logs '[{"category": "Audit","enabled": true}]' \
  --metrics '[{"category": "AllMetrics","enabled": true}]' \
  --workspace /subscriptions/.../workspaces/myLAW
```

**Key Metrics to Monitor:**

| Metric | Description |
|--------|-------------|
| `TotalCalls` | Number of API calls made |
| `SuccessfulCalls` | Calls that returned 2xx |
| `TotalErrors` | 4xx and 5xx responses |
| `Latency` | Response time in milliseconds |
| `TotalTokenCalls` | Token-based usage (OpenAI) |
| `BlockedCalls` | Calls rejected due to throttling |

**Useful KQL Queries:**
```kusto
// Error rate over time
AzureDiagnostics
| where ResourceType == "COGNITIVESERVICES/ACCOUNTS"
| where ResultType != "Success"
| summarize count() by bin(TimeGenerated, 5m), ResultType
| render timechart

// Top 5 operations by call volume
AzureDiagnostics
| where ResourceType == "COGNITIVESERVICES/ACCOUNTS"
| summarize count() by OperationName
| top 5 by count_ desc
```

---

### 🔄 Deployment and Container Options

Azure AI Services can be deployed in **containers** for offline, edge, or compliance scenarios.

| Container | Image |
|-----------|-------|
| Language Detection | `mcr.microsoft.com/azure-cognitive-services/language` |
| Sentiment Analysis | `mcr.microsoft.com/azure-cognitive-services/sentiment` |
| Key Phrase Extraction | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
| Speech-to-Text | `mcr.microsoft.com/azure-cognitive-services/speechservices/speech-to-text` |
| Read (OCR) | `mcr.microsoft.com/azure-cognitive-services/vision/read` |

**Container requirements:**
- Must connect to Azure at startup to validate billing credentials
- Pass `ApiKey`, `Billing` (endpoint), and `Eula=accept` as environment variables
- Data does NOT flow to Azure — only billing information is sent

```bash
docker run -d \
  -p 5000:5000 \
  -e ApiKey=<your-key> \
  -e Billing=https://myaiservice.cognitiveservices.azure.com/ \
  -e Eula=accept \
  mcr.microsoft.com/azure-cognitive-services/sentiment:latest
```

---

## 1.4 Implement AI Solutions Responsibly

### ⚖️ Microsoft's Responsible AI Principles

| Principle | Description |
|-----------|-------------|
| **Fairness** | AI systems should treat all people fairly and avoid discriminatory outcomes |
| **Reliability & Safety** | AI should perform reliably and safely across all conditions |
| **Privacy & Security** | AI must protect personal data and respect individual privacy |
| **Inclusiveness** | AI should empower everyone, including people with disabilities |
| **Transparency** | People should understand how AI decisions are made |
| **Accountability** | People must be held accountable for AI systems and their outcomes |

> 💡 These 6 principles form the backbone of Microsoft's responsible AI framework. Expect scenario-based questions asking which principle is being violated or upheld.

---

### 🛡️ Azure Content Safety

A dedicated service for detecting harmful content across text and images.

**Harm Categories:**

| Category | Description |
|----------|-------------|
| **Hate** | Attacks on groups based on identity attributes |
| **Violence** | Physical harm, weapons, threatening language |
| **Sexual** | Sexually explicit or adult content |
| **Self-harm** | Content promoting self-injury or suicide |

**Severity Levels:** 0–7 scale (0 = safe, 7 = extremely harmful)

```python
from azure.ai.contentsafety import ContentSafetyClient
from azure.ai.contentsafety.models import AnalyzeTextOptions
from azure.core.credentials import AzureKeyCredential

client = ContentSafetyClient(endpoint, AzureKeyCredential(key))

request = AnalyzeTextOptions(text="User-generated text here")
response = client.analyze_text(request)

for category in response.categories_analysis:
    print(f"{category.category}: severity {category.severity}")
```

---

### 📋 Impact Assessment & Transparency Notes

**Transparency Note:** Microsoft publishes Transparency Notes for AI services detailing intended use cases, limitations, and responsible deployment guidance.

**Limited Access Services:** Certain capabilities (Face identification, Custom Neural Voice, Speaker Recognition) require **application and approval** due to higher risk potential:
- Face API — Identify/Verify features
- Custom Neural Voice — creating synthetic voices

**Key practices:**
- Conduct impact assessments before deployment
- Document data sources and model limitations
- Provide human oversight for high-stakes decisions (healthcare, justice, finance)
- Implement feedback mechanisms for users to report AI errors

---

📘 [← Back to AI-102 Index](./index.md)
