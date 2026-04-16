# 🎨 Brandcraft

A single-file brand intelligence layer for AI assistants. Drop `BRAND_CREATIVE.md` into any AI tool and it gains complete brand awareness — applying the right colors, typography, tone, and layout for **300 global companies** across the US, India, China, and Europe.

Works with any AI assistant that accepts a system prompt or file context.

---

## ✨ What It Does

| Capability | Details |
|---|---|
| **Brand-accurate output** | Colors, fonts, tone, layout, logo treatment — all from an embedded token schema |
| **300 preset brands** | US · India · China · Europe — see [Brand Coverage](#-brand-coverage) |
| **9 social platforms** | Exact pixel dimensions & safe zones for Instagram, LinkedIn, X, Facebook, YouTube, TikTok, Pinterest, WhatsApp, Google Business |
| **Co-branding** | Merges two brands into a single coherent visual identity |
| **Custom brands** | Full intake flow for brands not in the index |
| **All output types** | HTML, PPTX, DOCX, PDF, social creatives, email templates, invoices, event creatives |
| **Two fidelity modes** | Lite (colors + tone) or Full (fonts, layout, imagery direction, border radius) |

---

## 🚀 Quick Start

Add `BRAND_CREATIVE.md` as context in your AI tool of choice, then start prompting:

```
Make me a Nike Instagram post announcing a new running shoe drop.
```

```
Create a co-branded HTML landing page for a Netflix × Spotify partnership campaign.
```

```
Design an Apple-style product launch presentation — 5 slides.
```

```
Write a formal letter on Tata Consultancy Services letterhead to a new enterprise client.
```

```
My brand: primary color #6B21A8, modern serif font, luxury, minimal, fashion.
Make me a LinkedIn carousel for a new collection launch.
```

See [examples/](examples/) for full walkthroughs.

---

## 🛠 Tool-Specific Setup

| Tool | How to add context | Native file output |
|---|---|---|
| **Claude** | Upload to a Project as a knowledge file | ✅ PPTX, DOCX, PDF, HTML |
| **ChatGPT** | Custom GPT instructions, or upload file per conversation | HTML only (code) |
| **Gemini** | Gemini Gem instructions, or `@file` in Workspace | Google Slides/Docs |
| **Copilot** | Copilot Studio instructions, or SharePoint reference | ✅ PPTX, DOCX (native) |
| **Any LLM API** | System prompt field | Depends on implementation |

See the [`adapters/`](adapters/) folder for step-by-step setup guides per tool.

---

## 📁 Repository Structure

```
brandcraft/
├── BRAND_CREATIVE.md         ← Add this file to your AI tool
├── README.md                 ← You are here
├── CHANGELOG.md
├── LICENSE
├── adapters/
│   ├── claude.md             ← Claude Projects + API setup
│   ├── chatgpt.md            ← Custom GPT + OpenAI API setup
│   ├── gemini.md             ← Gemini Gems + API setup
│   └── copilot.md            ← Copilot Studio + Azure setup
├── docs/
│   ├── BRAND_TOKEN_SCHEMA.md ← Token field reference
│   └── SOCIAL_FORMATS.md     ← Social media specs quick reference
└── examples/
    ├── nike-instagram.md
    └── cobranding-guide.md
```

---

## 🌍 Brand Coverage

### United States (100 brands)
Apple · Microsoft · Amazon · Google · Meta · NVIDIA · Tesla · Nike · Netflix · Adobe · Airbnb · Uber · Starbucks · PayPal · Salesforce · Disney · Cisco · Goldman Sachs · JPMorgan · Visa · Mastercard · Pfizer · Johnson & Johnson · Walmart · Costco · and 75 more.

### India (50 brands)
Tata Group · Reliance Industries · Infosys · Wipro · HCL · HDFC Bank · ICICI Bank · Bajaj · Mahindra · Zomato · Swiggy · Flipkart · Ola · Paytm · Byju's · and 35 more.

### China (50 brands)
Alibaba · Tencent · Huawei · Xiaomi · ByteDance · Baidu · JD.com · Meituan · DiDi · BYD · CATL · Lenovo · and 38 more.

### Europe (50 brands)
LVMH · BMW · Mercedes-Benz · Volkswagen · Siemens · SAP · Nestlé · Unilever · Shell · HSBC · Santander · Rolls-Royce · Ferrari · IKEA · H&M · Spotify · and 34 more.

---

## 🧠 How It Works

`BRAND_CREATIVE.md` embeds a structured **Brand Token Schema** for every company. When you request a branded creative, the AI looks up the brand, loads these tokens silently, and applies them to every design decision:

```
primary_color     Main brand color — backgrounds, headers, CTAs
secondary_color   Supporting color — subheadings, borders, accents
accent_color      Highlight / CTA color
tone_keywords     e.g. ["bold", "modern", "aspirational"]
tagline           Brand's signature line
font_vibe         geometric-sans | humanist-sans | condensed-bold | serif | display
logo_style        wordmark | lettermark | mark-only | emblem | combo
layout_pref       full-bleed | centered | editorial | grid | minimal
imagery_dir       e.g. "action-sport", "people-first", "product-hero"
```

Full schema reference: [docs/BRAND_TOKEN_SCHEMA.md](docs/BRAND_TOKEN_SCHEMA.md)

---

## 📐 Social Media Formats

Pixel-perfect specifications for every major format on 9 platforms. Quick reference: [docs/SOCIAL_FORMATS.md](docs/SOCIAL_FORMATS.md)

---

## 🔀 Co-Brand Support

Combines two brands into one coherent identity using a strict merge protocol — neither brand dominates, neutral background, both marks visible with a separator. See [examples/cobranding-guide.md](examples/cobranding-guide.md).

---

## ⚙️ Fidelity Modes

| Mode | Applies |
|---|---|
| **Lite** | Primary, secondary, accent colors · Tone of voice · Tagline |
| **Full** | Everything in Lite + fonts · heading case · layout preference · border radius · imagery direction · logo treatment |

---

## 🛠 Custom Brands

For brands not in the index, a 5-question intake flow collects the essentials and normalises them into the same token schema used by the 300 presets. Full fidelity adds 5 more questions.

---

## 📝 License

MIT — free to use, modify, and distribute. See [LICENSE](LICENSE).

---

## 🤝 Contributing

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on adding brands, fixing tokens, updating social specs, and adding new tool adapters.

---

## 📌 Version

**v1.0** — 300 brands · 9 social platforms · 9 output types · 4 tool adapters
