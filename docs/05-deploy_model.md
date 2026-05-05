# 05 - Publishing & Deploying the Model

In this step, we register the containerized model in Vertex AI, create an endpoint, and deploy it with GPU acceleration.

---

## 📌 Overview

**Execution Context:** Virtual Machine (VM) – Terminal

We will:

* Upload the model to Vertex AI
* Create an endpoint
* Deploy the model using an **NVIDIA A100 GPU**

---

## ☁️ 1. Register the Model with Vertex AI

We register the Docker container as a Vertex AI Model resource.

```bash id="p4k7ld"
echo "🔄 Uploading model to Vertex AI..."
gcloud ai models upload \
  --region=$REGION \
  --display-name="pixtral-run-v1" \
  --container-image-uri=$IMAGE_URI \
  --container-health-route="/health" \
  --container-predict-route="/predict" \
  --container-ports="8080"
```

---

## 🌐 2. Create an Endpoint

The endpoint provides a stable API URL for inference requests.

```bash id="t8v2qs"
echo "🌐 Creating endpoint..."
gcloud ai endpoints create \
  --region=$REGION \
  --display-name="pixtral-run-endpoint"
```

---

## 🚀 3. Deploy the Model to the Endpoint

This step provisions a **GPU-powered instance (A100)** and deploys the model.

> ⏳ This process may take **10–25 minutes**.

### 🔹 Get Model and Endpoint IDs

```bash id="m2x9qp"
export MODEL_ID=$(gcloud ai models list --region=$REGION --filter="displayName=pixtral-run-v1" --format="value(name)" | head -1)
export ENDPOINT_ID=$(gcloud ai endpoints list --region=$REGION --filter="displayName=pixtral-run-endpoint" --format="value(name)" | head -1)
```

---

### 🔹 Deploy Model

```bash id="v3n8rk"
echo "🚀 Deploying model to endpoint... (Please wait 10–25 minutes)"

gcloud ai endpoints deploy-model $ENDPOINT_ID \
  --region=$REGION \
  --model=$MODEL_ID \
  --display-name="pixtral-run-deployment" \
  --machine-type="a2-highgpu-1g" \
  --accelerator="type=nvidia-tesla-a100,count=1" \
  --min-replica-count=1 \
  --max-replica-count=2
```

---

## 📌 Notes

* `max-replica-count=2` enables scaling if needed
* You can set it to `1` for a single-instance deployment
* Ensure A100 quota is approved before deployment

---

## ✅ Expected Result

After deployment:

* Vertex AI endpoint is active
* Model is running on an A100 GPU
* REST API is available for inference

---

## ➡️ Next Step

Continue with:

➡️ [06 - Testing & Verification](06-testing.md)

---

[← Back to README](../README.md)
