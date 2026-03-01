# Domain 3 – Computer Vision Workloads on Azure `15–20%`

> ⬅️ [Back to AI-900 Index](./index.md)

---

## 3.1 Computer Vision Concepts

Computer vision enables machines to **interpret and understand visual information** from images and videos.

### Common Computer Vision Tasks

| Task | Description | Example |
|------|-------------|---------|
| **Image Classification** | Identify what the primary subject of an image is | "This is a cat" |
| **Object Detection** | Identify and locate multiple objects with bounding boxes | "There's a cat at [x,y,w,h] and a dog at [x,y,w,h]" |
| **Semantic Segmentation** | Classify every pixel in an image by category | Self-driving car road/pedestrian detection |
| **Instance Segmentation** | Like semantic, but distinguishes individual instances | Count specific people in a crowd |
| **Image Analysis** | Extract rich metadata from images (tags, captions, objects) | "outdoor, sunset, beach, people" |
| **Optical Character Recognition (OCR)** | Extract text from images | Reading receipts, ID cards, handwritten notes |
| **Facial Recognition** | Detect, analyze, and identify human faces | Face verification, emotion detection |
| **Spatial Analysis** | Understand people's movement and presence in physical space | Counting people entering a store |

---

## 3.2 Azure AI Vision (Computer Vision Service)

Azure AI Vision is a pre-built AI service for analyzing images.

### Capabilities

| Capability | What It Does |
|-----------|--------------|
| **Image Analysis** | Tags, captions, objects, brands, colors, adult content detection |
| **OCR (Read API)** | Extracts printed and handwritten text from images and documents |
| **Spatial Analysis** | Analyzes live video feed for people counting, movement, distance |
| **Smart Crop / Thumbnail** | Generates thumbnails focusing on the most important area |
| **Background Removal** | Removes or blurs image backgrounds |
| **Dense Captioning** | Generates captions for multiple regions of an image |

### OCR vs Document Intelligence

| | Azure AI Vision OCR | Azure AI Document Intelligence |
|--|---------------------|-------------------------------|
| Focus | General text extraction from images | Form and document understanding |
| Output | Raw text | Structured fields (key-value pairs) |
| Use case | Signs, screenshots, general images | Invoices, receipts, ID cards, tax forms |

---

## 3.3 Azure AI Custom Vision

Allows you to **train your own image classification and object detection models** without deep ML expertise.

### Workflow
1. Create a Custom Vision project (classification or detection)
2. Upload and **label training images**
3. **Train** the model (one click)
4. **Evaluate** performance (precision, recall per tag)
5. **Publish** as an endpoint (prediction API)
6. **Iterate** — add more images for poor-performing classes

### Classification vs Detection Projects

| | Image Classification | Object Detection |
|--|---------------------|-----------------|
| Task | "What is in the image?" | "What is in the image and WHERE?" |
| Output | Class labels + confidence | Bounding boxes + labels + confidence |
| Labels per image | One (or multiple) tags | Bounding box coordinates + tag |
| Example | Is this plant healthy or diseased? | Find all defects on a circuit board |

> ⭐ **Exam Tip:** Custom Vision is the right choice when you need to classify your OWN custom categories that Azure AI Vision doesn't support out of the box (e.g., your company's specific product types).

---

## 3.4 Azure AI Face Service

Specialized service for **face-related tasks**.

### Capabilities

| Capability | Description | Notes |
|-----------|-------------|-------|
| **Face Detection** | Detect faces in images with bounding boxes | Always available |
| **Face Analysis** | Attributes like glasses, head pose, blur, exposure | Available |
| **Face Verification** | Compare two faces — same person? | Requires approval |
| **Face Identification** | Identify a face against a gallery of known people | Requires approval |
| **Face Grouping** | Group unknown faces by similarity | Requires approval |
| **Liveness Detection** | Determine if face is real vs spoofed | Available |

> ⭐ **Exam Tip:** Face **identification** (matching a face to a known person) and **verification** (are two faces the same person?) require Microsoft approval due to responsible AI concerns around privacy and surveillance.

### Responsible Use of Face Service
- Microsoft has restricted some Face capabilities due to potential for misuse
- Emotion recognition has been retired from general availability
- Facial identification requires a Limited Access application

---

## 3.5 Azure AI Document Intelligence

Formerly known as **Form Recognizer** — extracts structured data from documents.

### Pre-built Models (no training required)

| Model | Extracts From |
|-------|--------------|
| **Receipt** | Merchant name, items, totals, date |
| **Invoice** | Vendor, line items, totals, due date |
| **ID Document** | Name, DOB, document number (passports, driver's licenses) |
| **Business Card** | Name, company, phone, email |
| **W-2 / Tax** | Tax form fields |
| **Health Insurance Card** | Insurance details |

### Custom Models (train on your own documents)
- Upload sample documents with labeled fields
- Train to extract specific fields from your document type
- Useful for unique forms not covered by pre-built models

---

## 3.6 Azure Video Indexer

- Analyzes **video and audio content** to extract insights
- Extracts: faces, celebrities, keywords, topics, transcript, sentiment, OCR on screen
- Part of **Azure AI Video Indexer** service (integrated into Azure AI services)

---

> ⬅️ [← Domain 2](./domain-2-machine-learning.md) | [Back to Index](./index.md) | [Domain 4 →](./domain-4-nlp.md)
