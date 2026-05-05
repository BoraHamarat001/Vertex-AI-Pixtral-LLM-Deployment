# 04 - Building the Container

In this step, we package the application into a Docker container and push it to Google Artifact Registry.

---

## 📌 Overview

**Execution Context:** Virtual Machine (VM) – Terminal

We will:

* Define environment variables
* Build the Docker image
* Push the image to Artifact Registry

---

## ⚙️ 1. Set Environment Variables

To simplify commands, define the following variables:

```bash id="k2m8xp"
export PROJECT_ID="llm-development-XXXX"   # Replace with your project ID
export REGION="europe-west4"               # Replace with your region
export REPO_NAME="pixtral-models-repo"     # Artifact Registry repository name
export IMAGE_NAME="pixtral-run"            # Docker image name
export IMAGE_URI="${REGION}-docker.pkg.dev/${PROJECT_ID}/${REPO_NAME}/${IMAGE_NAME}:v1"
```

---

## 🐳 2. Build the Docker Image

Use the Dockerfile created in the previous step:

```bash id="n9q3ls"
echo "🔨 Building Docker image ($IMAGE_NAME:v1)..."
docker build -t $IMAGE_URI .
```

---

## 📤 3. Push Image to Artifact Registry

```bash id="v7x2rd"
echo "📤 Pushing image to Artifact Registry..."
docker push $IMAGE_URI
```

---

## ✅ Expected Result

After completion:

* Your Docker image is successfully built
* The image is available in **Artifact Registry**
* You can reference it using:

```bash id="c4z8yt"
$IMAGE_URI
```

---

## ➡️ Next Step

Continue with:

➡️ [05 - Publishing & Deploying the Model](05-deployment.md)

---

[← Back to README](../README.md)
