# 07 - Monitoring

This section describes how to monitor the deployed **Pixtral-12B-2409 model** on Vertex AI using Google Cloud Monitoring dashboards.

---

## 📌 Overview

**Execution Context:** Google Cloud Console / Web Browser

The monitoring dashboard provides real-time insights into:

* Resource usage
* System performance
* Network activity
* Prediction behavior

---

## 📊 Pixtral Model Monitoring Dashboard (see screenshot below)

This dashboard is used to monitor the operational state of the Pixtral-12B-2409 model deployed on Vertex AI.

It enables real-time tracking of:

* CPU and GPU utilization
* Memory consumption
* Network traffic
* Prediction latency and volume

---

## 📈 1. Tracked Metrics

| Metric Name                   | Source          | Description                   | Importance                                        |
| ----------------------------- | --------------- | ----------------------------- | ------------------------------------------------- |
| CPU Utilization               | Vertex Endpoint | Percentage of CPU usage       | High usage may indicate compute bottlenecks       |
| Accelerator Memory Usage      | GPU             | GPU memory consumption        | Prevents OOM (Out-Of-Memory) issues               |
| Memory Usage                  | Vertex Endpoint | System RAM usage              | Detects memory leaks or inefficiencies            |
| Network Bytes Sent / Received | Vertex Endpoint | Data transfer volume          | Helps identify traffic patterns and scaling needs |
| Prediction Latency (p95)      | Vertex Endpoint | 95th percentile response time | High latency indicates performance degradation    |
| Prediction Count              | Vertex Endpoint | Number of inference requests  | Used for usage and autoscaling analysis           |

---

## 📋 2. Dashboard Details

* **Dashboard Name:** Pixtral-Model-Monitoring
* **Project ID:** llm-development-XXXX
* **Created On:** November 3, 2025
* **Update Interval:** Every 1 minute (default)
* **Data Retention:** 6 weeks (Google Cloud default)

---

## 🌐 3. Accessing the Dashboard

### Step 1 — Sign in to Google Cloud Console

Go to:
👉 https://console.cloud.google.com

* Sign in with your Google Cloud account
* Ensure the correct project is selected:
  `llm-development-XXXX`

---

### Step 2 — Open Monitoring

Navigate to:

```text
Monitoring → Dashboards
```

You will see all available dashboards in the project.

---

### Step 3 — Locate Pixtral Dashboard

Find and open:

```text
Pixtral-Model-Monitoring
```

Click to open the dashboard.

---

### Step 4 — Analyze Metrics

Once opened, you can:

* 📊 Monitor CPU, GPU, and memory usage in real time
* 🌐 Track network traffic trends
* ⚡ Observe prediction latency changes
* 📅 Adjust time range (Last 1 hour, 24 hours, etc.)
* 🔍 Hover graphs to inspect exact values

---

## 📌 Notes

* Metrics update approximately every **1 minute**
* Data is stored for **6 weeks by default**
* This dashboard is essential for detecting:

  * Performance bottlenecks
  * GPU saturation
  * Scaling needs

---

## 🏁 Project Complete

You have successfully completed:

* Environment setup
* Model containerization
* Vertex AI deployment
* Testing & verification
* Monitoring setup

---

[← Back to README](../README.md)
