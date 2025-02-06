# AI-Powered-Menu-Digitization
# 🍽️ AI-Powered Menu Digitization
🚀 **Automating restaurant menu digitization using AI to extract and structure data into Excel.**  

---

## 🔥 Project Overview
This project automates the **conversion of restaurant menu images** into structured **Excel files** using OpenAI’s GPT model. It reduces manual data entry, improves accuracy, and accelerates onboarding for restaurant partners.  

---

## 🌟 Why This Project?
✔️ **Eliminates manual data entry**  
✔️ **Ensures accuracy & consistency**  
✔️ **Speeds up restaurant menu onboarding**  

---

## 🖼️ Project Images
**🔝 Top of Website:**  
![Top of Website](https://cdn.midjourney.com/fc7c8a7a-046a-46c5-9fe1-c0c633276a26/0_2.png)

**📜 Restaurant Menu Project:**  
![Menu Project](https://drive.google.com/uc?id=110yTe9nAQ442NMan95uhGF-GzTAjwRI4)

---

## 💻 Code Implementation

```python
# Mount Google Drive to access files
from google.colab import drive
drive.mount('/content/drive')

# Set the directory containing the menu images
directory = '/content/drive/MyDrive/GenAI/OpenAI/OpenAI Project'

# Install OpenAI library quietly
!pip install openai --quiet

# Load libraries
import os
import base64
import pandas as pd
from openai import OpenAI
from IPython.display import Image, display, Markdown

# Set up OpenAI API
openai_api_key = "your_openai_api_key_here"
client = OpenAI(api_key=openai_api_key)

# System prompt for structured Excel conversion
system_prompt = """
Convert menu images to a structured Excel sheet format.
Ensure proper categories, subcategories, and pricing structures.
"""

# Encode images in Base64
def encode_image(image_path):
    with open(image_path, "rb") as image_file:
        return base64.b64encode(image_file.read()).decode('utf-8')

# Process images in directory
image_files = sorted([f for f in os.listdir(directory) if f.endswith(('.png', '.jpg', '.jpeg'))])

# Create DataFrame
df = pd.DataFrame(columns=['CategoryTitlePt', 'ItemNamePt', 'ItemPrice', 'Description'])

for image in image_files:
    image_path = os.path.join(directory, image)
    image_data = encode_image(image_path)

    response = client.chat.completions.create(
        model="gpt-4o",
        messages=[{"role": "system", "content": system_prompt},
                  {"role": "user", "content": [{"type": "text", "text": "Convert this menu image to structured Excel format."},
                                               {"type": "image_url", "image_url": {"url": f"data:image/png;base64,{image_data}"}}]}],
        temperature=0
    )

    extracted_data = response.choices[0].message.content
    df = pd.concat([df, pd.DataFrame([extracted_data])], ignore_index=True)

# Save data to Excel
df.to_excel("/content/menu_data.xlsx", index=False)
print("✅ Excel file saved successfully!")

