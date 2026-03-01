# 🟠 Module 2: Implement Decision-Support Solutions (10–15%)

📘 [← Back to AI-102 Index](./index.md)

---

## 2.1 Create Decision-Support Solutions for Data Monitoring and Content Delivery

### 📉 Azure Anomaly Detector

**What it is:** A time-series AI service that automatically detects anomalies (unexpected spikes, dips, or pattern changes) in data — without needing to train a custom ML model.

**Use cases:** IoT sensor monitoring, financial fraud detection, infrastructure metrics, usage telemetry.

**Detection Modes:**

| Mode | Description |
|------|-------------|
| **Univariate** | Anomaly detection on a single time-series variable |
| **Multivariate** | Correlates multiple variables together; detects system-level anomalies |
| **Streaming** | Detect anomalies in real time as data arrives |
| **Batch** | Detect anomalies across a historical dataset all at once |

**Univariate API — Key Concepts:**

| Concept | Description |
|---------|-------------|
| `granularity` | Time interval between data points (hourly, daily, monthly, etc.) |
| `sensitivity` | Controls how sensitive the algorithm is (1–99; higher = more anomalies flagged) |
| `isAnomaly` | Boolean in response — whether the point is anomalous |
| `expectedValue` | What the model expected the value to be |
| `upperMargin` / `lowerMargin` | Bounds of the expected range |

```python
from azure.ai.anomalydetector import AnomalyDetectorClient
from azure.ai.anomalydetector.models import DetectRequest, TimeSeriesPoint
from azure.core.credentials import AzureKeyCredential
from datetime import datetime

client = AnomalyDetectorClient(endpoint, AzureKeyCredential(key))

series = [
    TimeSeriesPoint(timestamp=datetime(2024, 1, i), value=float(v))
    for i, v in enumerate([1.1, 1.2, 1.0, 1.3, 5.8, 1.1, 1.2], start=1)
]

request = DetectRequest(
    series=series,
    granularity="daily",
    sensitivity=95
)

response = client.detect_univariate_entire_series(request)

for i, point in enumerate(response.is_anomaly):
    if point:
        print(f"Anomaly detected at index {i}: value={series[i].value}")
```

**Multivariate Anomaly Detection:**
- Requires training a model on historical data (minimum 1 month, at least 15,000 data points)
- Use cases: server fleet monitoring (CPU + memory + disk together), manufacturing sensor correlation
- Returns `contributionScore` per variable to explain which dimension caused the anomaly

---

### 🎯 Azure AI Personalizer

**What it is:** A reinforcement learning service that selects the best content item (action) to show to a user based on contextual features. Learns from user feedback (rewards) over time.

**Core Concepts:**

| Concept | Description |
|---------|-------------|
| **Actions** | The items to choose from (e.g., articles, product recommendations, UI layouts) |
| **Context** | User/environment features at the time of decision (device, time of day, location, past behaviour) |
| **Reward** | Feedback signal (0.0 to 1.0) indicating how good the chosen action was |
| **Rank** | API call that selects the best action given the context |
| **Reward wait time** | How long to wait for a reward signal before defaulting to 0 |
| **Exploration** | Percentage of time Personalizer tries random actions to discover better ones |

**Workflow:**
```
1. App calls Rank API with context + list of actions
2. Personalizer returns the recommended actionId + eventId
3. App shows the recommended content to the user
4. App observes user behaviour (click, purchase, etc.)
5. App calls Reward API with eventId and reward score (0.0–1.0)
6. Personalizer updates its model based on the reward
```

```python
# Rank request
rank_request = {
    "contextFeatures": [
        {"user": {"age": "young", "device": "mobile"}},
        {"time": {"hour": 14, "dayOfWeek": "Monday"}}
    ],
    "actions": [
        {"id": "article1", "features": [{"topic": "sports", "length": "short"}]},
        {"id": "article2", "features": [{"topic": "tech", "length": "long"}]},
    ],
    "excludedActions": [],
    "eventId": "event-xyz-123",
    "deferActivation": False
}

# Reward request (called after observing user interaction)
reward_request = {"value": 1.0}  # 1.0 = user engaged, 0.0 = no engagement
```

**Learning Modes:**

| Mode | Description |
|------|-------------|
| Online | Personalizer makes and learns from live decisions |
| Apprentice | Personalizer observes and learns from existing logic without influencing outcomes |
| Offline Evaluation | Evaluates policy performance using historical logs |

---

### 🚨 Azure Content Moderator

> ⚠️ **Note:** Content Moderator is being superseded by **Azure AI Content Safety**. New projects should use Content Safety. Content Moderator may still appear on the exam in legacy scenario questions.

**What it is:** A moderation service for text, images, and videos — detecting adult content, offensive language, and PII.

**Moderation Types:**

| Type | Description |
|------|-------------|
| **Image Moderation** | Detects adult/racy content, text in images, face detection |
| **Text Moderation** | Detects profanity, PII (email, phone, SSN), custom term lists |
| **Video Moderation** | Frame-by-frame analysis of video content |

**Human Review Tool:**
- Web-based portal for human reviewers to approve/reject AI moderation decisions
- Supports teams, workflows, and audit trails
- Used when AI confidence is low or for compliance requirements

**Text Moderation Response:**
```json
{
  "OriginalText": "This is some sample text with a phone 555-1234",
  "NormalizedText": "This is some sample text with a phone ****-****",
  "PII": {
    "Phone": [{ "Index": 38, "Text": "555-1234" }]
  },
  "Terms": [],
  "Status": { "Code": 3000, "Description": "OK" }
}
```

---

### 📊 Azure Metrics Advisor

**What it is:** A more advanced, fully managed anomaly detection platform built on top of Anomaly Detector — adds a web portal, alerting, root cause analysis, and feedback loops.

**Key Features:**

| Feature | Description |
|---------|-------------|
| **Data feeds** | Connect to Azure Blob, SQL, Cosmos DB, Kusto, etc. |
| **Metrics** | Define the time-series to monitor within each feed |
| **Incident hub** | Groups related anomalies into incidents with root cause analysis |
| **Diagnostic tree** | Visual drill-down to identify which dimension caused an anomaly |
| **Alerting** | Notify via hooks (webhook, email, Teams) when anomalies are detected |
| **Feedback** | Analysts can label anomalies as expected (e.g., holidays) to improve the model |

**When to use Metrics Advisor vs Anomaly Detector:**

| | Anomaly Detector | Metrics Advisor |
|--|-----------------|----------------|
| Interface | API only | API + Web portal |
| Multi-metric | Via multivariate API | Native support |
| Alerting | Manual (via Azure Monitor) | Built-in hooks |
| Root cause analysis | No | Yes |
| Human feedback loop | No | Yes |
| Best for | Developers integrating into apps | Data/Operations teams |

---

📘 [← Back to AI-102 Index](./index.md)
