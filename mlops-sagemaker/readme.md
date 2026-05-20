
#  End-to-End MLOps Pipeline using Amazon SageMaker

##  Project Overview

The project uses a combination of cloud, machine learning, and development tools including AWS services such as SageMaker for model training and deployment, S3 for data and model storage, and IAM for access management, along with Python as the programming language, and key libraries like scikit-learn for building the machine learning model, pandas and numpy for data processing, boto3 for interacting with AWS services, and joblib for model serialization, while AWS CLI is used for configuring and connecting the local environment to AWS, and development is done using tools like VS Code or Jupyter Notebook for coding and experimentation.
---

##  Technology Stack & Tools Utilized
Tech Stack

Cloud

Amazon SageMaker
Amazon S3
AWS Identity and Access Management

Programming & ML

Python
scikit-learn
pandas
NumPy

SDK & Tools

boto3
joblib
AWS Command Line Interface

Dev Environment

Visual Studio Code / Jupyter Notebook


---

##  Workflow

### 1. Data Ingestion
* Load dataset using Pandas
* Perform preprocessing and validation
* Split into train and test datasets
* Upload datasets to AWS S3

### 2. Model Training
* Use SageMaker SKLearn Estimator
* Train model on SageMaker-managed instances
* Log parameters, metrics, and model artifacts using MLflow
* Save trained model artifacts to S3

### 3. Model Evaluation
* Evaluate model using test dataset
* Generate accuracy and classification metrics
* Track and compare experiment results using MLflow
  
### 4. Model Registry
Register trained model in MLflow Model Registry, 
Manage model versions (v1, v2, etc.), 
Promote selected model to Production stage

### 5. Deployment
* Deploy trained model as a SageMaker endpoint
* Expose endpoint for real-time predictions

### 6. Inference
* Send input data to endpoint
* Receive predictions via API

### 7. Cleanup
* Delete endpoint after usage to avoid extra cost

---

##  Project Structure
```
mlops-sagemaker-pipeline/
│
├── src/
│   ├── data_ingestion.py
│   ├── train.py
│   ├── inference.py
│   └── utils.py
│
├── notebooks/
│   └── experimentation.ipynb
│
├── scripts/
│   └── script.py
│
├── artifacts/
│
├── requirements.txt
├── README.md
└── pipeline.py
```
---

##  Setup Instructions

### Step 1: Clone Repository
```
git clone https://github.com/sandya-portrfolio/MLOPS/edit/main/mlops-sagemaker.git
cd mlops-sagemaker-pipeline
```
Setup Instructions

### Step 2: Create Environment (Conda)
```bash
conda deactivate
conda create -n sagemaker-mlops python=3.10 -y
conda activate sagemaker-mlops
```
### Step 3: Install Dependencies
```bash
pip install -r requirements.txt
```
### Step 2: Install Dependencies
```
pip install -r requirements.txt
```
### Step 3: Configure AWS CLI
```
aws configure
```
Provide:
* AWS Access Key
* Secret Key
* Region (e.g., us-east-1)
---
##  Run the Project

### Train Model

```
python train.py
```
### Deploy Model
```
python deploy.py
```
### Run Inference
```
python inference.py
```
---
##  Model Used
* Random Forest Classifier (Scikit-learn)
---
##  Key Features
* End-to-end ML lifecycle implementation
* Automated training on cloud instances
* Scalable deployment using SageMaker endpoints
* Cloud-based data storage using S3
---

##  Future Improvements
* Add CI/CD pipeline (GitHub Actions / Jenkins)
* Implement model versioning (MLflow integration)
* Add monitoring & logging
* Use Docker + Kubernetes for advanced deployment
---

## key points
* Hands-on experience with AWS SageMaker
* Understanding of MLOps workflow
* Cloud-based model deployment
* Managing ML lifecycle in production

---

##  Cleanup Note
Always delete active endpoints after use to avoid unnecessary AWS charges:

```
delete_endpoint()
```
---
