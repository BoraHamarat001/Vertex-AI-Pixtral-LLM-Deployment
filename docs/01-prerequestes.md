# 01 - Prerequisites

Before starting the deployment, ensure your Google Cloud environment is properly configured.

---

## 📌 Note

**Execution Context:** Google Cloud Console / Web Browser

The following requirements must be completed before proceeding:

* Required APIs must be enabled
* At least **one A100 GPU quota** must be approved for Vertex AI

Required APIs:

* Vertex AI API
* Artifact Registry API
* Compute Engine API

---

## Enable Required APIs

1. Open the **Google Cloud Console**.
2. In the search bar, type:

```id="a9u4m2"
APIs & Services
```

3. From the left panel, click **Library**.
4. Search and enable the following APIs one by one:

* Vertex AI API
* Artifact Registry API
* Compute Engine API

5. Open each API page and click **Enable**.

---

## Configure A100 GPU Quota

1. In the Google Cloud Console search bar, search for:

```id="mq4t7r"
IAM & Admin
```

2. Open **Quotas & System Limits**.

3. Apply the following filters:

* **Service:** `Vertex AI API`
* **Metric:** `A100`
* **Region:** `europe-west4`

> If you plan to use another GPU or region, adjust the filters accordingly.

---

## Expected Result

You should have:

* Required APIs enabled
* At least **1 approved A100 GPU quota** for Vertex AI

---

## ✅ Next Step

Continue with:

➡️ [02 - VM Setup & Folder Structure](02-vm-setup.md)

---

[← Back to README](../README.md)
