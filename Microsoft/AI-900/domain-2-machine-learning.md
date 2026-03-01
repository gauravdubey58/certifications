# Domain 2 – Fundamental ML Principles on Azure `20–25%`

> ⬅️ [Back to AI-900 Index](./index.md)

---

## 2.1 Core Machine Learning Concepts

### What is Machine Learning?
Machine learning is a method of training computers to **learn patterns from data** and make predictions or decisions **without being explicitly programmed** for each scenario.

### Types of Machine Learning

#### Supervised Learning
- Model learns from **labeled training data** (input + correct output pairs)
- Goal: learn a mapping function from inputs to outputs
- Two main types:

| Type | Task | Output | Example |
|------|------|--------|---------|
| **Regression** | Predict a numeric value | Continuous number | House price prediction, temperature forecast |
| **Classification** | Predict a category | Discrete label | Spam or not spam, disease diagnosis |

#### Unsupervised Learning
- Model finds patterns in **unlabeled data** (no predefined answers)
- Goal: discover hidden structure

| Type | Task | Example |
|------|------|---------|
| **Clustering** | Group similar data points | Customer segmentation, document grouping |
| **Dimensionality Reduction** | Reduce number of features | Data compression, visualization |
| **Anomaly Detection** | Find outliers | Fraud detection, defect identification |

#### Reinforcement Learning
- Agent learns by **interacting with an environment** and receiving rewards/penalties
- Goal: maximize cumulative reward over time
- Examples: game playing (chess, Go), robotics, self-driving cars

---

## 2.2 The Machine Learning Workflow

```
1. Data Collection → 2. Data Preparation → 3. Feature Engineering →
4. Model Training → 5. Model Evaluation → 6. Model Deployment → 7. Monitoring
```

### Data Splitting
- **Training set** (typically 70–80%): used to train the model
- **Validation set**: tune hyperparameters during training
- **Test set** (typically 20–30%): final evaluation on unseen data

### Key ML Terms

| Term | Definition |
|------|-----------|
| **Feature** | An input variable used to make predictions (e.g., age, price) |
| **Label** | The target output the model predicts (e.g., survived = yes/no) |
| **Model** | The mathematical function learned from training data |
| **Training** | The process of fitting the model to training data |
| **Inference** | Using a trained model to make predictions on new data |
| **Overfitting** | Model learns training data too well; poor on new data |
| **Underfitting** | Model is too simple; poor on both training and new data |
| **Hyperparameter** | Setting configured before training (e.g., learning rate) |

### Evaluation Metrics

**For Regression:**
| Metric | What It Measures |
|--------|-----------------|
| **MAE** (Mean Absolute Error) | Average absolute difference between predicted and actual |
| **MSE** (Mean Squared Error) | Average squared difference (penalizes large errors more) |
| **RMSE** (Root MSE) | Square root of MSE; in same units as target |
| **R²** (R-squared) | Proportion of variance explained by model (1.0 = perfect) |

**For Classification:**
| Metric | What It Measures |
|--------|-----------------|
| **Accuracy** | % of correct predictions overall |
| **Precision** | Of all predicted positives, how many are actually positive |
| **Recall** | Of all actual positives, how many did we predict correctly |
| **F1 Score** | Harmonic mean of precision and recall |
| **AUC-ROC** | Area under ROC curve; model's ability to distinguish classes |

> ⭐ **Exam Tip:** For classification, a **confusion matrix** shows True Positives (TP), True Negatives (TN), False Positives (FP), and False Negatives (FN). Know the basics.

---

## 2.3 Azure Machine Learning

Azure Machine Learning (Azure ML) is a **cloud platform for training, deploying, and managing ML models** at scale.

### Key Components of Azure ML

| Component | Description |
|-----------|-------------|
| **Workspace** | Top-level resource; contains all ML assets |
| **Compute** | Resources for training and deployment (clusters, instances) |
| **Datastore** | Connection to data sources (Blob, ADLS, SQL) |
| **Dataset / Data Asset** | Registered data for training |
| **Experiment / Job** | A training run |
| **Model** | A trained model registered in the model registry |
| **Endpoint** | Deployed model accessible via REST API |
| **Pipeline** | Workflow of ML steps (preprocessing, training, evaluation) |
| **Environment** | Docker/Conda configuration for reproducibility |

### Compute Types in Azure ML

| Compute Type | Use Case |
|-------------|---------|
| **Compute Instance** | Interactive development (Jupyter notebooks) |
| **Compute Cluster** | Scalable training (auto-scales, pay when used) |
| **Inference Cluster (AKS)** | Real-time production deployment |
| **Serverless Compute** | Fully managed, no provisioning needed |

### Automated ML (AutoML)
- Automatically **tries multiple algorithms and hyperparameters**
- Selects the best performing model
- Reduces need for ML expertise
- Supports: Classification, Regression, Time Series Forecasting, NLP, Computer Vision
- Accessible via: Azure ML Studio UI, Azure ML SDK

### Azure ML Designer
- **Drag-and-drop visual interface** for building ML pipelines
- No code required for many tasks
- Pre-built modules for data prep, training, evaluation
- Great for beginners and quick prototyping

### MLflow Integration
- Azure ML integrates with **MLflow** for experiment tracking, model registration, and deployment
- Open standard for ML lifecycle management

### Deployment Options

| Deployment Type | Description | Use Case |
|----------------|-------------|---------|
| **Real-time endpoint** | Always-on REST API for instant predictions | Web app integration |
| **Batch endpoint** | Process large batches asynchronously | Overnight scoring |
| **Online endpoint** | Managed real-time inference | Production models |

---

## 2.4 Responsible ML in Azure

### Model Interpretability
- **Model explanations** — understand why a model made a prediction
- Azure ML supports: **SHAP values**, feature importance
- Interpretability dashboard in Azure ML Studio

### Fairness Assessment
- **Fairlearn** integration in Azure ML
- Assess disparities in model performance across demographic groups
- Mitigate identified fairness issues

### Data Drift Monitoring
- Monitors production data vs training data over time
- Alerts when model performance may be degrading
- Helps determine when to retrain

---

> ⬅️ [← Domain 1](./domain-1-ai-workloads.md) | [Back to Index](./index.md) | [Domain 3 →](./domain-3-computer-vision.md)
