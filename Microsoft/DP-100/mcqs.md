# 📝 DP-100: Designing and Implementing a Data Science Solution on Azure — 50 Practice MCQs

> Mapped to official exam domains (updated April 2025):
> - **Domain 1:** Design and Prepare a Machine Learning Solution — 20–25%
> - **Domain 2:** Explore Data and Run Experiments — 20–25%
> - **Domain 3:** Train and Deploy Models — 25–30%
> - **Domain 4:** Optimize Language Models for AI Applications — 25–30%

📘 [← Back to DP-100 Index](./index.md)

---

## 🔵 Domain 1: Design and Prepare a Machine Learning Solution (Q1–Q12)

---

**Q1.** A data science team needs individual development environments with Jupyter notebooks, but also requires a shared cluster for running large training jobs. Which two Azure ML compute types should they provision?

- A) Two Compute Clusters — one for notebooks, one for training
- B) A Compute Instance for each data scientist and a Compute Cluster for training jobs
- C) A single large Compute Instance shared by all team members
- D) A Kubernetes cluster for both notebooks and training

✅ **Answer: B**  
*A Compute Instance is a single-user, always-available VM optimised for notebook-based development. A Compute Cluster (AmlCompute) auto-scales for training jobs and idles to zero. These two types serve complementary purposes.*

---

**Q2.** A training cluster is consistently charging costs even when no jobs are running. What setting should be configured to eliminate idle compute costs?

- A) Set `max_instances` to 0
- B) Set `min_instances` to 0 so the cluster scales down to zero when idle
- C) Delete and recreate the cluster before each job
- D) Set `idle_time_before_scale_down` to 0 seconds

✅ **Answer: B**  
*Setting `min_instances=0` allows the cluster to scale down to zero nodes when no jobs are queued. With `min_instances=1` or higher, at least one node stays running (and billing) at all times, even when idle.*

---

**Q3.** A data scientist wants to train a model on image files stored in Azure Blob Storage. The images are organised in subfolders by class label. Which data asset type should be registered in Azure ML?

- A) `uri_file` pointing to a single ZIP archive
- B) `mltable` with image schema defined
- C) `uri_folder` pointing to the root folder of the image dataset
- D) `mlflow_model` referencing the storage path

✅ **Answer: C**  
*`uri_folder` registers a reference to a folder path. During training, the folder is mounted or downloaded to the compute node, preserving the subfolder structure needed for image classification tasks (class labels as subfolders).*

---

**Q4.** A company has three Azure ML workspaces: Development, Staging, and Production. They want to share a curated environment across all three without duplicating it. What should they use?

- A) Copy the environment YAML to each workspace manually
- B) Use an Azure ML Registry to publish and share the environment across workspaces
- C) Create a custom Docker image in Azure Container Registry and reference it in each workspace
- D) Export the environment as a ZIP and import it in each workspace

✅ **Answer: B**  
*Azure ML Registries are organisation-level repositories for sharing assets — environments, models, components, and data — across multiple workspaces. This avoids duplication and ensures consistency.*

---

**Q5.** A team uses an Azure Data Lake Storage Gen2 account as their primary data store. Which datastore type should be registered in Azure ML to connect to it?

- A) `AzureBlobDatastore`
- B) `AzureDataLakeGen2Datastore`
- C) `AzureFileDatastore`
- D) `AzureSqlDatastore`

✅ **Answer: B**  
*`AzureDataLakeGen2Datastore` is the correct type for ADLS Gen2 connections in the Azure ML SDK. It uses account name and file system (container) name, with service principal or account key credentials.*

---

**Q6.** A training environment requires a specific version of PyTorch not available in any Azure ML curated environment. What is the correct way to define this environment?

- A) Install the package using `pip install` directly in the training script
- B) Create a custom environment with a conda YAML or Dockerfile that specifies the exact PyTorch version
- C) Modify the curated environment in-place and save it
- D) Use environment variables to specify the package version at runtime

✅ **Answer: B**  
*Custom environments are defined via conda YAML, pip requirements, or a custom Dockerfile — then registered and versioned in Azure ML. Installing packages in training scripts works but doesn't ensure reproducibility across runs.*

---

