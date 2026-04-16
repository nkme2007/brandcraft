# Contributing to Brand Creative Skill

Thank you for helping improve this skill! This document covers the most common contribution types.

---

## Types of Contributions

### 1. Adding a New Brand

Edit the appropriate section in `BRAND_CREATIVE.md` (Section 7) and add an entry in this format:

```
XX-NNN | Company Name | #XXXXXX,#XXXXXX,#XXXXXX | vibe | logo | keyword,keyword,keyword | Tagline here
```

**Field guide:**

| Field | Values |
|---|---|
| Region code | `US` `IN` `CN` `EU` |
| ID | Sequential within region, e.g. `US-101` |
| Colors | `primary,secondary,accent` as hex codes |
| Font vibe | `geo` `hum` `cond` `cser` `mser` `disp` `scr` |
| Logo style | `wm` `lm` `mk` `emb` `cmb` |
| Tone keywords | 3–6 descriptors, comma-separated, no spaces |
| Tagline | Official brand tagline, or `—` if none |

**Before submitting**, verify:
- Colors are taken from the brand's official guidelines or website (not approximated)
- Tagline is current (brands update these — check the official site)
- Tone keywords genuinely reflect the brand's communication style

---

### 2. Fixing an Existing Brand Token

If a brand has updated its colors, tagline, or identity:

1. Find the entry in `BRAND_CREATIVE.md` Section 7
2. Update only the changed fields
3. In your PR description, link to the source confirming the change (brand press release, official style guide, etc.)

---

### 3. Updating Social Media Specs

Social platforms change their format specs regularly. To update Section 9:

1. Link to the official platform spec page in your PR
2. Update the table row(s) with the new dimensions, safe zones, or character limits
3. Update the design principles bullet if behavior has changed

---

### 4. Adding a New Output Type

To add a new output type (e.g., a new document format):

1. Add a row to the Output Routing Table (Section 3)
2. Add a row to the content inputs table in Section 1 (Step 4)
3. Add the file format and downstream skill reference
4. If the output type needs a new downstream skill, document that dependency

---

## Pull Request Guidelines

- **One change per PR** — don't mix brand additions with format spec updates
- **Source your data** — include a link or reference for any factual claim (brand colors, platform specs)
- **Test your formatting** — the brand index uses a strict pipe-delimited format; misalignment breaks parsing
- **Keep it minimal** — this is a single-file skill; avoid bloat

---

## Brand Data Sources

Recommended sources for verifying brand tokens:

- **Official brand/press sites** — most reliable; look for "Brand" or "Press" in the footer
- **Brandfetch** (brandfetch.com) — aggregates brand colors and logos
- **Brand Style Guide** repositories on GitHub
- **Wikipedia** infobox colors for major brands (cross-check with official site)

---

## Reporting Outdated Brand Data

Open an issue with the label `outdated-brand` and include:
- Brand name and region
- What's incorrect (color, tagline, etc.)
- The correct value with source link

---

### 5. Adding a New Tool Adapter

To add support for a new AI tool (e.g. Mistral, Llama, Perplexity):

1. Create `adapters/<toolname>.md`
2. Cover: how to load `BRAND_CREATIVE.md` as context, elicitation adjustments (if any), file output capabilities
3. Note any tool-specific limitations (e.g. context window size, no file generation)
4. Follow the structure of existing adapter files

---

## Code of Conduct

Be constructive. Brand data can be subjective (especially tone keywords) — if you disagree with an existing value, explain your reasoning with evidence rather than just asserting it's wrong.
