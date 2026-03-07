# 🟠 Module 3: Train and Deploy Models (25–30%)

📘 [← Back to DP-100 Index](./index.md)

---

## 3.1 Run Model Training Scripts as Jobs

### 🚀 Command Jobs

A Command Job runs a script on a specified compute target with a defined environment, data inputs, and parameters.

**Job Components:**

| Component | Description |
|-----------|-------------|
| `code` | Path to the script directory |
| `command` | Shell command to execute |
| `environment` | Docker image + Python packages |
| `compute` | Target compute (cluster name or `serverless`) |
| `inputs` | Data and parameter inputs |
| `outputs` | Data outputs (model artifacts, processed data) |

```python
from azure.ai.ml import command, Input, Output
from azure.ai.ml.constants import AssetTypes, InputOutputModes

job = command(
    code="./src",
    command="python train.py \
        --training_data ${{inputs.training_data}} \
        --learning_rate ${{inputs.learning_rate}} \
        --model_output ${{outputs.model_output}}",
    environment="azureml:sklearn-training-env:1",
    compute="myTrainingCluster",
    experiment_name="diabetes-classification",
    inputs={
        "training_data": Input(
            type=AssetTypes.MLTABLE,
            path="azureml:diabetes-training:1",
            mode=InputOutputModes.RO_MOUNT   # Read-only mount
        ),
        "learning_rate": Input(type="number", default=0.01)
    },
    outputs={
        "model_output": Output(
            type=AssetTypes.MLFLOW_MODEL,
            mode=InputOutputModes.RW_MOUNT
        )
    }
)

returned_job = ml_client.jobs.create_or_update(job)
ml_client.jobs.stream(returned_job.name)
```

**Input/Output Modes:**

| Mode | Description | Use Case |
|------|-------------|---------|
| `ro_mount` | Read-only mount | Training data input |
| `rw_mount` | Read-write mount | Writing output during job |
| `download` | Download to local disk before job | Small files |
| `upload` | Upload from local disk after job | Small outputs |
| `direct` | Pass URI directly (no copy) | Large files, pass-through |

---

### 📝 Training Script (train.py)

```python
import argparse
import mlflow
import mlflow.sklearn
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
import mltable

def main():
    # Parse arguments from job command
    parser = argparse.ArgumentParser()
    parser.add_argument("--training_data", type=str)
    parser.add_argument("--learning_rate", type=float, default=0.01)
    parser.add_argument("--model_output", type=str)
    args = parser.parse_args()

    # Load data
    tbl = mltable.load(args.training_data)
    df = tbl.to_pandas_dataframe()
    X = df.drop("Diabetic", axis=1)
    y = df["Diabetic"]
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

    # MLflow autolog
    mlflow.sklearn.autolog()

    with mlflow.start_run():
        model = LogisticRegression(C=args.learning_rate, max_iter=1000)
        model.fit(X_train, y_train)
        accuracy = accuracy_score(y_test, model.predict(X_test))
        mlflow.log_metric("accuracy", accuracy)

        # Save model to output path
        mlflow.sklearn.save_model(model, args.model_output)

if __name__ == "__main__":
    main()
```

---

### 🐛 Troubleshoot Job Runs with Logs

**Log Locations in Azure ML:**

| Log | Location in Job Output | Content |
|-----|----------------------|---------|
| `user_logs/std_log.txt` | User outputs tab | `print()` statements, script output |
| `system_logs/` | System tab | AML infrastructure logs |
| `70_driver_log.txt` | Logs tab | Main training driver log |
| `75_job_post_proc*.txt` | Logs tab | Artifact upload logs |

```python
# Download logs programmatically
ml_client.jobs.download(
    name=returned_job.name,
    download_path="./job_logs",
    all=True
)
```

**Common Errors:**

| Error | Likely Cause | Fix |
|-------|-------------|-----|
| `ModuleNotFoundError` | Package not in environment | Add to conda.yml / pip requirements |
| `FileNotFoundError` on input | Data path wrong or not mounted | Check `path` in Input definition |
| `OutOfMemory` | VM too small | Upgrade SKU or use distributed training |
| Job stuck at `Preparing` | Environment image building | Wait; check ACR push status |

---

## 3.2 Implement Training Pipelines

### 🔗 Pipeline Overview

Pipelines chain multiple steps (components) into a repeatable, schedulable workflow. Each step runs independently — enabling parallelism, step reuse, and modular development.

