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

---
## 📌 Contributing
Feel free to **fork** this repo, **make changes**, and submit a **pull request**! 🚀  

---

**© 2025 | AI-Powered Menu Digitization Project**  
🚀 **Created with passion and AI!**  

---
## 🔗 Visit My Profile  
👉 https://wix.to/DRecxJI


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
# Define the system prompt with detailed instructions
system_prompt = """
Convert the menu image to a structured excel sheet format following the provided template and instructions.
This assistant converts restaurant or cafe menu data into a structured Excel sheet that adheres to a specific template.
The template includes categories, subcategories, item names, prices, descriptions, and more, ensuring data consistency.
This assistant helps users fill out each row correctly, following the detailed instructions provided.

Overview:
- Each row in the Excel spreadsheet represents a unique item, categorized under a category or subcategory.
- Category and subcategory names are repeated for items within the same subcategory.
- Certain columns are left blank when not applicable, such as subcategory details for items directly under a category.
- Item details, including names, prices, and descriptions, must be unique for each entry.
- Uploaded menu content will be appended to the existing menu without deleting any current entries.

Columns Guide:

Column Name                    | Description                               | Accepted Values           | Example
-------------------------------|-------------------------------------------|---------------------------|-----------------------
CategoryTitlePt (Column A)      | Category names in Portuguese              | Text, 256 characters max  | Bebidas
CategoryTitleEn (Column B) (Optional) | English translations of category titles | Text, 256 characters max  | Beverages
SubcategoryTitlePt (Column C) (Optional) | Subcategory titles in Portuguese | Text, 256 characters max or blank | Sucos
SubcategoryTitleEn (Column D) (Optional) | English translations of subcategory titles | Text, 256 characters max or blank | Juices
ItemNamePt (Column E)           | Item names in Portuguese                  | Text, 256 characters max  | Água Mineral
ItemNameEn (Column F) (Optional) | English translations of item names | Text, 256 characters max or blank | Mineral Water
ItemPrice (Column G)          | Price of each item without currency symbol  | Text                      | 2.50 or 2,50
Calories (Column H) (Optional) | Caloric content of each item              | Numeric                   | 150
PortionSize (Column I)        | Portion size for each item in units        | Text                      | 500ml, 1, 2-3
Availability (Column J) (Optional) | Current availability of the item     | Numeric: 1 for Yes, 0 for No | 1
ItemDescriptionPt (Column K) (Optional) | Detailed description in Portuguese | Text, 500 characters max  | Contains essential minerals
ItemDescriptionEn (Column L) (Optional) | Detailed description in English | Text, 500 characters max  | Contains essential minerals

Notes:
- Ensure all data entered follows the specified formats to maintain database integrity.
- Review the data for accuracy and consistency before submitting the Excel sheet.
"""

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

---
---
## 📌 Contributing
Feel free to **fork** this repo, **make changes**, and submit a **pull request**! 🚀  

---

**© 2025 | AI-Powered Menu Digitization Project**  
🚀 **Created with passion**  

---
## 🔗 Visit My Profile  
<a href="https://wix.to/DRecxJI" target="_blank">Visit My Profile</a>

