# Brandcraft — Claude Adapter

Setup and integration notes specific to Claude (claude.ai and Claude API).

---

## Setup: Claude Projects (Recommended)

1. Go to [claude.ai](https://claude.ai) → **Projects** → create or open a project
2. Click **Add content** → upload `BRAND_CREATIVE.md`
3. The file is now active for every conversation in that project

All conversations in the project will automatically have brand context. No need to paste the file each time.

---

## Setup: API / System Prompt

Pass the full contents of `BRAND_CREATIVE.md` as the system prompt, or prepend it to your first user message:

```python
import anthropic

with open("BRAND_CREATIVE.md", "r") as f:
    skill_content = f.read()

client = anthropic.Anthropic()
response = client.messages.create(
    model="claude-opus-4-5",
    max_tokens=4096,
    system=skill_content,
    messages=[
        {"role": "user", "content": "Make me a Nike Instagram post for a new shoe launch."}
    ]
)
```

---

## Elicitation: Interactive Buttons

Claude supports interactive elicitation via the `ask_user_input_v0` tool on claude.ai. If you want the guided step-by-step flow with clickable buttons, add this line back into Section 1 of `BRAND_CREATIVE.md`:

```
Run these steps in order. Use `ask_user_input_v0` buttons wherever options are listed.
```

The default version in this repo uses plain text lists for cross-tool compatibility.

---

## Downstream Skill Integration

Claude Projects supports chaining multiple skill files. For the best output quality, add these alongside `BRAND_CREATIVE.md` in the same project:

| Output type | Add this skill |
|---|---|
| Presentations (.pptx) | `pptx/SKILL.md` from your skill library |
| Word documents (.docx) | `docx/SKILL.md` |
| PDFs | `pdf/SKILL.md` |
| HTML creatives | `frontend-design/SKILL.md` |

When multiple skills are present, `BRAND_CREATIVE.md` routes to them automatically per the Output Routing Table (Section 3).

---

## File Output

Generated files are saved to `/mnt/user-data/outputs/` and made available for download. The quality checklist in Section 6 confirms file placement before delivery.

---

## Recommended Model

`claude-opus-4-5` or `claude-sonnet-4-5` — Full fidelity brand outputs benefit from stronger instruction-following. Sonnet is the best balance of speed and quality for most creative tasks.
