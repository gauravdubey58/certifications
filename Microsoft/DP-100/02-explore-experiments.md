# 🟢 Module 2: Explore Data and Run Experiments (20–25%)

📘 [← Back to DP-100 Index](./index.md)

---

## 2.1 Use Automated Machine Learning to Explore Optimal Models

### 🤖 AutoML Overview

Automated ML (AutoML) in Azure ML automatically tries many algorithm and preprocessing combinations, selects the best-performing model, and explains results — without requiring deep ML expertise.

**Supported Task Types:**

| Task | Type | Output |
|------|------|--------|
| Classification | Tabular | Class label + probability |
| Regression | Tabular | Continuous value |
| Time Series Forecasting | Tabular | Future values |
| Image Classification | Computer Vision | Class label |
| Image Classification (Multilabel) | CV | Multiple labels |
| Object Detection | CV | Bounding boxes + labels |
| Instance Segmentation | CV | Pixel masks + labels |
| Text Classification | NLP | Category label |
| Text Classification (Multilabel) | NLP | Multiple labels |
| Named Entity Recognition | NLP | Entity spans |

---

### 📊 AutoML for Tabular Data

```python
from azure.ai.ml import automl, Input
from azure.ai.ml.constants import AssetTypes

# Define training data input
training_data = Input(
    type=AssetTypes.MLTABLE,
    path="azureml:diabetes-training:1"
)

# Configure AutoML classification job
automl_job = automl.classification(
    compute="myTrainingCluster",
    experiment_name="diabetes-automl",
    training_data=training_data,
    target_column_name="Diabetic",
    primary_metric="accuracy",             # Metric to optimise
    n_cross_validations=5,
    enable_model_explainability=True,
    allowed_training_algorithms=["LightGBM", "XGBoostClassifier"],
    timeout_minutes=60,                    # Total job timeout
    trial_timeout_minutes=10              # Per-trial timeout
)

returned_job = ml_client.jobs.create_or_update(automl_job)
ml_client.jobs.stream(returned_job.name)
```

**Primary Metrics by Task:**

| Task | Common Primary Metrics |
|------|----------------------|
| Classification | `accuracy`, `AUC_weighted`, `norm_macro_recall` |
| Regression | `r2_score`, `normalized_root_mean_squared_error` |
| Forecasting | `normalized_root_mean_squared_error` |

**Preprocessing AutoML applies automatically:**
- Missing value imputation
- Categorical encoding (one-hot, label encoding)
- Feature scaling and normalisation
- DateTime feature generation
- Text featurisation (TF-IDF, word embeddings)

---

### 🏃 Evaluating AutoML Results

**Accessing the Best Run:**
```python
best_run = ml_client.jobs.get(returned_job.name)

# Download best model artifacts
ml_client.jobs.download(
    name=best_run.name,
    download_path="./automl_outputs",
    all=True
)
```

**Responsible AI in AutoML:**
- **Model Explainability:** Feature importance computed automatically when `enable_model_explainability=True`
- **Fairness Assessment:** Evaluate disparate impact across demographic groups
- **Data Guardrails:** AutoML warns about class imbalance, high cardinality, feature leakage

---

### 🖥️ AutoML for Computer Vision

```python
automl_vision_job = automl.image_classification(
    compute="gpu-cluster",
    experiment_name="product-image-classifier",
    training_data=Input(type=AssetTypes.MLTABLE, path="azureml:product-images:1"),
    target_column_name="label",
    primary_metric="accuracy",
    model_name="vitb16r224"   # Vision Transformer base 16
)
```

**Supported CV Models:** ViT (Vision Transformer), ResNet, EfficientNet, YOLOv5 (for object detection).

---

## 2.2 Use Notebooks for Custom Model Training

### 📓 Compute Instance Terminal

The terminal on a Compute Instance provides full Linux shell access for environment setup, Git operations, and package installation.

```bash
# Install a package in the active Jupyter kernel
conda install -n azureml_py310_sdkv2 lightgbm -y

# Or via pip
pip install shap --quiet

# Check available conda environments
conda env list

# Switch kernel in terminal
conda activate azureml_py310_sdkv2
```

---

### 📊 Data Access and Wrangling in Notebooks

**Reading from a Datastore:**
```python
from azure.ai.ml import MLClient
from azure.identity import DefaultAzureCredential

ml_client = MLClient.from_config(credential=DefaultAzureCredential())

# Get a registered data asset
data_asset = ml_client.data.get("diabetes-training", version="1")

# Read with pandas
import pandas as pd
import mltable

tbl = mltable.load(data_asset.path)
df = tbl.to_pandas_dataframe()
print(df.head())
```