**Q7.** A data science solution processes 10 TB of raw logs in Azure Data Lake daily. The preprocessing logic uses PySpark. Which compute type is most appropriate for this step in an Azure ML pipeline?

- A) A Compute Instance with a large VM SKU
- B) An AmlCompute cluster with 10 nodes
- C) Serverless Spark Compute or an attached Synapse Spark pool
- D) A GPU Compute Cluster

✅ **Answer: C**  
*Serverless Spark Compute and Synapse Spark pools are designed for distributed big data processing using PySpark. CPU clusters (AmlCompute) are not optimised for Spark-based distributed workloads.*

---

**Q8.** A tabular dataset is registered in Azure ML as a data asset. A training job needs to load it with automatic schema inference and support for row filtering before it is passed to the training script. Which data asset type supports this?

- A) `uri_file`
- B) `uri_folder`
- C) `mltable`
- D) `mlflow_model`

✅ **Answer: C**  
*`mltable` is a data asset type that bundles a data path with a transformation spec (schema inference, column selection, row filtering). It is the preferred format for AutoML and structured training jobs.*

---

**Q9.** A team wants to track all changes to their training scripts and trigger automated retraining pipelines when code is pushed to the main branch. Which Azure ML feature supports connecting the workspace to a Git repository?

- A) Azure ML Datasets versioning
- B) Git integration on Compute Instances and CI/CD via Azure DevOps or GitHub Actions
- C) Azure ML Registries with code snapshots
- D) MLflow Projects

✅ **Answer: B**  
*Azure ML supports Git integration on Compute Instances for version control of notebooks and scripts. CI/CD pipelines (via GitHub Actions or Azure DevOps) can be triggered on push events to run Azure ML pipeline jobs.*

---

**Q10.** A model will be trained using a GPU. The team has a budget constraint and needs the lowest-cost GPU SKU that supports CUDA workloads. Which VM family should they select?

- A) Standard_E series (memory-optimised CPU)
- B) Standard_NC series (NVIDIA GPU — entry-level CUDA)
- C) Standard_ND series (NVIDIA high-bandwidth for distributed deep learning)
- D) Standard_F series (compute-optimised CPU)

✅ **Answer: B**  
*The NC series (e.g., `Standard_NC6`) provides NVIDIA GPU VMs at the lowest cost tier for CUDA workloads. ND series are higher-end, multi-GPU VMs suited for large distributed training. E and F series are CPU-only.*

---

**Q11.** A data scientist creates a new Azure ML workspace. What happens to the Azure Storage Account, Key Vault, and Application Insights resources?

- A) They must be created separately and then linked to the workspace
- B) They are automatically created and associated with the workspace during provisioning
- C) Only Storage Account is created automatically; Key Vault and App Insights must be added manually
- D) These resources are optional and only needed for production workloads

✅ **Answer: B**  
*When an Azure ML workspace is created, Azure automatically provisions and links a Storage Account (for artifacts), Key Vault (for secrets), Container Registry (for environments), and Application Insights (for endpoint monitoring).*

---

**Q12.** An organisation wants each team's models to be discoverable and reusable by other teams, with central governance over approved model versions. Which Azure ML feature best addresses this?

- A) Publishing models to the public MLflow Model Registry
- B) Using Azure ML Registries to share and govern model assets across workspaces
- C) Storing model files in a shared Azure Blob Storage container
- D) Tagging models with team names in the workspace model registry

✅ **Answer: B**  
*Azure ML Registries provide a centralised, governed asset store for models, environments, components, and data at the organisation level — separate from individual workspace registries, enabling cross-team discovery and reuse.*

---

## 🟢 Domain 2: Explore Data and Run Experiments (Q13–Q24)

---

**Q13.** An AutoML classification job completes but the data guardrails report warns about high class imbalance. Which action should be taken?

- A) Ignore the warning — AutoML handles class imbalance automatically
- B) Review the flagged issue; consider resampling the training data or using a weighted metric like `AUC_weighted`
- C) Reduce the number of training samples to balance the classes manually
- D) Switch to a regression task instead

