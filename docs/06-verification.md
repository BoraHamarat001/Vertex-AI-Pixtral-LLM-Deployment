# 06 - Testing & Verification

This section covers **access control setup** and **verification of the deployed Vertex AI model** using a custom test script.

---

## 📌 Overview

We will:

* Assign required IAM roles to users
* Run a local test script to verify the deployed model
* Send image-based inference requests to the endpoint

---

## 🔐 1. Authorizing Users (Administrator Step)

**Execution Context:** Google Cloud Shell or Local Terminal (Admin Access)

To allow access to Vertex AI, assign the required IAM role.

---

### 👨‍💻 Add a New Developer

```bash id="a8k2qp"
gcloud projects add-iam-policy-binding llm-development-XXXX \
    --member="user:[DEVELOPER_EMAIL_ADDRESS]" \
    --role="roles/aiplatform.user"
```

---

### 🧾 Add a Service Account

```bash id="c4v9tr"
gcloud projects add-iam-policy-binding llm-development-XXXX \
    --member="serviceAccount:[SERVICE_ACCOUNT_EMAIL]" \
    --role="roles/aiplatform.user"
```

---

## 🧪 2. Testing the Model (Developer Step)

**Execution Context:** Local Machine (Developer)

This step verifies that the deployed endpoint is working correctly.

---

## 📦 Prerequisites

Ensure the following:

* Python 3 installed
* Vertex AI SDK installed:

```bash id="p2x7ld"
pip install google-cloud-aiplatform
```

* Authentication completed:

```bash id="m9q3rk"
gcloud auth application-default login
```

* Test images available in the working directory:

```bash id="t7v2qn"
cat.jpg
dog.jpg
car.jpg
```

---

## 🧠 3. Create Test Script (`test.py`)

Update:

* `PROJECT_ID`
* `REGION`
* `ENDPOINT_ID`

```python id="x3n8qp"
# test.py (V9 - Structured Model Test)
import base64
import os
from google.cloud import aiplatform

PROJECT_ID = "llm-development-XXXX"
REGION = "europe-west4"
ENDPOINT_ID = "[YOUR_ENDPOINT_ID]"

TEST_SCENARIO_TO_RUN = 1

PROMPT_1 = "What is in this image? Describe it in detail."
IMAGE_FILES_1 = ["cat.jpg"]
PARAMETERS_1 = {"temperature": 0.2, "maxOutputTokens": 1024}

PROMPT_2 = "Compare these two images."
IMAGE_FILES_2 = ["cat.jpg", "dog.jpg"]
PARAMETERS_2 = {"temperature": 0.5, "maxOutputTokens": 1024}


def run_test(prompt: str, image_paths: list, parameters: dict):
    print(f"--- Testing with {len(image_paths)} image(s)... ---")

    aiplatform.init(project=PROJECT_ID, location=REGION)
    endpoint = aiplatform.Endpoint(endpoint_name=ENDPOINT_ID)

    encoded_images = []
    for path in image_paths:
        if not os.path.exists(path):
            print(f"❌ ERROR: '{path}' not found.")
            return
        with open(path, "rb") as image_file:
            encoded_images.append(base64.b64encode(image_file.read()).decode("utf-8"))

    instance = {
        "prompt": prompt,
        "parameters": parameters
    }

    if len(encoded_images) == 1:
        instance["image"] = encoded_images[0]
    else:
        instance["images"] = encoded_images

    try:
        print("Sending request...")
        response = endpoint.predict(instances=[instance])

        full_text = response.predictions[0].get('generated_text', '')
        print("\n🎉 MODEL RESPONSE:")
        print(full_text)

    except Exception as e:
        print(f"\n❌ ERROR: {e}")


if __name__ == "__main__":
    if TEST_SCENARIO_TO_RUN == 1:
        run_test(PROMPT_1, IMAGE_FILES_1, PARAMETERS_1)
    elif TEST_SCENARIO_TO_RUN == 2:
        run_test(PROMPT_2, IMAGE_FILES_2, PARAMETERS_2)
```

---

## ▶️ 4. Run the Test

```bash id="q1v8lk"
python test.py
```

---

## ✅ Expected Result

If everything is configured correctly:

* Request is sent to Vertex AI endpoint
* Model processes image(s)
* Response is printed in terminal

Example output:

```
🎉 MODEL RESPONSE:
A detailed description of the image...
```

---

## ➡️ Next Step

Continue with:

➡️ [07 - Monitoring](07-monitoring.md)

---

[← Back to README](../README.md)
