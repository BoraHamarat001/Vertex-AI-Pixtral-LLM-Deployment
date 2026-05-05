# 02 - VM Setup & Folder Structure

In this step, we prepare a Linux-based development environment and configure all required tools for deployment.

---

## 📌 Overview

All commands should be executed in a **Linux Virtual Machine (VM)** that:

* Has `gcloud` installed
* Has `Docker` installed
* Is authenticated and authorized for your Google Cloud project

---

## 🖥️ Virtual Machine (VM) Specifications

**Execution Context:** Google Cloud Console / Web Browser

Create a VM with the following configuration:

* **Region:** A region that supports A100 GPUs (e.g., `europe-west4`)
* **Machine Type:** E2 series (General Purpose)
* **Disk Size:** 200 GB
* **Image:** Debian GNU/Linux 12 (bookworm)

---

## ⚙️ VM Setup Commands

**Execution Context:** Virtual Machine (VM) – Terminal

SSH into your VM and run the following:

```bash id="jz8n2p"
# System update
sudo apt-get update && sudo apt-get upgrade -y

# Install essential tools
sudo apt-get install -y git git-lfs curl wget gnupg apt-transport-https ca-certificates software-properties-common

# Enable Git LFS
git lfs install

# Install Docker
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Allow Docker usage without sudo
sudo usermod -aG docker $USER

# Install Python and pip
sudo apt-get install -y python3 python3-pip python3-venv

# Configure Google Cloud SDK
gcloud auth login
gcloud config set project llm-development-XXXX # Replace with your Project ID
gcloud config set compute/region europe-west4 # Replace with your region

# Authenticate Docker for Artifact Registry
gcloud auth configure-docker europe-west4-docker.pkg.dev # Replace with your region

# Create an Artifact Registry repository (first time only)
gcloud artifacts repositories create pixtral-models-repo \
  --repository-format=docker \
  --location=europe-west4 \
  --description="Pixtral model containers"

echo "✅ VM setup complete!"
echo "❗ Please type 'exit' to close SSH and reconnect (required for Docker group changes)."
```

---

## 🔄 Important Step

After Docker installation:

1. Exit the SSH session:

```bash id="x3k9qp"
exit
```

2. Reconnect to your VM

This is required for Docker group permissions to take effect.

---

## 📁 Project Folder Setup

After reconnecting:

```bash id="p9v2lt"
mkdir -p pixtral-run
cd pixtral-run
```

---

## ✅ Expected Result

At this point you should have:

* A fully configured Linux VM
* Docker installed and working without `sudo`
* Google Cloud authenticated
* Artifact Registry repository created
* Project directory initialized

---

## ➡️ Next Step

Continue with:

➡️ [03 - Creating Application Files](03-application-files.md)

---

[← Back to README](../README.md)