✅ **Answer: B**  
*AutoML's data guardrails flag potential issues that could degrade model quality. Class imbalance can lead to biased models. Switching to a metric like `AUC_weighted` or `norm_macro_recall` reduces the impact of imbalance, and resampling can help.*

---

**Q14.** A data scientist wants AutoML to only try tree-based algorithms (Random Forest, Gradient Boosting, LightGBM) and skip linear and neural network models. Which parameter controls this?

- A) `blocked_training_algorithms`
- B) `allowed_training_algorithms`
- C) `model_selection`
- D) `algorithm_filter`

✅ **Answer: B**  
*`allowed_training_algorithms` restricts AutoML to only try the specified algorithms. `blocked_training_algorithms` excludes specific algorithms while allowing all others. Using `allowed_training_algorithms` gives direct control over the candidate set.*

---

**Q15.** A sweep job uses **Bayesian sampling** with 30 total trials and 5 concurrent trials. How does Bayesian sampling differ from Random sampling in choosing the next trial's parameters?

- A) Bayesian sampling tries all parameter combinations exhaustively
- B) Bayesian sampling uses a probabilistic model built from previous trial results to select the most promising next parameter set
- C) Bayesian sampling randomly samples from the search space independently of prior results
- D) Bayesian sampling is faster than Random because it uses fewer trials

✅ **Answer: B**  
*Bayesian sampling builds a surrogate probabilistic model (Gaussian Process) over the objective metric as trials complete. It uses this model to select the next trial's parameters that are most likely to improve the metric — making it more efficient than random sampling for expensive experiments.*

---

**Q16.** A sweep job has been running for 2 hours. Many trials with low learning rates are performing significantly worse than the top trials. Which early termination policy cancels trials that perform more than 10% worse than the best run?

- A) `MedianStoppingPolicy`
- B) `TruncationSelectionPolicy(truncation_percentage=10)`
- C) `BanditPolicy(slack_factor=0.1)`
- D) `NoTerminationPolicy`

✅ **Answer: C**  
*`BanditPolicy` cancels a run if its metric is not within `slack_factor` (10% here) of the best run's metric at evaluation intervals. This aggressively prunes poor performers early, saving compute while keeping promising trials.*

---

**Q17.** An MLflow run logs training loss at every epoch using `mlflow.log_metric("train_loss", loss, step=epoch)`. Where can a data scientist view the per-epoch loss curve?

- A) In the Azure ML workspace Job details → Metrics tab
- B) In the Azure ML workspace Experiments → Runs → Selected run → Charts tab
- C) In the MLflow UI served locally via `mlflow ui`
- D) Both B and C are correct

✅ **Answer: D**  
*Azure ML natively renders MLflow metrics — including step-based metrics — as charts in the Studio UI (Jobs → Experiments). The same data is also viewable in the MLflow UI if connected to the Azure ML tracking URI locally.*

---

**Q18.** A data scientist uses `mlflow.sklearn.autolog()` before training a scikit-learn model. Which of the following is NOT automatically captured by autolog?

- A) Model hyperparameters (e.g., `C`, `max_iter`)
- B) Training metrics (e.g., `accuracy`, `f1`)
- C) The trained model artifact
- D) Business KPIs from the production system

✅ **Answer: D**  
*MLflow autolog captures framework-specific parameters, metrics, and the model artifact. It cannot capture custom business metrics or production system KPIs — these must be logged manually with `mlflow.log_metric()`.*

---

**Q19.** A feature store materialises features into an offline store. A data scientist retrieves training features using a point-in-time join. What problem does point-in-time joining prevent?

- A) Slow query performance on large feature tables
- B) Feature leakage — using feature values that would not have been available at prediction time in training
- C) Duplicate feature values across different feature sets
- D) Schema mismatches between training and serving features

✅ **Answer: B**  
*Point-in-time joins retrieve feature values as they existed at the observation timestamp — not the current value. Without this, a model might accidentally train on future data (leakage), leading to unrealistically good offline metrics that degrade in production.*

---

**Q20.** A Spark wrangling step in a notebook should process data in an attached Synapse pool without switching tools. Which syntax routes notebook cell execution to the Spark pool?

- A) `%%spark`
- B) `%%synapse`
- C) `%%pyspark`
- D) `%run spark`

