# 🟣 Module 3: Implement Azure AI Vision Solutions (15–20%)

📘 [← Back to AI-102 Index](./index.md)

---

## 3.1 Analyse Images with Azure AI Vision

### 👁️ Azure AI Vision — Image Analysis

Azure AI Vision (formerly Computer Vision) provides pre-built models for analysing images and extracting visual information.

**Visual Features (v4.0 Image Analysis API):**

| Feature | Description |
|---------|-------------|
| **Caption** | Single human-readable sentence describing the image |
| **Dense Captions** | Captions for multiple regions within the image |
| **Tags** | Keywords describing objects, scenes, and concepts |
| **Objects** | Detects objects with bounding boxes and confidence scores |
| **People** | Detects people and returns bounding boxes (no identity) |
| **Smart Crops** | Suggests cropping regions based on content relevance |
| **Read (OCR)** | Extracts text from images and documents |

```python
from azure.ai.vision.imageanalysis import ImageAnalysisClient
from azure.ai.vision.imageanalysis.models import VisualFeatures
from azure.core.credentials import AzureKeyCredential

client = ImageAnalysisClient(endpoint, AzureKeyCredential(key))

result = client.analyze_from_url(
    image_url="https://example.com/photo.jpg",
    visual_features=[
        VisualFeatures.CAPTION,
        VisualFeatures.TAGS,
        VisualFeatures.OBJECTS,
        VisualFeatures.READ
    ],
    language="en"
)

# Caption
print(f"Caption: {result.caption.text} ({result.caption.confidence:.2f})")

# Tags
for tag in result.tags.list:
    print(f"Tag: {tag.name} ({tag.confidence:.2f})")

# OCR Text
for block in result.read.blocks:
    for line in block.lines:
        print(f"Text: {line.text}")
```

---

### 📖 Read API (OCR)

The **Read API** (part of Azure AI Vision) extracts printed and handwritten text from images and multi-page PDFs.

**Key Characteristics:**

| Feature | Detail |
|---------|--------|
| Async operation | Submit → poll for result (operationId) |
| Multi-page support | PDFs and multi-page TIFFs |
| Languages | 164+ printed language support; handwriting for English |
| Output structure | Pages → Lines → Words (with bounding polygons) |

**REST API Flow:**
```python
# Step 1: Submit the image
submit_response = requests.post(
    f"{endpoint}vision/v3.2/read/analyze",
    headers={"Ocp-Apim-Subscription-Key": key, "Content-Type": "application/json"},
    json={"url": "https://example.com/document.pdf"}
)
operation_url = submit_response.headers["Operation-Location"]

# Step 2: Poll until complete
import time
while True:
    result = requests.get(operation_url,
        headers={"Ocp-Apim-Subscription-Key": key}).json()
    if result["status"] in ["succeeded", "failed"]:
        break
    time.sleep(1)

# Step 3: Extract text
for page in result["analyzeResult"]["readResults"]:
    for line in page["lines"]:
        print(line["text"])
```

---

### 👤 Face API

Detects, analyses, and verifies human faces in images.

> ⚠️ **Limited Access:** Face identification (1:1 and 1:N) and celebrity recognition require **application and approval** due to privacy concerns. Detection (location, attributes) is unrestricted.

**Capabilities:**

| Capability | Access |
|------------|--------|
| **Detect** | Face location, age estimate, emotion, head pose, landmarks | Unrestricted |
| **Verify** | Are these two faces the same person? (1:1) | Requires approval |
| **Identify** | Who is this person from a known group? (1:N) | Requires approval |
| **Group** | Cluster faces by similarity without known identities | Restricted |
| **Find Similar** | Find faces similar to a query face | Restricted |

**Face Detection Attributes:**
```python
from azure.cognitiveservices.vision.face import FaceClient
from msrest.authentication import CognitiveServicesCredentials

client = FaceClient(endpoint, CognitiveServicesCredentials(key))

detected_faces = client.face.detect_with_url(
    url="https://example.com/person.jpg",
    return_face_attributes=["Age", "Gender", "Emotion", "HeadPose", "Blur"],
    detection_model="detection_03",
    recognition_model="recognition_04"
)

for face in detected_faces:
    print(f"Age: {face.face_attributes.age}")
    print(f"Emotion: {face.face_attributes.emotion}")
    print(f"Face rectangle: {face.face_rectangle}")
```

**Person Groups (for identification):**
1. Create a `PersonGroup`
2. Add `Person` objects to the group
3. Add face images to each `Person`
4. Train the `PersonGroup`
5. Call `Identify` with a detected face

---

## 3.2 Implement Custom Azure AI Vision Models

### 🎨 Custom Vision Service

Build custom **image classification** and **object detection** models without deep ML expertise.

