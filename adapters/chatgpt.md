# Brandcraft — ChatGPT / OpenAI Adapter

Setup and integration notes for ChatGPT and the OpenAI API.

---

## Setup: Custom GPT (Recommended for ChatGPT)

1. Go to [chat.openai.com](https://chat.openai.com) → **Explore GPTs** → **Create**
2. In the **Instructions** field, paste the full contents of `BRAND_CREATIVE.md`
3. Name the GPT something like "Brand Creative Assistant"
4. Save and publish (to yourself or your team)

The Custom GPT will now apply brand tokens to every conversation.

---

## Setup: OpenAI API

```python
from openai import OpenAI

with open("BRAND_CREATIVE.md", "r") as f:
    skill_content = f.read()

client = OpenAI()
response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": skill_content},
        {"role": "user", "content": "Make me a Nike Instagram post for a new shoe launch."}
    ]
)
```

---

## Setup: ChatGPT with a File

In a standard ChatGPT conversation, upload `BRAND_CREATIVE.md` as a file and say:

> "I've attached a brand creative specification. Use it as your guide for everything in this conversation."

Note: this only applies to the current conversation — use a Custom GPT for persistence.

---

## Elicitation

ChatGPT doesn't have native interactive button UI. The plain-text numbered list elicitation in the default `BRAND_CREATIVE.md` works well. For best results, add this line to the system prompt:

> "Present all elicitation options as a numbered list and wait for the user to reply before proceeding."

---

## File Output

ChatGPT / API does not natively generate .pptx or .docx files. Workarounds:

- **HTML output** → ChatGPT can generate HTML code that you paste into a browser
- **Presentations** → Generate a structured outline; use the content in Google Slides or PowerPoint manually
- **Python generation** → Ask GPT-4 to write a `python-pptx` or `python-docx` script for you to run locally

---

## Recommended Model

`gpt-4o` for Full fidelity tasks. `gpt-4o-mini` is sufficient for Lite fidelity social creatives.