✅ **Answer: B**  
*The `%%synapse` magic in Azure ML notebooks routes cell execution to the attached Synapse Spark pool. `%%spark` and `%%pyspark` are used within Synapse Studio notebooks directly, not from Azure ML.*

---

**Q21.** During AutoML training for a time series forecasting task, which data preparation step is handled **automatically** by AutoML that a custom model would require manually?

- A) Removing duplicate rows
- B) Generating lag features, rolling window aggregations, and calendar features from the datetime column
- C) Splitting the dataset into training and test sets
- D) Normalising the target variable

✅ **Answer: B**  
*AutoML's time-series mode automatically generates lag features (previous values), rolling statistics, and calendar-based features (day of week, month, holiday indicators) from datetime columns — a substantial engineering effort if done manually.*

---

**Q22.** A hyperparameter search space includes `learning_rate: loguniform(-4, -1)`. What range of actual learning rate values does this represent?

- A) Between -4 and -1
- B) Between 0.0001 (10⁻⁴) and 0.1 (10⁻¹)
- C) Between 4 and 1, on a log scale
- D) A random integer between 4 and 1

✅ **Answer: B**  
*`loguniform(low, high)` samples values uniformly on the log scale between `exp(low)` and `exp(high)`. With base-10 logarithm: `10^-4 = 0.0001` to `10^-1 = 0.1`. Log-uniform is ideal for learning rates since good values span multiple orders of magnitude.*

---

**Q23.** The Responsible AI Dashboard shows an Error Analysis heatmap with high error rates for customers aged 18–25 with low income. What should a data scientist do with this information?

- A) Remove the 18–25 age group from the training dataset
- B) Investigate whether these cohorts are underrepresented in training data and consider rebalancing or targeted data collection
- C) Add age and income as additional features if not already present
- D) Deploy the model with a disclaimer that it's less accurate for this group

✅ **Answer: B**  
*Error Analysis reveals where the model underperforms across data cohorts. The responsible action is to investigate the root cause — typically underrepresentation in training data — and address it through data collection, resampling, or bias mitigation techniques.*

---

**Q24.** A data scientist wants to compare three completed MLflow runs to select the best model. Where in Azure ML Studio can they do this?

- A) Models → Compare tab
- B) Jobs → Experiments → Select runs → Compare
- C) Endpoints → Deployments → Compare models
- D) Datastores → Run history

✅ **Answer: B**  
*In Azure ML Studio, within an Experiment, you can select multiple runs and click "Compare" to see metrics, parameters, and charts side-by-side — making it easy to identify the best performing configuration.*

---

## 🟠 Domain 3: Train and Deploy Models (Q25–Q37)

---

**Q25.** A training script fails with `FileNotFoundError: training_data/labels.csv not found` even though the file exists in the registered dataset. What is the most likely cause?

- A) The file extension is not supported by Azure ML
- B) The input data is mounted but the script uses a hardcoded path instead of `args.training_data`
- C) The Compute Cluster does not have access to the datastore
- D) The data asset version specified in the job does not exist

✅ **Answer: B**  
*When data is mounted, it's available at the path provided via the job argument (e.g., `${{inputs.training_data}}`). If the script hardcodes a local path like `training_data/labels.csv` instead of reading from `args.training_data`, it will fail because the mounted path is different.*

---

**Q26.** A pipeline has three sequential steps: `prep_data → train_model → evaluate_model`. The prep_data step is expensive (1 hour) and the underlying data hasn't changed. How can the pipeline avoid re-running prep_data on the next execution?

- A) Set `skip_unchanged=True` on the pipeline job
- B) Azure ML automatically caches step outputs when inputs and code haven't changed
- C) Use a condition step to check if prep_data output already exists
- D) Pipelines always re-run all steps; caching is not supported

✅ **Answer: B**  
*Azure ML Pipeline steps cache their outputs automatically. If a step's code, environment, inputs, and parameters are identical to a previous run, the cached output is reused and the step is skipped — saving time and compute costs.*

---

**Q27.** A pipeline component outputs a processed dataset. The next component needs to read it. How should data be passed between pipeline steps?

