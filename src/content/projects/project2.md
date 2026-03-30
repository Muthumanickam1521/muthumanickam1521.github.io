---
title: "AutoML Solution for Drug Discovery" 
description: ""
pubDate: 2025-05-18
heroImage: "/project2_banner.png"
tags: ["scikit-learn", "NumPy", "Pandas", "PyTorch", "TensorFlow", "AsyncIO", "FastAPI", "MangoDB", "AutoML", "Plotly", "Seaborn", "Matplotlib", "RDKit", "Machine Learning", "Deep Learning", "Optuna", "Hyper-parameter Tuning", "Neural Networks", "Pipeline", "Model Deployment"]
---


# Overview

AI in drug discovery helps in predicting how a new drug (a chemical substance like paracetamol) will behave and whether it’s safe or effective just by looking at its chemical structure. This speeds up drug development, reduces cost, and helps scientists focus on the most promising medicines. 

As many scientists often require a tool to make AI more accessible, we come up with proposal of developing an AutoML solution (a pipeline that automated machine learning model building process, an end-to-end solution). Though most of the workflow is pipelined and the project itself is implemented as Human-in-the-loop HITL.


---

# Objective

To develop a workflow for automated machine learning model building process (in short AutoML solution)


---

# Workflow

Typical drug dataset might look like:

![workflow_image](/project2_image3.png)

> **Note (High level):** Given a dataset (contains molecules, identifier, target property) → preprocesses data → create features → engineer features → trains ML models → hyperparameter tunes best model → ready-for-prediction  
> **Code**: Can’t be disclosed as this project is owned by my employer.
> ![depiction.png](/project2_image2.png)
> ![hla.png](/project2_image1.png)


---

# Tech Stack

- Core AI/ML
    - scikit-learn - traditional ML algorithms
    - LightGBM - gradient boosting algorithm that fits data with less loss
    - RDKit - to generate features
    - PyTorch - constructing custom neural network architectures
- Data Preprocessing / Feature Enginering
    - pandas / NumPy - data manipulation and numerical operations
    - category_encoders - encoding categorical features
    - SciPy / stats - statistical preprocessing
    - t-SNE/UMAP - reduce dimensions of feature space
- Visualization
    - matplotlib / seaborn / Plotly - for exploratory data analysis, plots, and model performance evaluation
- Model Optimization
    - GridSearchCV / Optuna - finding optimal parameter set
- Model Deployemt / APIs
    - FastAPI
    - pickle - packaging objects (models, other objects)
    - AsyncIO - asynchronous execution of jobs (model training, tuning, prediction)
- Databases / Data Storage
    - pymango / motor - a MangoDB and its client
- Software Engineering Practices
    - logging / tqdm / email - logs outputs, updates (or) errors, showing execution using progress bar, triggering email notifications


---

# Key Features

1. Can solve regression as well as classification problems. In classification, it is capable of multi-label training as well.
2. Performs computations such as feature generation, dimension reduction, model training, model tuning, etc. in parallel, effectively and seamlessly.
3. Handles raw datasets which are too dirty.
4. Performs automatic feature selection
5. Trains a set of ML models and choose the best one based on metrics RMSE (for regression) and F1 score / AUC (for classification).
6. Finetunes the best model automatically without needing to manual intervention
7. Saves the ready-to-use model for later needs 
8. Logs everything including progress, successes, and failures
9. Triggers email notifications upon training job submission, progression, completion, (or) in-completion
10. **4000+** lines of code, modular, re-usable
11. Use of **Blue-Green** deployment approach - Deploying in an environment and making it accessible to a population


---

# Achievements

1. Handles multiple machine learning jobs (training, tuning, prediction) at the same time, utilizing computer memory efficiently 
2. Improved model performance by **8%** (on average for both regression / classification) with feature engineering 
3. Optimized the workflow’s overall performance in model hyper-parameter tuning by smartly including impactful parameters of each algorithm.
4. Improved model performance by **11%** using Optuna (an automated hyperparameter tuning framework that uses Bayesian optimization strategy) 

> **11%** performance improvement: Average of % difference before/after tuning, **200 times tested** with different datasets.
> 
5. Potential to shrink drug discovery timeline form **20 years → 2+ years**


---

# Business Impact

1. **Major**: Helps Scientist validate their new drug molecule by predicting molecular properties (like vaporization). 
2. Used by **30+** internal clients 
3. **Minor**: Attracts investors and stakeholders for future projects in AI

> Example, I’m a scientist and found a chemical compound for treating headache. I want to validate whether it is really suitable or not with testing. But it would take so much time and might cost me more. With AutoML solution, I can understand more about molecules properties. If it has extreme value in a property, it helps deciding not to process further.
>