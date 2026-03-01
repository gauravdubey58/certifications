# 🟤 Module 5: Implement Knowledge Mining and Document Intelligence Solutions (10–15%)

📘 [← Back to AI-102 Index](./index.md)

---

## 5.1 Implement an Azure AI Search Solution

### 🔍 Azure AI Search Overview

Azure AI Search (formerly Cognitive Search) is a cloud search platform with built-in AI enrichment. It indexes content from various data sources and makes it searchable via a rich query API.

**Core Components:**

| Component | Description |
|-----------|-------------|
| **Index** | The searchable repository — like a database table with searchable fields |
| **Indexer** | Automates data ingestion from a data source into the index |
| **Data Source** | Connection to the content — Azure Blob, SQL, Cosmos DB, Table Storage |
| **Skillset** | Chain of AI skills (OCR, NER, translation) applied during indexing |
| **Synonym Map** | Maps equivalent terms so `car` also matches `automobile` |

---

### 🗂️ Index Design

**Field Attributes:**

| Attribute | Description |
|-----------|-------------|
| `key` | Unique document identifier (required) |
| `searchable` | Full-text indexed; supports text queries |
| `filterable` | Used in `$filter` expressions |
| `sortable` | Used in `$orderby` |
| `facetable` | Used for faceted navigation (counts by category) |
| `retrievable` | Returned in search results |

**Data Types:** `Edm.String`, `Edm.Int32`, `Edm.Double`, `Edm.Boolean`, `Edm.DateTimeOffset`, `Collection(Edm.String)`

```json
{
  "name": "hotels-index",
  "fields": [
    { "name": "HotelId", "type": "Edm.String", "key": true, "retrievable": true },
    { "name": "HotelName", "type": "Edm.String", "searchable": true, "retrievable": true },
    { "name": "Rating", "type": "Edm.Double", "filterable": true, "sortable": true, "facetable": true },
    { "name": "Category", "type": "Edm.String", "filterable": true, "facetable": true },
    { "name": "Description", "type": "Edm.String", "searchable": true, "retrievable": true }
  ]
}
```

---

### 🔎 Search Query Types

| Query Type | Syntax | Use Case |
|------------|--------|----------|
| **Simple** | Default; `+term` required, `-term` excluded | Basic keyword search |
| **Full Lucene** | Wildcards, regex, fuzzy, proximity, boosting | Advanced search scenarios |
| **Semantic** | Re-ranks results using language models | Most relevant results first |
| **Vector** | Search by embedding similarity | Semantic / AI-powered search |
| **Hybrid** | Combines keyword + vector | Best of both worlds |

**Query Examples:**
```python
from azure.search.documents import SearchClient
from azure.core.credentials import AzureKeyCredential

client = SearchClient(endpoint, "hotels-index", AzureKeyCredential(query_key))

# Simple search
results = client.search("luxury hotel", top=5)

# Filter + sort + select fields
results = client.search(
    search_text="hotel",
    filter="Rating gt 4.0 and Category eq 'Resort'",
    order_by=["Rating desc"],
    select=["HotelName", "Rating", "Category"],
    top=10
)

# Facets
results = client.search(
    search_text="*",
    facets=["Category,count:5", "Rating,interval:1"]
)

# Semantic search
results = client.search(
    search_text="comfortable rooms with good service",
    query_type="semantic",
    semantic_configuration_name="my-semantic-config",
    query_caption="extractive",
    query_answer="extractive"
)

# Highlight results
results = client.search(
    search_text="beach view",
    highlight_fields="Description",
    highlight_pre_tag="<b>",
    highlight_post_tag="</b>"
)
```

---

### 🤖 AI Enrichment with Skillsets

**Skillsets** apply AI transformations during indexing — extracting insights from raw content.

**Built-in Cognitive Skills:**

| Skill | Description |
|-------|-------------|
| `OcrSkill` | Extracts text from images |
| `ImageAnalysisSkill` | Tags, captions, and objects from images |
| `EntityRecognitionSkill` | Extracts named entities (people, places, organisations) |
| `KeyPhraseExtractionSkill` | Extracts key phrases from text |
| `SentimentSkill` | Sentiment analysis |
| `LanguageDetectionSkill` | Detects language of text |
| `TranslationSkill` | Translates text |
| `SplitSkill` | Splits text into pages or sentences |
| `MergeSkill` | Combines text from multiple fields |
| `ShaperSkill` | Creates a complex type from multiple inputs |
| `ConditionalSkill` | Assigns a value based on a condition |
| `AzureOpenAIEmbeddingSkill` | Generates vector embeddings using Azure OpenAI |

**Knowledge Store:**
- Persists AI-enriched data to Azure Storage (Blob or Tables) for downstream use
- Defined within the skillset as `knowledgeStore` projections
- Enables Power BI analysis of enriched data

**Custom Skills:**
- Call a custom Azure Function or web API as part of the skillset pipeline
- Defined with `WebApiSkill`
- Used when built-in skills don't cover a specific extraction need