**Benefits:**
- Reuse expensive steps (e.g., skip data prep if data hasn't changed)
- Run steps in parallel when there are no dependencies
- Schedule recurring retraining workflows
- Share components across teams via registries

---

### 🧩 Custom Components

Components are reusable, self-contained steps with defined inputs, outputs, and environments — described by a YAML spec.

**Component YAML (prep_data.yml):**
```yaml
name: prep_data
display_name: Prepare Training Data
version: "1"
type: command
inputs:
  raw_data:
    type: uri_folder
  test_split_ratio:
    type: number
    default: 0.2
outputs:
  train_data:
    type: mltable
  test_data:
    type: mltable
code: ./prep_data_src
environment: azureml:sklearn-training-env:1
command: python prep_data.py
  --raw_data ${{inputs.raw_data}}
  --test_split_ratio ${{inputs.test_split_ratio}}
  --train_data ${{outputs.train_data}}
  --test_data ${{outputs.test_data}}
```

**Register and use a component:**
```python
from azure.ai.ml import load_component

# Load from YAML
prep_component = load_component(source="./components/prep_data.yml")
ml_client.components.create_or_update(prep_component)

# Or create inline
from azure.ai.ml import command

prep_component = command(
    name="prep_data",
    display_name="Prepare Data",
    inputs={"raw_data": Input(type="uri_folder")},
    outputs={"train_data": Output(type="mltable")},
    code="./prep_data_src",
    command="python prep_data.py --raw_data ${{inputs.raw_data}} --train_data ${{outputs.train_data}}",
    environment="azureml:sklearn-training-env:1"
)
```

---

### 🏗️ Building and Running a Pipeline

```python
from azure.ai.ml.dsl import pipeline
from azure.ai.ml import Input

# Load components
prep_component = ml_client.components.get("prep_data", version="1")
train_component = ml_client.components.get("train_model", version="1")
eval_component = ml_client.components.get("evaluate_model", version="1")

@pipeline(
    name="diabetes-training-pipeline",
    description="End-to-end diabetes classification pipeline",
    compute="myTrainingCluster"
)
def diabetes_pipeline(raw_data: Input(type="uri_folder")):
    # Step 1: Prepare data
    prep_step = prep_component(
        raw_data=raw_data,
        test_split_ratio=0.2
    )

    # Step 2: Train model (receives output of Step 1)
    train_step = train_component(
        train_data=prep_step.outputs.train_data,
        learning_rate=0.01,
        n_estimators=100
    )

    # Step 3: Evaluate (receives test data + trained model)
    eval_step = eval_component(
        test_data=prep_step.outputs.test_data,
        model=train_step.outputs.model_output
    )

    return {
        "trained_model": train_step.outputs.model_output,
        "evaluation_report": eval_step.outputs.report
    }

# Create and run the pipeline job
pipeline_job = diabetes_pipeline(
    raw_data=Input(type="uri_folder", path="azureml:raw-diabetes-data:1")
)
returned_pipeline = ml_client.jobs.create_or_update(pipeline_job)
ml_client.jobs.stream(returned_pipeline.name)
```

---

### 📅 Scheduling a Pipeline

```python
from azure.ai.ml.entities import RecurrenceTrigger, JobSchedule

# Recurrence trigger — run every Monday at 08:00 UTC
schedule = JobSchedule(
    name="weekly-retraining",
    trigger=RecurrenceTrigger(
        frequency="week",
        interval=1,
        schedule={"week_days": ["Monday"], "hours": [8], "minutes": [0]}
    ),
    create_job=pipeline_job
)
ml_client.schedules.begin_create_or_update(schedule).result()
```

---

## 3.3 Manage Models

### 📝 MLflow Model Signature

The model **signature** defines the schema of the model's inputs and outputs — enabling validation and deployment contract enforcement.

```python
from mlflow.models.signature import infer_signature

# Infer signature from training data
signature = infer_signature(
    model_input=X_train,
    model_output=model.predict(X_train)
)

mlflow.sklearn.log_model(
    sk_model=model,
    artifact_path="model",
    signature=signature,
    input_example=X_train[:5]  # Sample input for documentation
)
```

**Manual signature definition:**
```python
from mlflow.types.schema import Schema, ColSpec
from mlflow.models.signature import ModelSignature

input_schema = Schema([
    ColSpec("double", "Pregnancies"),
    ColSpec("double", "Glucose"),
    ColSpec("double", "BloodPressure")
])
output_schema = Schema([ColSpec("long", "Diabetic")])
signature = ModelSignature(inputs=input_schema, outputs=output_schema)
```

---

### 📦 Registering an MLflow Model

```python
# Register from a completed job's outputs
ml_client.models.create_or_update(
    Model(
        path=f"azureml://jobs/{returned_job.name}/outputs/model_output",
        name="diabetes-classifier",
        description="Logistic regression for diabetes classification",
        type=AssetTypes.MLFLOW_MODEL,
        tags={"framework": "sklearn", "task": "classification"}
    )
)

# Or register directly from MLflow run
import mlflow
from mlflow.tracking import MlflowClient

client = MlflowClient()
result = client.register_model(
    model_uri=f"runs:/{run_id}/model",
    name="diabetes-classifier"
)
```

**Model Versions and Stages:**

| Stage | Description |
|-------|-------------|
| `None` | Newly registered, not yet promoted |
| `Staging` | Under validation / testing |
| `Production` | Live production model |
| `Archived` | Retired, kept for audit |

---

### ⚖️ Responsible AI Model Assessment

```python
from azure.ai.ml.entities import (
    ModelPackage,
    AzureMLOnlineInferencingServer
)

# Generate RAI insights dashboard for a registered model
from azure.ai.ml import rai_insights

rai_job = rai_insights(
    title="Diabetes Classifier RAI",
    target_column_name="Diabetic",
    task_type="classification",
    train_dataset=Input(type=AssetTypes.MLTABLE, path="azureml:diabetes-train:1"),
    test_dataset=Input(type=AssetTypes.MLTABLE, path="azureml:diabetes-test:1"),
    model_info="diabetes-classifier:1"
)
```

---

## 3.4 Deploy a Model

### 🟢 Online Endpoints (Real-time Inference)

Online endpoints serve predictions synchronously — low latency, one request at a time.

**Endpoint Types:**

| Type | Infrastructure | Best For |
|------|---------------|---------|
| **Managed Online** | Azure-managed, serverless | Default; simplest setup |
| **Kubernetes Online** | Your AKS/Arc cluster | Custom networking, compliance |

```python
from azure.ai.ml.entities import (
    ManagedOnlineEndpoint,
    ManagedOnlineDeployment,
    Model,
    CodeConfiguration
)

# 1. Create endpoint
endpoint = ManagedOnlineEndpoint(
    name="diabetes-endpoint",
    description="Real-time diabetes classification",
    auth_mode="key"   # key or aml_token
)
ml_client.online_endpoints.begin_create_or_update(endpoint).result()

# 2. Create deployment
blue_deployment = ManagedOnlineDeployment(
    name="blue",
    endpoint_name="diabetes-endpoint",
    model="azureml:diabetes-classifier:1",
    instance_type="Standard_DS3_v2",
    instance_count=1,
    liveness_probe=ProbeSettings(initial_delay=10, period=10),
    readiness_probe=ProbeSettings(initial_delay=30, period=10)
)
ml_client.online_deployments.begin_create_or_update(blue_deployment).result()

# 3. Route 100% traffic to blue
endpoint.traffic = {"blue": 100}
ml_client.online_endpoints.begin_create_or_update(endpoint).result()
```

**Testing the endpoint:**
```python
import json

response = ml_client.online_endpoints.invoke(
    endpoint_name="diabetes-endpoint",
    request_file="./sample_request.json",
    deployment_name="blue"
)
print(response)
```

**Blue-Green Deployment (safe rollout):**
```python
# Deploy new version as "green"
green_deployment = ManagedOnlineDeployment(
    name="green",
    endpoint_name="diabetes-endpoint",
    model="azureml:diabetes-classifier:2",
    instance_type="Standard_DS3_v2",
    instance_count=1
)
ml_client.online_deployments.begin_create_or_update(green_deployment).result()

# Gradually shift traffic
endpoint.traffic = {"blue": 80, "green": 20}
ml_client.online_endpoints.begin_create_or_update(endpoint).result()

# After validation — full traffic to green
endpoint.traffic = {"blue": 0, "green": 100}
ml_client.online_endpoints.begin_create_or_update(endpoint).result()
```

---

### 📦 Batch Endpoints (Asynchronous Scoring)

Batch endpoints score large datasets asynchronously — ideal for non-real-time, high-volume prediction tasks.

```python
from azure.ai.ml.entities import (
    BatchEndpoint,
    ModelBatchDeployment,
    ModelBatchDeploymentSettings,
    BatchRetrySettings
)

# 1. Create batch endpoint
batch_endpoint = BatchEndpoint(
    name="diabetes-batch-endpoint",
    description="Batch scoring for diabetes classifier"
)
ml_client.batch_endpoints.begin_create_or_update(batch_endpoint).result()

# 2. Create batch deployment
batch_deployment = ModelBatchDeployment(
    name="default",
    endpoint_name="diabetes-batch-endpoint",
    model="azureml:diabetes-classifier:1",
    compute="myTrainingCluster",
    settings=ModelBatchDeploymentSettings(
        instance_count=2,
        max_concurrency_per_instance=2,
        mini_batch_size=100,      # Items processed per mini-batch
        output_action="append_row",
        output_file_name="predictions.csv",
        retry_settings=BatchRetrySettings(max_retries=3, timeout=300),
        error_threshold=10,       # Max errors before job fails
        logging_level="info"
    )
)
ml_client.batch_deployments.begin_create_or_update(batch_deployment).result()

# 3. Invoke batch scoring job
job = ml_client.batch_endpoints.invoke(
    endpoint_name="diabetes-batch-endpoint",
    input=Input(type=AssetTypes.URI_FOLDER,
                path="azureml:new-patient-data:1")
)
ml_client.jobs.stream(job.name)
```

**Online vs Batch Endpoint Comparison:**

| | Online Endpoint | Batch Endpoint |
|--|----------------|----------------|
| Response | Synchronous (ms) | Asynchronous (minutes–hours) |
| Trigger | HTTP request | Job invocation |
| Input | Single record/small batch | Large dataset |
| Infrastructure | Always-on instances | Cluster spun up per job |
| Cost | Per-second billing | Per-job compute billing |
| Use case | Real-time APIs, chatbots | Nightly scoring, bulk predictions |

---

📘 [← Back to DP-100 Index](./index.md)