**Project Types:**

| Type | Description | Output |
|------|-------------|--------|
| **Classification — Multiclass** | One tag per image | Tag + confidence score |
| **Classification — Multilabel** | Multiple tags per image | Multiple tags + confidence scores |
| **Object Detection** | Locate and label objects with bounding boxes | Bounding box + tag + confidence |

**Training Domains:**

| Domain | Optimised For |
|--------|--------------|
| General | Broad variety of images |
| Food | Food and dishes |
| Landmarks | Recognisable geographic/architectural landmarks |
| Retail | Products and logos |
| Compact | Edge deployment — exportable to ONNX, TensorFlow, CoreML, Docker |

> 💡 Use **Compact** domains when you need to export and deploy the model to edge devices or containers.

**Minimum Training Data:**
- At least **5 images per tag** (30+ recommended for good accuracy)
- At least **2 tags** for classification

```python
from azure.cognitiveservices.vision.customvision.training import CustomVisionTrainingClient
from azure.cognitiveservices.vision.customvision.prediction import CustomVisionPredictionClient
from msrest.authentication import ApiKeyCredentials

# Training
train_credentials = ApiKeyCredentials(in_headers={"Training-key": training_key})
trainer = CustomVisionTrainingClient(endpoint, train_credentials)

project = trainer.create_project("MyClassifier")
tag_cat = trainer.create_tag(project.id, "cat")
tag_dog = trainer.create_tag(project.id, "dog")

# Upload images (with tags)
with open("cat.jpg", "rb") as f:
    trainer.create_images_from_data(project.id, f.read(), [tag_cat.id])

# Train
iteration = trainer.train_project(project.id)
while iteration.status == "Training":
    iteration = trainer.get_iteration(project.id, iteration.id)

# Publish
trainer.publish_iteration(project.id, iteration.id, "myModel", prediction_resource_id)

# Prediction
pred_credentials = ApiKeyCredentials(in_headers={"Prediction-key": prediction_key})
predictor = CustomVisionPredictionClient(endpoint, pred_credentials)

with open("test.jpg", "rb") as f:
    results = predictor.classify_image(project.id, "myModel", f.read())

for prediction in results.predictions:
    print(f"{prediction.tag_name}: {prediction.probability:.2%}")
```

**Exporting Models:**
```python
# Export to ONNX for edge deployment
export = trainer.export_iteration(project.id, iteration.id, "ONNX")
# Also supports: TensorFlow, CoreML, DockerFile, VAIDK, OpenVino
```

---

## 3.3 Analyse Videos with Azure Video Indexer

### 🎥 Azure Video Indexer

A cloud service that extracts deep insights from videos using AI — transcription, face detection, topic extraction, scene detection, and more.

**Insights Extracted:**

| Category | Insights |
|----------|---------|
| **Audio** | Transcription, translation, speaker diarisation, keywords, sentiments |
| **Visual** | Scene detection, shot boundaries, keyframe extraction, label detection |
| **People** | Face detection, face identification (requires approval), emotions |
| **Text** | OCR from video frames, logos, brand detection |

**Key Concepts:**

| Concept | Description |
|---------|-------------|
| **Account** | Free (limited) or Standard (paid, higher limits, API access) |
| **Widget** | Embeddable player + insights UI — `player` and `insights` widget types |
| **Index** | The extracted insights for a video, accessible via API |
| **Custom models** | Train custom Person, Language, and Brand models for better accuracy |

**API Workflow:**
```python
import requests

# 1. Get access token
token_response = requests.get(
    f"https://api.videoindexer.ai/auth/{location}/Accounts/{account_id}/AccessToken",
    headers={"Ocp-Apim-Subscription-Key": api_key}
)
access_token = token_response.text.strip('"')

# 2. Upload video
upload_response = requests.post(
    f"https://api.videoindexer.ai/{location}/Accounts/{account_id}/Videos",
    params={
        "accessToken": access_token,
        "name": "My Video",
        "videoUrl": "https://example.com/video.mp4"
    }
)
video_id = upload_response.json()["id"]

# 3. Get index (poll until ready)
index_response = requests.get(
    f"https://api.videoindexer.ai/{location}/Accounts/{account_id}/Videos/{video_id}/Index",
    params={"accessToken": access_token}
)
insights = index_response.json()
```

**Custom Models in Video Indexer:**

| Model Type | Purpose |
|------------|---------|
| **Person** | Identify specific people (requires approval) |
| **Language** | Custom vocabulary for domain-specific transcription accuracy |
| **Brands** | Detect company/product names in audio and text |
| **Animated characters** | Detect and group animated characters using Custom Vision integration |

---

📘 [← Back to AI-102 Index](./index.md)
