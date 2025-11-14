## Prototype Development for Image Captioning Using the BLIP Model and Gradio Framework

### AIM:
To design and deploy a prototype application for image captioning by utilizing the BLIP image-captioning model and integrating it with the Gradio UI framework for user interaction and evaluation.

### PROBLEM STATEMENT:
Build and deploy a working prototype web app that uses the BLIP image-captioning model to generate accurate, human-like captions for user images, exposes the functionality through a Gradio UI for interactive testing, and provides quantitative and qualitative evaluation of caption quality.

### DESIGN STEPS:

#### STEP 1:
Develop an image-captioning backend Implement a working pipeline using the BLIP model to take an input image and generate a meaningful caption.
#### STEP 2:
Build an interactive UI for testing Integrate this BLIP captioning pipeline with a Gradio interface so users can upload images and instantly see the generated captions.
#### STEP 3:
Deploy and evaluate the prototype
Run the app as a deployment-ready prototype, allowing real users to interact with it and evaluate caption quality for accuracy and usefulness

### PROGRAM:
```python
import os
import io
import IPython.display
from PIL import Image
import base64 
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
hf_api_key = os.environ['HF_API_KEY']
```



```python

import requests, json

#Image-to-text endpoint
def get_completion(inputs, parameters=None, ENDPOINT_URL=os.environ['HF_API_ITT_BASE']):
    headers = {
      "Authorization": f"Bearer {hf_api_key}",
      "Content-Type": "application/json"
    }
    data = { "inputs": inputs }
    if parameters is not None:
        data.update({"parameters": parameters})
    response = requests.request("POST",
                                ENDPOINT_URL,
                                headers=headers,
                                data=json.dumps(data))
    return json.loads(response.content.decode("utf-8"))
```

```python
image_url = "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT1AKF7LelsXtbK8YAYYdiPrDMZdFd74ZTgkQ&s"
display(IPython.display.Image(url=image_url))
get_completion(image_url)
```

<img width="865" height="288" alt="image" src="https://github.com/user-attachments/assets/eb7f9cda-40a9-41a8-9a14-22c508577884" />



```python
import gradio as gr 

def image_to_base64_str(pil_image):
    byte_arr = io.BytesIO()
    pil_image.save(byte_arr, format='PNG')
    byte_arr = byte_arr.getvalue()
    return str(base64.b64encode(byte_arr).decode('utf-8'))

def captioner(image):
    base64_image = image_to_base64_str(image)
    result = get_completion(base64_image)
    return result[0]['generated_text']

gr.close_all()
demo = gr.Interface(fn=captioner,
                    inputs=[gr.Image(label="Upload image", type="pil")],
                    outputs=[gr.Textbox(label="Caption")],
                    title="Image Captioning with BLIP",
                    description="Caption any image using the BLIP model",
                    allow_flagging="never",
                    examples=["christmas_dog.jpeg", "bird_flight.jpeg", "cow.jpeg"])

demo.launch(share=True, server_port=int(os.environ['PORT1']))
```
### OUTPUT:
<img width="915" height="732" alt="Screenshot 2025-11-14 112648" src="https://github.com/user-attachments/assets/55590646-4ac0-4ccd-8bb5-7fac35454482" />

<br>
### RESULT:
Thus,designing and deploying a prototype application for image captioning by utilizing the BLIP image-captioning model and integrating it with the Gradio UI framework for user interaction and evaluation have been done successfully.