**Direct Blob Access via Datastore:**
```python
from azureml.fsspec import AzureMachineLearningFileSystem

fs = AzureMachineLearningFileSystem(
    "azureml://subscriptions/<sub>/resourcegroups/<rg>/workspaces/<ws>/datastores/workspaceblobstore/paths/data/"
)
with fs.open("train.csv") as f:
    df = pd.read_csv(f)
```

---

### ⚡ Synapse Spark for Big Data Wrangling

Azure ML integrates with **Azure Synapse Analytics** and **Serverless Spark Compute** for distributed data processing.

**Attaching a Synapse Spark Pool:**
```python
from azure.ai.ml.entities import SynapseSparkCompute

synapse_compute = SynapseSparkCompute(
    name="mySynapsePool",
    resource_id="/subscriptions/.../workspaces/mySynapseWorkspace/bigDataPools/myPool"
)
ml_client.compute.begin_create_or_update(synapse_compute).result()
```

**Running Spark in a Notebook (interactive):**
```python
%%synapse
# This magic routes code to the attached Spark pool
import pyspark.pandas as ps
df = ps.read_parquet("abfss://container@account.dfs.core.windows.net/data/")
df.describe()
```

**Serverless Spark Job:**
```python
from azure.ai.ml import spark

spark_job = spark(
    code="./spark_scripts",
    entry={"file": "process_data.py"},
    compute="serverless",
    resources={"instance_type": "Standard_E4s_v3", "runtime_version": "3.3"},
    inputs={"input_data": Input(type="uri_folder", path="azureml:raw-data:1")},
    outputs={"output_data": Output(type="uri_folder")}
)
ml_client.jobs.create_or_update(spark_job)
```

---

### 🏪 Feature Store

Azure ML Feature Store provides a centralised repository of reusable, versioned features — ensuring consistency between training and inference.

**Core Concepts:**

| Concept | Description |
|---------|-------------|
| **Feature Set** | A named group of related features computed from a source entity |
| **Feature Store Entity** | The key used to join features (e.g., `customer_id`, `product_id`) |
| **Materialization** | Pre-computing and caching feature values for faster retrieval |
| **Point-in-time joins** | Retrieve feature values as they were at a specific timestamp (prevents leakage) |

```python
# Retrieve features for model training
from azureml.featurestore import FeatureStoreClient
from azureml.featurestore.feature_source import FeatureSource

fs_client = FeatureStoreClient(
    credential=DefaultAzureCredential(),
    subscription_id=subscription_id,
    resource_group_name=resource_group,
    name="myFeatureStore"
)

# Get feature set spec
customer_features = fs_client.feature_sets.get("customer-features", version="1")

# Retrieve features for training
training_df = fs_client.get_offline_features(
    features=[
        customer_features.get_feature("avg_spend_90d"),
        customer_features.get_feature("num_transactions_30d")
    ],
    observation_data=observations_df,
    timestamp_column="timestamp"
)
```

---

## 2.3 Track Model Training with MLflow

### 📈 MLflow Tracking

MLflow is the open-source ML lifecycle tool integrated natively into Azure ML for experiment tracking, logging, and model management.

**Key Concepts:**

| Concept | Description |
|---------|-------------|
| **Experiment** | A named collection of runs |
| **Run** | A single training execution with logged metrics, parameters, and artifacts |
| **Parameters** | Hypervalues logged once per run (`mlflow.log_param`) |
| **Metrics** | Time-series values logged during training (`mlflow.log_metric`) |
| **Artifacts** | Files saved with a run (plots, models, data samples) |
| **Tags** | Key-value metadata on runs |

**MLflow Autologging (recommended):**
```python
import mlflow
import mlflow.sklearn
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split

# Enable autologging — captures params, metrics, and model automatically
mlflow.sklearn.autolog()

mlflow.set_experiment("diabetes-classification")

with mlflow.start_run(run_name="logistic-regression-v1"):
    model = LogisticRegression(C=1.0, max_iter=100)
    model.fit(X_train, y_train)
    # Autolog captures: parameters (C, max_iter), metrics (accuracy, F1), and the model
```

