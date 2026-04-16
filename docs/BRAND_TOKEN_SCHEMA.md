# Brand Token Schema Reference

Every brand — whether from the 300-company index (Section 7) or a custom intake (Section 8) — is normalised into a unified token schema before any output is generated. This document explains each field.

---

## Core Tokens (Lite + Full fidelity)

These 8 fields are always populated. They drive every Lite-fidelity output and are the foundation of Full fidelity.

| Token | Type | Description | Example |
|---|---|---|---|
| `primary_color` | `#HEX` | Main brand color — used on backgrounds, headers, primary CTAs | `#E31937` |
| `secondary_color` | `#HEX` | Supporting color — subheadings, borders, accents, secondary UI | `#000000` |
| `accent_color` | `#HEX` | Highlight / CTA focus color — used sparingly for pop | `#FFFFFF` |
| `bg_color` | `#HEX` or `"auto"` | Default page/slide background. `auto` resolves to white or near-white | `#FFFFFF` |
| `text_color` | `#HEX` or `"auto"` | Body text. `auto` = dark on light backgrounds, light on dark | `#1A1A1A` |
| `tone_keywords` | `string[]` | 3–6 descriptors that define the brand's communication style | `["bold","athletic","empowering"]` |
| `tagline` | `string` | Brand's official tagline — used as a design element when space allows | `"Just Do It"` |
| `industry` | `string` | Sector context — informs imagery direction and content defaults | `"sportswear"` |

---

## Extended Tokens (Full fidelity only)

These fields are unlocked when the user selects **Full** fidelity. They allow the AI to precisely match typography, layout, and imagery.

| Token | Type | Allowed values | Description |
|---|---|---|---|
| `font_primary` | `string` | Any font name | Heading / display typeface. Use the closest available Google Font if the exact face is custom | `"Trade Gothic"` |
| `font_secondary` | `string` | Any font name | Body / supporting typeface | `"Helvetica Neue"` |
| `font_vibe` | `enum` | `geometric-sans` · `humanist-sans` · `condensed-bold` · `classic-serif` · `modern-serif` · `display` · `mono` · `script` | Abstract type personality — used when exact fonts aren't available |
| `logo_style` | `enum` | `wordmark` · `lettermark` · `mark-only` · `emblem` · `combo` | How the brand mark is treated in layout |
| `heading_case` | `enum` | `title` · `upper` · `sentence` · `lower` | CSS `text-transform` applied to all headings |
| `layout_pref` | `enum` | `full-bleed` · `centered` · `editorial` · `grid` · `minimal` | Structural layout personality |
| `imagery_dir` | `string` | e.g. `"action-sport"`, `"people-first"`, `"product-hero"`, `"abstract"` | Description of what photos/illustrations should convey |
| `border_radius` | `enum` | `none` · `subtle (4px)` · `rounded (8px)` · `pill` | Applied consistently to cards, buttons, and image frames |

---

## Font Vibe → System Font Mapping

When `font_primary` is not a web-safe or Google Font, map `font_vibe` to a safe system fallback:

| Vibe | Fallback stack |
|---|---|
| `geometric-sans` | Inter, DM Sans, system-ui |
| `humanist-sans` | system-ui, Segoe UI, sans-serif |
| `condensed-bold` | Barlow Condensed, Impact, sans-serif |
| `classic-serif` | Georgia, Playfair Display, serif |
| `modern-serif` | DM Serif Display, Cormorant Garamond, serif |
| `display` | Match to brand character — ask if unclear |
| `script` | Dancing Script, Pacifico, cursive |
| `mono` | JetBrains Mono, Courier New, monospace |

---

## Co-Brand Token Merge

When two brands are combined, tokens are merged by this ruleset:

| Token | Rule |
|---|---|
| `primary_color` | Brand A's primary |
| `secondary_color` | Brand B's primary |
| `accent_color` | Brand A's accent, or neutral white/grey if colors clash |
| `bg_color` | White or very light neutral — never one brand's color |
| `font_primary` | Brand A's font, or Inter/Helvetica if fonts conflict |
| `font_secondary` | Brand B's font, or same neutral |
| `tone_keywords` | Union of both, deduplicated, max 6 |
| `layout_pref` | Always `grid` or `editorial` (never `full-bleed`) |
| `logo_style` | Always `combo` — both marks side by side |
| `tagline` | Omit both; use context-specific copy only |
