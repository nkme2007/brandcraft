# Brandcraft — Microsoft Copilot Adapter

Setup and integration notes for Microsoft Copilot and Azure OpenAI Service.

---

## Setup: Copilot Studio (Recommended for enterprise)

1. Go to [copilotstudio.microsoft.com](https://copilotstudio.microsoft.com)
2. Create a new Copilot → **Configure** → **Instructions**
3. Paste the contents of `BRAND_CREATIVE.md` into the system instructions
4. Add a knowledge source or upload the file to the Copilot's knowledge base

This makes the brand index available to everyone in your Microsoft 365 tenant.

---

## Setup: Microsoft 365 Copilot

In Word, PowerPoint, or Teams, you can reference `BRAND_CREATIVE.md` from SharePoint/OneDrive:

1. Upload `BRAND_CREATIVE.md` to a SharePoint document library
2. In a Copilot prompt, reference it: "Using the brand guidelines in [link to file], create a Nike-branded PowerPoint presentation."

---

## Setup: Azure OpenAI API

```python
from openai import AzureOpenAI

with open("BRAND_CREATIVE.md", "r") as f:
    skill_content = f.read()

client = AzureOpenAI(
    azure_endpoint="https://YOUR_RESOURCE.openai.azure.com/",
    api_key="YOUR_KEY",
    api_version="2024-02-01"
)

response = client.chat.completions.create(
    model="gpt-4o",  # your deployment name
    messages=[
        {"role": "system", "content": skill_content},
        {"role": "user", "content": "Make me a Nike Instagram post for a new shoe launch."}
    ]
)
```

---

## Native Integration Advantage

Copilot in PowerPoint and Word is the strongest integration path for .pptx and .docx output — it can generate and edit files natively. Brand tokens map directly to PowerPoint theme colors and Word styles.

**PowerPoint example prompt:**
> "Create a 5-slide presentation for a Nike product launch. Use black (#000000) as the primary background, orange (#F5821F) as the accent color, bold condensed headlines in ALL CAPS, and an athletic, empowering tone."

---

## Recommended Model

GPT-4o via Azure OpenAI or native Copilot in Microsoft 365 for the best .pptx/.docx fidelity.