- A) Write to a shared Azure Blob path hardcoded in both scripts
- B) Define the output as an `Output()` in the first component and wire it to an `Input()` in the second component in the pipeline definition
- C) Use Azure Service Bus to send a message between steps
- D) Save the data to a global variable accessible across steps

✅ **Answer: B**  
*Azure ML pipeline components communicate by declaring `Output` and `Input` objects. In the `@pipeline` function, the output of one step is directly passed as an argument to the next step, and Azure ML handles the data transfer automatically.*

---

**Q28.** You need to run a pipeline on the first Monday of every month at 06:00 UTC. Which schedule trigger type should be used?

- A) `CronTrigger` with expression `0 6 1-7 * 1`
- B) `RecurrenceTrigger` with `frequency="month"` and the correct schedule parameters
- C) `TimerTrigger` with a monthly interval
- D) `EventTrigger` on a storage account file upload

✅ **Answer: A**  
*A CRON expression `0 6 1-7 * 1` means: minute 0, hour 6, day 1–7 of the month, any month, Monday (1) — which together express the first Monday of every month at 06:00. `RecurrenceTrigger` can also achieve this via specific weekday + `occurrence` settings.*

---

**Q29.** A registered MLflow model includes an `input_example` in its artifact. What is the primary purpose of this example?

- A) Used by Azure ML to automatically generate test traffic to the deployed endpoint
- B) Documents the expected input format and enables automatic input schema validation
- C) Required for the model to be registered — it fails without it
- D) Used to set the default batch size for batch endpoints

✅ **Answer: B**  
*The `input_example` in an MLflow model provides sample input data that documents the model's expected format. When combined with a signature, it enables schema validation during deployment and serves as auto-generated API documentation.*

---

**Q30.** A managed online endpoint receives sporadic traffic. The deployment `instance_count` is set to 1. Occasionally, requests time out during high-load moments. What is the most appropriate fix?

- A) Enable auto-scaling on the deployment with a minimum of 1 and maximum of 5 instances
- B) Switch to a batch endpoint
- C) Increase the `request_timeout_ms` setting
- D) Move to a larger VM SKU

✅ **Answer: A**  
*Auto-scaling allows the deployment to increase instance count during traffic spikes and scale back down when quiet. This handles sporadic high-load moments without permanently paying for extra instances.*

---

**Q31.** A model is deployed to an online endpoint as the "blue" deployment with 100% traffic. A new model version ("green") needs to be tested with 5% of live traffic. What steps are required?

- A) Delete the blue deployment and redeploy as green
- B) Create the green deployment on the same endpoint, then update the endpoint's traffic split to `{"blue": 95, "green": 5}`
- C) Create a separate endpoint for green and use Azure Traffic Manager to split traffic
- D) Deploy green to a staging slot and swap it with blue

✅ **Answer: B**  
*Managed online endpoints support multiple named deployments. Traffic weights are configured at the endpoint level — allowing gradual rollouts without service interruption or the need for a separate endpoint.*

---

**Q32.** A batch deployment processes customer records and writes predictions to an output file. After invoking the batch endpoint, what does the caller receive immediately?

- A) The completed prediction file
- B) A streaming response with predictions as they are generated
- C) A job ID that can be used to monitor the batch scoring job's progress
- D) An HTTP 200 response with predictions embedded in the body

✅ **Answer: C**  
*Batch endpoint invocations are asynchronous. The `invoke()` call returns a job reference (job ID/name) immediately. The caller must poll the job status to know when it completes and then retrieve the output from the designated output location.*

---

**Q33.** An MLflow model is registered with version 2. The team has validated it and wants to mark it as the current production model. Which MLflow model lifecycle stage should it be transitioned to?

- A) `Approved`
- B) `Production`
- C) `Active`
- D) `Released`

✅ **Answer: B**  
*MLflow defines four model stages: `None`, `Staging`, `Production`, and `Archived`. Transitioning a model to `Production` signals that it is the current approved version for live inference.*

---

**Q34.** A training job fails with the log message: `No module named 'lightgbm'`. The training script imports LightGBM. What is the root cause and fix?

