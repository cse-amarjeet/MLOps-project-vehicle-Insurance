
# 🚗 Vehicle MLOps Project

[![Python 3.10](https://img.shields.io/badge/python-3.10-blue.svg)](https://www.python.org/)  
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)  
[![Build Status](https://img.shields.io/github/actions/workflow/status/yourusername/vehicle-mlops/ci-cd.yaml?branch=main)](https://github.com/yourusername/vehicle-mlops/actions)  

An end-to-end MLOps pipeline for vehicle data:  
automated data ingestion from MongoDB, EDA & feature engineering, model training & evaluation, AWS S3 storage, and seamless CI/CD deployment to an EC2-hosted web app.

---

## 📋 Table of Contents

1. [Features](#features)  
2. [Tech Stack](#tech-stack)  
3. [Prerequisites](#prerequisites)  
4. [Local Setup](#local-setup)  
5. [MongoDB Atlas Setup](#mongodb-atlas-setup)  
6. [Logging, Exceptions & Notebooks](#logging-exceptions--notebooks)  
7. [Data Ingestion](#data-ingestion)  
8. [Data Validation, Transformation & Training](#data-validation-transformation--training)  
9. [AWS & Model Evaluation](#aws--model-evaluation)  
10. [Prediction Pipeline & Web App](#prediction-pipeline--web-app)  
11. [CI/CD & Deployment](#cicd--deployment)  
12. [Project Structure](#project-structure)  
13. [Contributing](#contributing)  
14. [License](#license)  

---

## 🎯 Features

- **Automated Project Template**  
- **Local package management** via `setup.py` & `pyproject.toml`  
- **Conda-based** virtual environment  
- **MongoDB Atlas** ingestion & management  
- **Structured logging** & **custom exception handling**  
- **Exploratory Data Analysis** & **Feature Engineering** notebooks  
- **Data validation**, **transformation**, **model training** & **evaluation**  
- **AWS S3** model registry & **Model Pusher**  
- **Dockerized** microservice & **GitHub Actions** CI/CD  
- **Self-hosted runner** on AWS EC2 for continuous deployment  

---

## 🔧 Tech Stack

| Layer                   | Tools & Libraries                                     |
|-------------------------|--------------------------------------------------------|
| Language                | Python 3.10                                            |
| Environment             | Conda, `virtualenv`                                    |
| Data Store              | MongoDB Atlas                                          |
| Logging & Exceptions    | `logging`, custom exception classes                    |
| Data Science            | Pandas, NumPy, Scikit-learn                            |
| Cloud                   | AWS S3, IAM, EC2                                       |
| CI/CD                   | GitHub Actions, Docker, AWS ECR                        |
| Web Framework           | Flask (via `app.py`), Jinja2 templates                 |

---

## ⚙️ Prerequisites

- [Conda](https://docs.conda.io/en/latest/)  
- Python 3.10  
- MongoDB Atlas account  
- AWS account (us-east-1)  
- Git & GitHub repository  

---

## 🏗️ Local Setup

1. **Generate project scaffold**  
   ```bash
   python template.py

2. **Configure packaging**

   * Edit `setup.py` & `pyproject.toml` to import local packages.
   * See [crashcourse.txt](crashcourse.txt) for details.
3. **Create & activate Conda env**

   ```bash
   conda create -n vehicle python=3.10 -y
   conda activate vehicle
   ```
4. **Add dependencies** to `requirements.txt`, then install:

   ```bash
   pip install -r requirements.txt
   pip list  # verify local packages
   ```

---

## 🗄️ MongoDB Atlas Setup

1. Sign up on MongoDB Atlas & create a **Project**.
2. **Create a Cluster**: choose **M0 (Free)**, defaults → Deploy.
3. Configure **Database User** with username/password.
4. In **Network Access**, allow `0.0.0.0/0`.
5. **Get Connection String** → Drivers → Python 3.6+ → copy & replace `<password>`.
6. Inside `notebook/`, create `mongoDB_demo.ipynb` (kernel: `vehicle`).
7. Add your dataset to `notebook/` and push to MongoDB from notebook.
8. Verify data in Atlas UI under **Browse Collections**.

---

## 📝 Logging, Exceptions & Notebooks

1. Implement `logger.py`; test via `demo.py`.
2. Implement `exception.py`; test via `demo.py`.
3. Add EDA & Feature Engineering notebooks under `notebook/`.

---

## 🚚 Data Ingestion

1. Define constants in `src/constants/__init__.py`.
2. Configure MongoDB connection in `src/configuration/mongo_db_connections.py`.
3. In `src/data_access/proj1_data.py`: fetch & transform MongoDB data to a DataFrame.
4. Define `DataIngestionConfig` & `DataIngestionArtifact` in `src/entity/`.
5. Implement ingestion logic in `src/components/data_ingestion.py`.
6. Hook into the training pipeline; run `demo.py` (ensure `MONGODB_URL` env var set).

### Environment Variable

```bash
# Bash
export MONGODB_URL="mongodb+srv://<username>:<password>@cluster0.mongodb.net/<db>?retryWrites=true&w=majority"
echo $MONGODB_URL

# PowerShell
$env:MONGODB_URL = "mongodb+srv://<username>:<password>…"
echo $env:MONGODB_URL
```

---

## 🔄 Data Validation, Transformation & Training

1. Fill `src/utils/main_utils.py` & update `config/schema.yaml` for validation rules.
2. Build **Data Validation** component (mirror ingestion workflow).
3. Build **Data Transformation** (add `estimator.py` entity).
4. Build **Model Trainer** (extend `estimator.py` with training classes).

---

## ☁️ AWS & Model Evaluation

1. In AWS console (region **us-east-1**):

   * Create IAM user `firstproj` w/ **AdministratorAccess** → CSV access keys.
2. Set AWS env vars:

   ```bash
   export AWS_ACCESS_KEY_ID="…"
   export AWS_SECRET_ACCESS_KEY="…"
   ```
3. Add to `src/constants/__init__.py`:

   ```python
   MODEL_EVALUATION_CHANGED_THRESHOLD_SCORE: float = 0.02
   MODEL_BUCKET_NAME = "my-model-mlopsproj"
   MODEL_PUSHER_S3_KEY = "model-registry"
   ```
4. Create S3 bucket `my-model-mlopsproj` (us-east-1, public access off).
5. Implement S3 connectors in `src/configuration/aws_connection.py` & `src/aws_storage/s3_estimator.py`.
6. Develop **Model Evaluation** & **Model Pusher** modules.

---

## 🚀 Prediction Pipeline & Web App

1. Scaffold prediction pipeline & `app.py` (Flask).
2. Add `static/` and `template/` directories for front-end assets and HTML.

---

## 🤖 CI/CD & Deployment

1. Write `Dockerfile` & `.dockerignore`.
2. Configure GitHub Actions in `.github/workflows/aws.yaml`.
3. Create IAM user `usvisa-user` for ECR & deployment.
4. Create ECR repository `vehicleproj`; note its URI.
5. Launch EC2 instance (Ubuntu 24.04, t2.medium) with security group open on **5080**.
6. Install Docker on EC2:

   ```bash
   sudo apt-get update -y && sudo apt-get upgrade -y
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh
   sudo usermod -aG docker ubuntu && newgrp docker
   ```
7. Configure **self-hosted runner** on EC2 for GitHub Actions.
8. Add GitHub Secrets under **Settings → Secrets**:

   * `AWS_ACCESS_KEY_ID`
   * `AWS_SECRET_ACCESS_KEY`
   * `AWS_DEFAULT_REGION`
   * `ECR_REPO`
9. On push to `main`, CI/CD builds Docker image, pushes to ECR, and deploys to EC2.
10. Access your app via `http://<EC2_PUBLIC_IP>:5080`.
11. Training endpoint available at `/training`.

---

## 📂 Project Structure

```
vehicle-mlops/
├── notebook/
│   ├── mongoDB_demo.ipynb
│   └── eda_feature_engg.ipynb
├── requirements.txt
├── setup.py
├── pyproject.toml
├── template.py
├── src/
│   ├── constants/
│   │   └── __init__.py
│   ├── configuration/
│   │   ├── mongo_db_connections.py
│   │   └── aws_connection.py
│   ├── data_access/
│   ├── components/
│   ├── entity/
│   ├── utils/
│   └── aws_storage/
├── demo.py
├── app.py
├── Dockerfile
├── .github/
│   └── workflows/aws.yaml
└── .gitignore
```

---

## 🤝 Contributing

1. Fork the repo & create a feature branch
2. Make your changes & add tests
3. Submit a Pull Request

Please adhere to the [Code of Conduct](CODE_OF_CONDUCT.md).

---

## 📝 License

This project is licensed under the **MIT License**. See [LICENSE](LICENSE) for details.

