# Brandcraft — Gemini Adapter

Setup and integration notes for Google Gemini and the Gemini API.

---

## Setup: Gemini Gems (Recommended)

1. Go to [gemini.google.com](https://gemini.google.com) → **Gems** → **New Gem**
2. In the **Instructions** field, paste the full contents of `BRAND_CREATIVE.md`
3. Name the Gem "Brand Creative" and save

The Gem will apply brand tokens to every conversation automatically.

---

## Setup: Gemini API

```python
import google.generativeai as genai

with open("BRAND_CREATIVE.md", "r") as f:
    skill_content = f.read()

genai.configure(api_key="YOUR_API_KEY")
model = genai.GenerativeModel(
    model_name="gemini-1.5-pro",
    system_instruction=skill_content
)

chat = model.start_chat()
response = chat.send_message("Make me a Nike Instagram post for a new shoe launch.")
```

---

## Setup: Gemini in Google Workspace

If you have access to Gemini for Workspace (Docs, Slides, Gmail):

- **Google Slides** → Paste brand token values directly into your prompt: "Create a 5-slide Nike presentation using primary color #000000, accent #F5821F, condensed bold type, bold athletic tone."
- **Google Docs** → Attach `BRAND_CREATIVE.md` in a Drive folder and reference it in your prompt with `@filename`

---

## Elicitation

Gemini works well with plain numbered list elicitation. No adjustments needed to `BRAND_CREATIVE.md`.

---

## File Output

- **Gemini Advanced** can generate structured content; export to Google Docs/Slides natively
- **Gemini API** returns text; use with Google Slides API or Apps Script for file generation
- For .pptx output, generate the content structure and build with `python-pptx` or Google Slides API

---

## Recommended Model

`gemini-1.5-pro` for Full fidelity tasks. `gemini-1.5-flash` for Lite fidelity and faster iteration.