- A) LightGBM is not compatible with Azure ML — use scikit-learn instead
- B) The environment used by the job does not include LightGBM — add it to the conda YAML and update the environment version
- C) The Compute Cluster needs to be restarted to pick up newly installed packages
- D) LightGBM requires a GPU cluster to run

✅ **Answer: B**  
*`No module named 'lightgbm'` means the package is not installed in the job's environment. The fix is to add `lightgbm` to the environment's conda YAML or pip requirements, register a new environment version, and re-run the job.*

---

**Q35.** A component's output is defined as `type: mltable`. The next component's input expects `type: uri_folder`. Will the pipeline succeed?

- A) Yes — Azure ML automatically converts between compatible types
- B) No — the output type must match the input type exactly; either change the output to `uri_folder` or the input to `mltable`
- C) Yes — `mltable` is a subset of `uri_folder` and is always compatible
- D) Only if both components use the same environment

✅ **Answer: B**  
*Azure ML pipeline component type contracts must match. `mltable` and `uri_folder` are distinct asset types and are not automatically interchangeable. Mismatched types cause a validation error when the pipeline is submitted.*

---

**Q36.** A batch endpoint is configured with `mini_batch_size=50` and `instance_count=4`. An input dataset has 1,000 records. How many mini-batches will be processed?

- A) 4 (one per instance)
- B) 20 (1000 ÷ 50)
- C) 50 (one per record group)
- D) 250 (1000 ÷ 4)

✅ **Answer: B**  
*`mini_batch_size` defines how many records each worker processes in one batch. With 1,000 records and a mini-batch size of 50, there are 20 mini-batches. The 4 instances process these mini-batches in parallel.*

---

**Q37.** After deploying a model to a managed online endpoint, a developer wants to verify the endpoint is working before sending it production traffic. What is the recommended approach?

- A) Monitor the endpoint for 24 hours and check Application Insights logs
- B) Use `ml_client.online_endpoints.invoke()` with a sample request to test the endpoint directly
- C) Send a small percentage of production traffic immediately and observe the results
- D) Review the model's offline evaluation metrics before enabling traffic

✅ **Answer: B**  
*`invoke()` sends a direct test request to a specific deployment (using `deployment_name=`) before routing traffic to it. This validates the endpoint is operational and returns expected predictions without exposing it to production load.*

---

## 🟣 Domain 4: Optimize Language Models (Q38–Q50)

---

**Q38.** A team needs a language model for summarising internal engineering reports. They compare GPT-4 and Phi-3-mini. GPT-4 scores higher on MMLU but costs 20× more. Phi-3-mini meets quality requirements in testing. What is the recommended decision framework?

- A) Always choose the highest benchmark score regardless of cost
- B) Select the model that meets quality requirements at the lowest cost — Phi-3-mini is appropriate here
- C) Use GPT-4 in development and Phi-3-mini in production
- D) Deploy both and let users choose

✅ **Answer: B**  
*Benchmark scores indicate capability, but the right model is the one that meets your quality threshold at acceptable cost. If Phi-3-mini meets requirements in testing, its lower cost makes it the better choice for production workloads.*

---

**Q39.** A Prompt Flow has three nodes: `generate_query` (LLM) → `search_documents` (Python) → `generate_answer` (LLM). The `search_documents` node fails with a timeout. How should this be diagnosed?

- A) Rerun the entire flow from the beginning
- B) Use the Prompt Flow tracing view to inspect the inputs, outputs, and execution time of each node individually
- C) Increase the temperature on the `generate_query` LLM node
- D) Check the model deployment logs in Azure OpenAI Studio

✅ **Answer: B**  
*Prompt Flow's tracing feature captures per-node execution details — inputs, outputs, latency, and errors. This allows you to isolate exactly which node failed, with what input, and why — without re-running the entire flow.*

---

**Q40.** A Prompt Flow is evaluated using the built-in `groundedness` metric. The average score is 2.1 out of 5. What does this indicate and what should be improved?

- A) The LLM's temperature is too high — lower it to improve consistency
- B) The retrieved context chunks are not relevant or sufficient — improve the retrieval component (chunking, embedding, or search configuration)
- C) The system prompt is too long — shorten it
- D) The evaluation dataset is too small to produce reliable scores