```json
{
  "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
  "name": "MyCustomSkill",
  "uri": "https://myfunctionapp.azurewebsites.net/api/CustomSkill",
  "httpMethod": "POST",
  "timeout": "PT30S",
  "inputs": [{ "name": "text", "source": "/document/content" }],
  "outputs": [{ "name": "customEntities", "targetName": "entities" }]
}
```

---

### 📐 Semantic Search and Vector Search

**Semantic Search:**
- Applies language model re-ranking on top of BM25 keyword results
- Generates captions (highlighted relevant passages) and answers
- Requires configuring a semantic configuration with `titleField`, `contentFields`, `keywordFields`
- Available on Standard tier and above

**Vector Search:**
- Documents and queries are represented as high-dimensional vectors (embeddings)
- Similarity is computed using cosine similarity, dot product, or Euclidean distance
- Enables "find similar" and semantic similarity queries
- Typically generated by Azure OpenAI's `text-embedding-ada-002` or `text-embedding-3-small`

**Hybrid Search (recommended):**
```python
results = client.search(
    search_text="quiet hotel near the beach",              # Keyword search
    vector_queries=[{
        "kind": "vector",
        "vector": get_embedding("quiet hotel near the beach"),  # Vector search
        "k_nearest_neighbors": 5,
        "fields": "DescriptionVector"
    }],
    query_type="semantic",
    semantic_configuration_name="my-config"
)
```

---

## 5.2 Implement Azure AI Document Intelligence

### 📄 Document Intelligence Overview

Azure AI Document Intelligence (formerly Form Recognizer) extracts structured data from forms, documents, and images using pre-built and custom models.

**Model Types:**

| Type | Description |
|------|-------------|
| **Prebuilt** | Ready-to-use models for common document types |
| **Custom** | Trained on your own document examples |
| **Composed** | Combines multiple custom models; routes to the best match |

---

### 📋 Prebuilt Models

| Model | Extracts |
|-------|---------|
| `prebuilt-read` | Text, language, handwriting from any document |
| `prebuilt-layout` | Text, tables, selection marks, structure (paragraphs, headings) |
| `prebuilt-invoice` | Vendor, invoice number, line items, totals, dates |
| `prebuilt-receipt` | Merchant, items, totals, date, payment type |
| `prebuilt-idDocument` | Passport, driver's licence — name, DOB, address, document number |
| `prebuilt-businessCard` | Contact name, phone, email, company, address |
| `prebuilt-tax.us.w2` | W-2 tax form fields |
| `prebuilt-healthInsuranceCard.us` | Insurance member and plan information |
| `prebuilt-contract` | Contract parties, jurisdictions, dates, signatures |

---

### 🏗️ Custom Models

**Custom Template Model:**
- Best for fixed-layout forms (invoices, purchase orders with consistent structure)
- Uses field coordinates from labelled training documents
- Minimum: **5 labelled training documents**

**Custom Neural Model:**
- Best for variable-layout forms and complex documents
- Uses deep learning — more accurate for diverse document types
- Minimum: **10 labelled training documents** (50+ recommended)

**Training Workflow:**
1. Upload training documents to Azure Blob Storage
2. Label documents in **Document Intelligence Studio** (or via SDK)
3. Train the model
4. Test accuracy with unseen documents
5. Deploy to an endpoint

```python
from azure.ai.formrecognizer import DocumentAnalysisClient

client = DocumentAnalysisClient(endpoint, AzureKeyCredential(key))

# Analyse with prebuilt invoice model
with open("invoice.pdf", "rb") as f:
    poller = client.begin_analyze_document("prebuilt-invoice", f)

result = poller.result()

for invoice in result.documents:
    print(f"Vendor: {invoice.fields.get('VendorName').value}")
    print(f"Invoice #: {invoice.fields.get('InvoiceId').value}")
    print(f"Total: {invoice.fields.get('InvoiceTotal').value}")

    # Line items
    items = invoice.fields.get("Items")
    if items:
        for item in items.value:
            print(f"  Item: {item.value.get('Description').value}")
            print(f"  Amount: {item.value.get('Amount').value}")
```

**Analysing Layout (Tables, Checkboxes):**
```python
poller = client.begin_analyze_document("prebuilt-layout", document_bytes)
result = poller.result()

for table in result.tables:
    print(f"Table: {table.row_count} rows x {table.column_count} cols")
    for cell in table.cells:
        print(f"  [{cell.row_index},{cell.column_index}] = {cell.content}")

for page in result.pages:
    for selection_mark in page.selection_marks:
        print(f"Checkbox: {selection_mark.state}")  # selected / unselected
```

---

### 🔗 Document Intelligence + AI Search Integration

A common exam pattern: combine Document Intelligence and AI Search for an intelligent document processing pipeline:

```
Documents in Blob Storage
       ↓
  AI Search Indexer
       ↓
  Skillset (OCR + NER + Document Intelligence custom skill)
       ↓
  AI Search Index (searchable fields)
       ↓
  Application / Power BI
```

---

📘 [← Back to AI-102 Index](./index.md)
