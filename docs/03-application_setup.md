# 03 - Creating Application Files

In this step, we create the core application files required to serve the model and prepare it for containerization.

---

## 📌 Overview

**Execution Context:** Virtual Machine (VM) – Terminal

We will create:

* A prediction server (`predictor.py`)
* A dependency file (`requirements.txt`)
* A container definition (`Dockerfile`)

---

## 🧠 1. Create the Prediction Server (`predictor.py`)

This file acts as the **core logic** of the application:

* Loads the model from Hugging Face
* Accepts incoming HTTP requests
* Processes images and prompts
* Returns predictions

Run the following command:

```bash id="r4k8zp"
cat > predictor.py << 'PRED_EOF'
# predictor.py (V9 - Max 10 Images & Structure Fix)
import os
import base64
import io
from flask import Flask, request, jsonify
from transformers import LlavaForConditionalGeneration, AutoProcessor
from PIL import Image
import torch

app = Flask(__name__)

# --- Model Loading ---
MODEL_ID = "mistral-community/pixtral-12b"
print(f"🚀 Starting server... Model: {MODEL_ID}")
processor = AutoProcessor.from_pretrained(MODEL_ID)
model = LlavaForConditionalGeneration.from_pretrained(
    MODEL_ID, torch_dtype=torch.bfloat16, device_map="auto"
)
print("✅ Server is ready!")

@app.route('/health', methods=['GET'])
def health():
    return '', 200

@app.route('/predict', methods=['POST'])
def predict():
    try:
        data = request.get_json()
        instances = data.get("instances", [])
        predictions = []

        for inst in instances:
            # Parse prompt and parameters
            prompt = inst.get("prompt", "Describe this image.")
            params = inst.get("parameters", {})
            temperature = float(params.get("temperature", 0.2))
            max_new_tokens = int(params.get("maxOutputTokens", 512))
            do_sample = temperature > 0.0

            # Collect images
            image_b64_list = []
            if 'images' in inst and isinstance(inst['images'], list):
                image_b64_list = inst['images']
            elif 'image' in inst:
                image_b64_list.append(inst['image'])

            # Security check
            if len(image_b64_list) > 10:
                return jsonify({"error": f"Too many images. Max allowed is 10, you sent {len(image_b64_list)}."}), 400
            
            if not image_b64_list:
                predictions.append({"error": "At least one image is required"})
                continue

            # Convert images
            pil_images = []
            try:
                for img_data in image_b64_list:
                    pil_images.append(Image.open(io.BytesIO(base64.b64decode(img_data))).convert('RGB'))
            except Exception as img_err:
                return jsonify({"error": f"Invalid image data: {str(img_err)}"}), 400

            # Build structured input
            content_list = []
            for _ in pil_images:
                content_list.append({"type": "image"})
            
            clean_prompt = prompt.replace("<image>", "").strip()
            content_list.append({"type": "text", "text": clean_prompt})

            messages = [{"role": "user", "content": content_list}]
            
            prompt_text = processor.apply_chat_template(messages, add_generation_prompt=True)
            inputs = processor(
                text=prompt_text,
                images=pil_images,
                return_tensors="pt"
            ).to(model.device, torch.bfloat16)
            
            # Run model
            with torch.no_grad():
                outputs = model.generate(
                    **inputs,
                    max_new_tokens=max_new_tokens,
                    temperature=temperature,
                    do_sample=do_sample
                )
            
            result = processor.decode(outputs[0], skip_special_tokens=True)

            # Clean response
            if "ASSISTANT:" in result:
                final_response = result.split("ASSISTANT:")[-1].strip()
            else:
                final_response = result

            predictions.append({"generated_text": final_response})
        
        return jsonify({"predictions": predictions})

    except Exception as e:
        print(f"CRITICAL ERROR: {str(e)}")
        return jsonify({"error": str(e)}), 500

if __name__ == '__main__':
    port = int(os.environ.get('AIP_HTTP_PORT', 8080))
    app.run(host='0.0.0.0', port=port)
PRED_EOF

echo "✅ predictor.py file created successfully."
```

---

## 📦 2. Define Dependencies (`requirements.txt`)

Create the required Python dependencies:

```bash id="2w9lqm"
cat > requirements.txt << 'REQ_EOF'
flask
transformers
accelerate
Pillow
sentencepiece
protobuf
REQ_EOF

echo "✅ requirements.txt file created successfully."
```

---

## 🐳 3. Create Dockerfile

Define the container image:

```bash id="l8x3nv"
cat > Dockerfile << 'EOF'
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY predictor.py .

EXPOSE 8080

CMD ["python3", "predictor.py"]
EOF

echo "✅ Dockerfile created successfully."
```

---

## ✅ Expected Result

You should now have the following files in your project directory:

```bash id="h3k91p"
pixtral-run/
├── predictor.py
├── requirements.txt
└── Dockerfile
```

---

## ➡️ Next Step

Continue with:

➡️ [04 - Building the Container](04-container.md)

---

[← Back to README](../README.md)
