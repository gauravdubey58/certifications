# 🔵 Module 1: Design and Prepare a Machine Learning Solution (20–25%)

📘 [← Back to DP-100 Index](./index.md)

---

## 1.1 Design a Machine Learning Solution

### 📐 Identify Dataset Structure and Format

Before selecting tools, you need to understand your data:

| Data Format | Description | Azure ML Asset Type |
|-------------|-------------|---------------------|
| Tabular (CSV, Parquet, Delta) | Rows and columns — structured | `mltable` or `uri_file` / `uri_folder` |
| Image files | PNG, JPEG, TIFF for CV tasks | `uri_folder` with annotation files |
| Text files | Raw or preprocessed for NLP | `uri_folder` |
| Audio files | WAV, MP3 for speech tasks | `uri_folder` |
| Time series | Timestamped tabular data | `mltable` |

**MLTable:** A special data asset type that defines how to load tabular data — supporting row filtering, column selection, and schema inference at load time. Preferred for AutoML and structured training.

---

### 💻 Determine Compute Specifications

| Compute Type | Use Case | Notes |
|-------------|----------|-------|
| **Compute Instance** | Individual dev, notebooks, VS Code | Single VM; always-on or auto-shutdown |
| **Compute Cluster (AmlCompute)** | Training jobs, pipelines, batch scoring | Auto-scales 0→N nodes; idles to 0 |
| **Serverless Compute** | On-demand job execution | No pre-provisioned cluster needed |
| **Kubernetes (AKS/Arc)** | Online endpoint serving, production inference | Bring your own cluster |
| **Attached Compute (Synapse Spark)** | Big data wrangling, Spark jobs | Linked Synapse workspace |

**VM SKU Selection Guide:**

| Workload | Recommended SKU |
|----------|----------------|
| General ML training (CPU) | `Standard_DS3_v2`, `Standard_D4s_v3` |
| Deep Learning / GPU training | `Standard_NC6s_v3`, `Standard_ND40rs_v2` |
| Lightweight notebooks | `Standard_DS1_v2` |
| Large memory (feature engineering) | `Standard_E16s_v3` |

---

### 🏗️ Select the Development Approach

| Approach | When to Use |
|----------|-------------|
| **Azure ML Studio (UI)** | Quick experimentation, AutoML, drag-and-drop designer |
| **Python SDK v2** | Full programmatic control, CI/CD integration |
| **Azure CLI (ml extension)** | DevOps/MLOps pipelines, scripted deployments |
| **REST API** | Cross-language, integration scenarios |
| **VS Code + AML extension** | Local development with remote compute |

---

## 1.2 Create and Manage Resources in an Azure ML Workspace

### 🏢 Azure ML Workspace

The workspace is the top-level resource for all Azure ML activities — it ties together compute, data, experiments, models, and deployments.

**Associated Resources (created automatically or linked):**

| Resource | Purpose |
|---------|---------|
| Azure Storage Account | Default datastore; stores artifacts, logs, datasets |
| Azure Key Vault | Secrets and connection strings for datastores |
| Azure Container Registry | Stores Docker images for environments |
| Application Insights | Logs and metrics for deployed endpoints |

**Create via CLI:**
```bash
az ml workspace create \
  --name myMLWorkspace \
  --resource-group myRG \
  --location eastus
```

**Create via SDK v2:**
```python
from azure.ai.ml import MLClient
from azure.ai.ml.entities import Workspace
from azure.identity import DefaultAzureCredential

ml_client = MLClient(DefaultAzureCredential(), subscription_id, resource_group)

ws = Workspace(
    name="myMLWorkspace",
    location="eastus",
    description="DP-100 study workspace"
)
ml_client.workspaces.begin_create(ws).result()
```

---

### 🗄️ Create and Manage Datastores

Datastores are registered connections to Azure storage services — they abstract connection details so data references stay consistent even if credentials change.

**Built-in Datastores:**

| Datastore | Backed By | Default? |
|-----------|-----------|---------|
| `workspaceblobstore` | Azure Blob Storage | ✅ Yes |
| `workspacefilestore` | Azure File Share | Yes |
| `workspaceartifactstore` | Azure Blob (internal) | Yes |

**Registering a Custom Datastore:**
```python
from azure.ai.ml.entities import AzureBlobDatastore, AccountKeyConfiguration

datastore = AzureBlobDatastore(
    name="my_blob_store",
    description="Training data in Azure Blob",
    account_name="mystorageaccount",
    container_name="trainingdata",
    credentials=AccountKeyConfiguration(
        account_key="<storage-account-key>"
    )
)
ml_client.datastores.create_or_update(datastore)
```