✅ **Answer: B**  
*Low groundedness means the model's answers are not well-supported by the provided context — indicating a retrieval problem. Improvements include better chunking strategies, higher-quality embeddings, hybrid search, or increasing the number of retrieved chunks (top-K).*

---

**Q41.** A RAG solution retrieves 5 document chunks per query. The answer quality is good, but chunk boundaries sometimes cut off important context mid-sentence. Which chunking improvement addresses this?

- A) Increase `chunk_size` to 2048 tokens
- B) Add `chunk_overlap` to ensure boundary regions are included in adjacent chunks
- C) Switch from fixed-size chunking to summarisation-based chunking
- D) Reduce the number of retrieved chunks from 5 to 3

✅ **Answer: B**  
*Adding `chunk_overlap` means each chunk shares N tokens with the previous and next chunk. This ensures that context split at a boundary appears in both adjacent chunks — preventing mid-sentence cuts from losing important information.*

---

**Q42.** A developer uses `pf.run()` to run two Prompt Flow variants against the same evaluation dataset. Variant 0 uses a formal system prompt and variant 1 uses a conversational tone. How should results be compared?

- A) Export both runs as CSV and compare manually in Excel
- B) Use `pf.visualize([run_variant_0, run_variant_1])` to compare metrics side-by-side
- C) Use Azure ML's A/B testing endpoint feature
- D) Deploy both variants and measure production click-through rates

✅ **Answer: B**  
*`pf.visualize()` renders an interactive comparison of multiple Prompt Flow runs — showing per-example outputs and aggregate metrics side-by-side. This is the standard way to compare prompt variants before selecting one for production.*

---

**Q43.** A fine-tuning job's `validation_loss` starts increasing after epoch 2 while `training_loss` continues to decrease. What does this indicate?

- A) The model is underfitting — increase the number of training examples
- B) The learning rate is too low — increase it
- C) The model is overfitting — reduce `n_epochs` or add more diverse training data
- D) The training data has labelling errors

✅ **Answer: C**  
*A divergence between training and validation loss — where training loss decreases but validation loss increases — is the classic signature of overfitting. The model is memorising training examples rather than learning generalisable patterns. Reducing epochs or increasing data diversity helps.*

---

**Q44.** When fine-tuning a language model, you want the model to learn a specific JSON output format for all responses. What should the training data include?

- A) Examples where the `user` message describes the JSON format in detail
- B) Consistent `assistant` responses in the desired JSON format across all training examples
- C) A `system` message in each example that says `"Always respond in JSON"`
- D) Both B and C — consistent JSON responses in `assistant` turns AND a system prompt specifying the format

✅ **Answer: D**  
*For consistent output formatting, both a system prompt (C) declaring the format and consistent `assistant` responses (B) demonstrating the format are important. The model learns the pattern from examples and is guided by the system prompt at inference time.*

---

**Q45.** A model catalog deployment uses the `Serverless` endpoint type (pay-per-token). A team expects high, consistent throughput of 50,000 tokens/minute. Which deployment type would be more cost-effective at this volume?

- A) Global Standard (still pay-per-token but lower per-token rates)
- B) Provisioned throughput (reserved PTUs) — more cost-effective at sustained high volume
- C) Batch deployment with scheduled jobs
- D) Standard deployment with auto-scaling

✅ **Answer: B**  
*Provisioned throughput (PTUs) reserves a fixed capacity at a flat hourly rate — becoming more cost-effective than pay-per-token pricing at sustained high throughput. At low/variable usage, pay-per-token is cheaper.*

---

**Q46.** An Azure AI Search index is configured for a RAG solution. A user query returns documents that are keyword matches but not semantically relevant. What configuration change would improve semantic relevance?

- A) Add more fields to the index schema
- B) Enable semantic ranking (semantic search configuration) and use `query_type="semantic"`
- C) Increase the `$top` parameter to return more results
- D) Add a filter on the `source` field

✅ **Answer: B**  
*Semantic ranking applies a language model to re-rank BM25 keyword results by semantic relevance. Documents that match keywords but are contextually irrelevant will be pushed down; semantically relevant documents will surface higher.*

---

