# Prototype Development for Image Generation Using the Stable Diffusion Model and Gradio Framework

## AIM:
To design and deploy a prototype application for image generation utilizing the Stable Diffusion model, integrated with the Gradio UI framework for interactive user engagement and evaluation.

---

## PROBLEM STATEMENT:
Text-to-image synthesis requires seamless bridging between complex deep learning diffusion models and end-users. Generating high-resolution, visually accurate images from natural language descriptions often demands substantial local computing resources (GPUs) and specialized technical knowledge. 

This project aims to solve this challenge by developing an accessible, cloud-backed interactive prototype leveraging Hugging Face's serverless Inference API with the Stable Diffusion model (SDXL) and an intuitive Gradio web interface for real-time image synthesis and evaluation.

---

## DESIGN STEPS:

### STEP 1: Environment Setup and Authentication
- Install and configure required Python dependencies (`gradio`, `huggingface_hub`, `Pillow`, `python-dotenv`).
- Set up authentication by obtaining and configuring a Hugging Face API access token with Inference permissions.

### STEP 2: Model Integration & Inference Function
- Initialize the `InferenceClient` from `huggingface_hub` to communicate with the cloud-hosted Stable Diffusion endpoint (`stabilityai/stable-diffusion-xl-base-1.0`).
- Define the prediction/completion function `get_completion(prompt)` that takes a text prompt and returns the synthesized `PIL.Image`.

### STEP 3: Gradio User Interface Construction
- Build an interactive web UI using `gr.Interface` with:
  - Text input component (`gr.Textbox`) for user prompts.
  - Image output component (`gr.Image`) for displaying generated visual outputs.
  - Sample prompts (`examples`) for quick testing and demonstration.
  - Set `flagging_mode="never"` for clean execution.

### STEP 4: Application Deployment & Verification
- Launch the Gradio application with `demo.launch(share=True)` to generate both local and public shareable URLs.
- Test the application with diverse natural language descriptions to verify rendering quality and latency.

---

## PROGRAM:

```python
# ==============================================================================
# Prototype Development for Image Generation Using Stable Diffusion & Gradio
# ==============================================================================

import os
import io
from PIL import Image
from dotenv import load_dotenv
from huggingface_hub import InferenceClient
import gradio as gr

# Step 1: Load environment variables and authenticate
load_dotenv()
hf_api_key = os.environ.get("HF_API_KEY", "your_huggingface_token_here")

# Initialize Hugging Face Inference Client
client = InferenceClient(token=hf_api_key)

# Step 2: Define image generation inference function
def get_completion(prompt, model="stabilityai/stable-diffusion-xl-base-1.0"):
    """Generates a PIL Image from a text prompt using Hugging Face Inference API."""
    image = client.text_to_image(prompt, model=model)
    return image

# Helper wrapper for Gradio
def generate(prompt):
    return get_completion(prompt)

# Step 3: Build interactive Gradio User Interface
gr.close_all()

demo = gr.Interface(
    fn=generate,
    inputs=[gr.Textbox(label="Your prompt", placeholder="Enter a prompt to generate an image...")],
    outputs=[gr.Image(label="Result", type="pil")],
    title="Image Generation with Stable Diffusion XL (SDXL)",
    description="Generate high-resolution images from natural language descriptions using the Stable Diffusion model and Gradio.",
    flagging_mode="never",
    examples=[
        "the spirit of a tamagotchi wandering in the city of Vienna",
        "a mecha robot in a favela",
        "a futuristic cyberpunk city at night with neon lights",
        "a cute golden retriever puppy playing in a park, photorealistic 4k"
    ]
)

# Step 4: Launch the Gradio app
if __name__ == "__main__":
    demo.launch(share=True)
```

---

## OUTPUT:

### 1. Test Prompt Execution:
- **Prompt:** `"a cute golden retriever puppy playing in a park, photorealistic 4k"`
- **Result:** Successfully synthesized 1024x1024 photorealistic image.

### 2. Gradio Interactive Interface:
- **Local URL:** `http://127.0.0.1:7860/`
- **Public Shareable Link:** `https://<generated-hash>.gradio.live`


---
<img width="1596" height="804" alt="image" src="https://github.com/user-attachments/assets/48222f1b-a7e1-4a7c-bb09-39ae5aa1c829" />

## RESULT:
The prototype application for text-to-image generation utilizing the Stable Diffusion model and the Gradio framework was successfully developed, tested, and deployed with an intuitive web interface for real-time user interaction.
