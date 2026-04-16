# Example: Co-Branding Guide

How the skill handles two-brand creative requests.

---

## Example Prompt

```
Create a co-branded HTML landing page for a Netflix × Spotify campaign 
called "Soundtrack Your Binge". Headline: "Every scene. Every song."
```

---

## Token Merge Result

| Token | Netflix | Spotify | Merged |
|---|---|---|---|
| `primary_color` | #E50914 | #1DB954 | **#E50914** (Netflix = Brand A, listed first) |
| `secondary_color` | #000000 | #000000 | **#1DB954** (Spotify's primary becomes secondary) |
| `accent_color` | #FFFFFF | #FFFFFF | **#FFFFFF** (neutral — colors would clash as accent) |
| `bg_color` | — | — | **#0A0A0A** (near-black neutral; neither brand's color) |
| `font_vibe` | condensed | humanist | **system neutral** (Inter — fonts conflict) |
| `layout_pref` | — | — | **grid** (co-brand rule: never full-bleed) |
| `logo_style` | — | — | **combo** (both marks side by side with separator) |
| `tone_keywords` | bold, entertainment, global | music, joyful, discovery | **bold, entertainment, music, joyful, discovery, global** |
| `tagline` | — | — | *omitted* (context-specific copy used instead) |

---

## Visual Treatment Rules Applied

1. **Split layout:** Netflix red zone on left · Spotify green zone on right · neutral dark centre body
2. **Logo placement:** Netflix wordmark ↔ separator line ↔ Spotify mark
3. **No gradient between brand colors** — flat adjacent zones only
4. **Headline in white** on the dark neutral background section
5. **CTA uses accent white** — neither brand's primary color dominates the action button

---

## More Co-Brand Examples to Try

```
Co-branded event sponsorship deck: Adidas × Puma (rival brands test)
```

```
Co-branded social post: IKEA × Airbnb ("design your stay")
```

```
Co-branded email: Amazon × Whole Foods (same parent company)
```

```
Co-branded invoice/form: Tata × JLR (acquisition scenario)
```

---

## Co-Brand Rules Reference

Full merge rules are in `BRAND_CREATIVE.md` Section 5. Key constraints:

- **Brand A** = first brand mentioned, or the larger/more recognisable brand
- **Never** use both brands' primary colors as a gradient — always flat zones
- **Never** use either brand's tagline — write context-specific copy
- Background is **always** a neutral (white, off-white, or dark neutral)
- Layout is **always** grid or editorial — never full-bleed