**Supported Datastore Types:**
- Azure Blob Storage
- Azure Data Lake Storage Gen1 and Gen2
- Azure File Share
- Azure SQL Database
- Azure Databricks File System (DBFS)

---

### ⚙️ Create and Manage Compute Targets

**Compute Instance (for development):**
```python
from azure.ai.ml.entities import ComputeInstance

compute = ComputeInstance(
    name="myDevVM",
    size="Standard_DS3_v2",
    idle_time_before_shutdown_minutes=30
)
ml_client.compute.begin_create_or_update(compute).result()
```

**Compute Cluster (for training):**
```python
from azure.ai.ml.entities import AmlCompute

cluster = AmlCompute(
    name="myTrainingCluster",
    type="amlcompute",
    size="Standard_DS3_v2",
    min_instances=0,      # Scale to zero when idle
    max_instances=4,
    idle_time_before_scale_down=120
)
ml_client.compute.begin_create_or_update(cluster).result()
```

> 💡 Always set `min_instances=0` to avoid charges when the cluster is idle. The cluster will spin up when a job is submitted and scale back to 0 afterwards.

---

### 🔗 Git Integration for Source Control

Azure ML supports connecting notebooks and scripts to a Git repository for version-controlled development.

**Setup on Compute Instance:**
```bash
# In the terminal of the compute instance
git config --global user.name "Gaurav Dubey"
git config --global user.email "gaurav@example.com"

# Clone a repo
git clone https://github.com/myorg/myrepo.git

# Or set up inside the workspace via the Studio UI:
# Settings → Git repositories → Add repository
```

**Benefits:**
- Track changes to training scripts and notebooks
- Trigger pipelines on code commits (CI/CD)
- Collaborate across team members

---

## 1.3 Create and Manage Assets in an Azure ML Workspace

### 📦 Data Assets

Data assets are versioned references to data — they can point to files, folders, or MLTable definitions in a datastore.

**Asset Types:**

| Type | Description | Best For |
|------|-------------|---------|
| `uri_file` | Single file reference | Individual CSV, Parquet, JSON |
| `uri_folder` | Folder reference | Image datasets, multiple files |
| `mltable` | Tabular data with schema + transforms | AutoML, structured training |

```python
from azure.ai.ml.entities import Data
from azure.ai.ml.constants import AssetTypes

# Register a folder as a data asset
data_asset = Data(
    name="training-images",
    version="1",
    description="Labelled product images for classification",
    path="azureml://datastores/my_blob_store/paths/images/",
    type=AssetTypes.URI_FOLDER
)
ml_client.data.create_or_update(data_asset)

# Reference in a job
from azure.ai.ml import Input
data_input = Input(type=AssetTypes.URI_FOLDER,
                   path="azureml:training-images:1")
```

---

### 🐍 Environments

Environments define the Docker image + Python packages used for running jobs. Versioned and reusable.

**Types:**

| Type | Description |
|------|-------------|
| **Curated** | Microsoft-managed, optimised images (e.g., `AzureML-sklearn-1.0-ubuntu20.04-py38`) |
| **Custom** | Built from a conda YAML, pip requirements, or custom Dockerfile |

```python
from azure.ai.ml.entities import Environment

# From conda YAML
env = Environment(
    name="sklearn-training-env",
    description="Scikit-learn training environment",
    conda_file="conda.yml",
    image="mcr.microsoft.com/azureml/openmpi4.1.0-ubuntu20.04"
)
ml_client.environments.create_or_update(env)
```

**conda.yml:**
```yaml
name: sklearn-env
channels:
  - conda-forge
  - defaults
dependencies:
  - python=3.10
  - scikit-learn=1.3.0
  - pandas=2.0.3
  - pip:
    - mlflow
    - azureml-mlflow
```

---

### 🌐 Registries — Sharing Assets Across Workspaces

Registries are organisation-level repositories for sharing environments, models, data assets, and components across multiple workspaces.

**Use cases:**
- Share a curated training environment across dev, test, and prod workspaces
- Promote a trained model from a development workspace to a production workspace
- Centralise reusable pipeline components for all teams

```python
# Create a registry client
registry_client = MLClient(
    DefaultAzureCredential(),
    registry_name="myOrgRegistry"
)

# Share an environment to the registry
registry_client.environments.create_or_update(env)

# Consume from another workspace
shared_env = Input(
    type="mlflow_model",
    path="azureml://registries/myOrgRegistry/environments/sklearn-training-env/versions/1"
)
```

---

📘 [← Back to DP-100 Index](./index.md)
