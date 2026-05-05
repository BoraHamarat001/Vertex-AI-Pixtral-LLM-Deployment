# 🚀 Pixtral 12B Deployment on Vertex AI

This repository provides a detailed, step-by-step guide for deploying the `mistral-community/pixtral-12b` model from scratch using **Google Cloud Vertex AI**.

The deployment is performed under the **deployment project**, within the **europe-west4** region, following the **pixtral-run** naming convention.

This guide is designed to be followed sequentially—similar to *Kubernetes The Hard Way*—so that you understand every step of the deployment process.

---

## 🎯 Target Audience

This tutorial is intended for:

* Engineers
* ML practitioners
* Developers

who want to understand the fundamentals of deploying **Large Language Models (LLMs)** on Vertex AI using:

* Custom containers
* GPU infrastructure

---

## 📚 Labs

This tutorial is broken down into the following sections:

1. [Prerequisites](docs/01-prerequisites.md)
2. [VM Setup & Folder Structure](docs/02-vm_setup.md)
3. [Creating Application Files](docs/03-application_setup.md)
4. [Building the Container](docs/04-container_build.md)
5. [Publishing & Deploying the Model](docs/05-deploy_model.md)
6. [Testing & Verification](docs/06-verification.md)
7. [Monitoring](docs/07-monitoring.md)

---

## 🧠 Deployment Overview

We will be deploying the `mistral-community/pixtral-12b` model.

The deployment process includes:

* Setting up a **GPU-enabled VM** for development
* Creating a **Python Flask application** (`predictor.py`) to serve the model
* Containerizing the application using **Docker**
* Pushing the container to **Google Artifact Registry**
* Deploying the container to a **Vertex AI Endpoint** with **A100 GPU acceleration**
* Verifying the deployment with a custom test script
* Monitoring the deployment via **Google Cloud Monitoring**

---

## 🏗️ Project Structure

```bash
.
├── README.md
└── docs/
    ├── prerequisites.md
    ├── vm-setup.md
    ├── application-files.md
    ├── container.md
    ├── deployment.md
    ├── testing.md
    └── monitoring.md
```

---

## 📌 Notes

* All steps are explained in detail inside the `docs/` directory.
* Each lab builds on the previous one—follow them in order.
* The guide focuses on **understanding the process**, not just executing commands.

---

## 📄 License

This project is for educational and demonstration purposes.

---

*Generated for the Pixtral 12B Deployment Project.*