**Manual Logging:**
```python
with mlflow.start_run() as run:
    # Log parameters
    mlflow.log_param("learning_rate", 0.01)
    mlflow.log_param("n_estimators", 100)

    # Train model
    model.fit(X_train, y_train)
    accuracy = model.score(X_test, y_test)

    # Log metrics
    mlflow.log_metric("accuracy", accuracy)
    mlflow.log_metric("val_loss", val_loss)

    # Log per-epoch metrics
    for epoch, loss in enumerate(training_losses):
        mlflow.log_metric("train_loss", loss, step=epoch)

    # Log artifacts
    mlflow.log_artifact("confusion_matrix.png")
    mlflow.log_figure(fig, "roc_curve.png")

    # Log the model
    mlflow.sklearn.log_model(model, "model")

print(f"Run ID: {run.info.run_id}")
```

**Supported Autolog Frameworks:**
`sklearn`, `pytorch`, `tensorflow/keras`, `xgboost`, `lightgbm`, `statsmodels`, `spark`

---

### 🔍 Evaluate Model with Responsible AI

The **Responsible AI Dashboard** in Azure ML provides:

| Component | Description |
|-----------|-------------|
| **Model Overview** | Accuracy, precision, recall, F1 across data cohorts |
| **Error Analysis** | Decision tree + heatmap showing where the model fails |
| **Data Analysis** | Distribution statistics, class imbalance checks |
| **Feature Importances** | Global and local SHAP explanations |
| **Counterfactual Analysis** | "What would need to change for a different prediction?" |
| **Causal Analysis** | Understand true cause-and-effect relationships |

```python
from raiutils.dashboard import ResponsibleAIDashboard
from responsibleai import RAIInsights

rai_insights = RAIInsights(
    model=model,
    train=train_df,
    test=test_df,
    target_column="Diabetic",
    task_type="classification"
)

# Add components
rai_insights.explainer.add()
rai_insights.error_analysis.add()
rai_insights.fairlearn.add(
    sensitive_features=test_df[["Age", "Gender"]],
    sensitive_feature_names=["Age", "Gender"]
)

rai_insights.compute()
```

---

## 2.4 Automate Hyperparameter Tuning (Sweep Jobs)

### 🎯 Hyperparameter Tuning with Sweep Jobs

Azure ML Sweep Jobs automate hyperparameter search by running multiple trials across a defined search space.

### 📐 Sampling Methods

| Method | Description | When to Use |
|--------|-------------|-------------|
| **Grid** | Exhaustive — tries every combination | Small discrete search spaces |
| **Random** | Random samples from the space | General purpose; good default |
| **Bayesian** | Uses prior results to guide next trial | When each trial is expensive |
| **Sobol** | Quasi-random — better coverage than pure random | Large, continuous spaces |

### 🔢 Search Space Parameter Types

| Type | Syntax | Description |
|------|--------|-------------|
| `choice` | `choice([0.01, 0.1, 1.0])` | Discrete values |
| `uniform` | `uniform(0.01, 0.1)` | Continuous uniform range |
| `loguniform` | `loguniform(-3, 0)` | Log-scale uniform (good for LR) |
| `randint` | `randint(50, 200)` | Random integer in range |
| `quniform` | `quniform(1, 10, 1)` | Quantised uniform |

### 🛑 Early Termination Policies

| Policy | Description | Key Parameters |
|--------|-------------|---------------|
| **Bandit** | Stops runs that fall below top-K% | `slack_factor`, `evaluation_interval` |
| **Median Stopping** | Stops runs below median of completed runs | `evaluation_interval` |
| **Truncation Selection** | Stops bottom X% of runs each interval | `truncation_percentage` |
| **No termination** | All runs complete | Default |

```python
from azure.ai.ml.sweep import Choice, Uniform, BanditPolicy
from azure.ai.ml import command

# Base command job with hyperparameters as arguments
command_job = command(
    code="./training",
    command="python train.py --learning_rate ${{inputs.learning_rate}} --n_estimators ${{inputs.n_estimators}}",
    environment="azureml:sklearn-training-env:1",
    compute="myTrainingCluster",
    inputs={
        "training_data": Input(type=AssetTypes.MLTABLE, path="azureml:diabetes-training:1")
    }
)

# Define sweep
sweep_job = command_job.sweep(
    sampling_algorithm="bayesian",
    primary_metric="accuracy",
    goal="maximize",
    search_space={
        "learning_rate": Uniform(min_value=0.001, max_value=0.1),
        "n_estimators": Choice(values=[50, 100, 200, 500])
    },
    max_total_trials=20,
    max_concurrent_trials=4,
    early_termination_policy=BanditPolicy(
        slack_factor=0.1,
        evaluation_interval=1,
        delay_evaluation=5
    )
)

returned_sweep = ml_client.jobs.create_or_update(sweep_job)
ml_client.jobs.stream(returned_sweep.name)
```

---

📘 [← Back to DP-100 Index](./index.md)