**Q47.** A Prompt Flow is used in a customer support bot. After deployment, some conversations return irrelevant answers because the retrieval step fetches the wrong document chunks. Which built-in evaluation metric specifically measures the quality of the retrieval step?

- A) `fluency`
- B) `coherence`
- C) `retrieval_relevance` (also called `context_relevance`)
- D) `f1_score`

✅ **Answer: C**  
*`retrieval_relevance` (or `context_relevance`) specifically measures whether the retrieved chunks are relevant to the user's query — independently of the LLM's answer quality. Low scores indicate the retriever needs improvement.*

---

**Q48.** A developer wants to deploy a language model from the Azure AI Foundry model catalog for **zero-cost prototyping** without provisioning dedicated compute. Which deployment option should they use?

- A) Provisioned throughput endpoint
- B) Managed online endpoint with `Standard_DS3_v2`
- C) Serverless API endpoint (pay-per-token, no infrastructure provisioning)
- D) Azure Container Apps deployment

✅ **Answer: C**  
*Serverless API endpoints in Azure AI Foundry require no infrastructure provisioning — you simply deploy a model from the catalog and are billed only for tokens consumed. This is ideal for prototyping with zero upfront cost.*

---

**Q49.** When preparing a RAG solution, documents are split into 512-token chunks and embedded using `text-embedding-3-small`. Later, a different embedding model is used for query embedding at inference time. What problem does this cause?

- A) No problem — different embedding models produce compatible vectors
- B) The vector dimensions may differ, causing the similarity search to fail
- C) A dimension mismatch OR incompatible vector spaces — documents and queries must be embedded with the same model for meaningful similarity
- D) Query embedding must always use a larger model than document embedding

✅ **Answer: C**  
*Embedding models produce vectors in their own learned vector space. Even if dimensions match, different models embed semantically different meanings differently. For cosine similarity to be meaningful, documents and queries must be embedded with the **same model**.*

---

**Q50.** A fine-tuned GPT-3.5 Turbo model has been validated. The team now wants to promote it to production and retire the previous version. Using the Azure AI Foundry SDK or MLflow, which sequence of actions is correct?

- A) Delete the old deployment → create the new deployment
- B) Deploy the fine-tuned model as a new deployment → validate with test traffic → shift traffic → archive the old model version
- C) Register the fine-tuned model → transition it to `Production` stage → delete the `Staging` model
- D) Fine-tuned models cannot be versioned and managed in the Azure ML model registry

✅ **Answer: B**  
*Best practice for production promotion: deploy the new version alongside the old one → validate with a small traffic percentage → gradually shift traffic → finally delete or scale down the old deployment. This ensures zero-downtime rollout and easy rollback.*

---

## 📊 Summary by Domain

| Domain | Questions | Exam Weight |
|--------|-----------|------------|
| 1 – Design & Prepare | Q1–Q12 | 20–25% |
| 2 – Explore & Experiments | Q13–Q24 | 20–25% |
| 3 – Train & Deploy | Q25–Q37 | 25–30% |
| 4 – Language Models | Q38–Q50 | 25–30% |
| **Total** | **50** | **100%** |

---

## 🔗 Study Resources

| Resource | Link |
|----------|------|
| Official Study Guide | [DP-100 Study Guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/dp-100) |
| Free Practice Assessment | [Practice Questions](https://learn.microsoft.com/en-us/credentials/certifications/exams/dp-100/practice/assessment?assessment-type=practice&assessmentId=62) |
| Azure ML Documentation | [Azure Machine Learning](https://learn.microsoft.com/en-us/azure/machine-learning/) |
| Azure AI Foundry | [Azure AI Foundry Docs](https://learn.microsoft.com/en-us/azure/ai-studio/) |
| MLflow Docs | [MLflow on Azure ML](https://learn.microsoft.com/en-us/azure/machine-learning/concept-mlflow) |
| Prompt Flow | [Prompt Flow Docs](https://learn.microsoft.com/en-us/azure/machine-learning/prompt-flow/overview-what-is-prompt-flow) |

---

📘 [← Back to DP-100 Index](./index.md)

*Good luck with DP-100! 🚀 Train & Deploy and Language Models are each 25–30% — give them equal weight in your prep.*
