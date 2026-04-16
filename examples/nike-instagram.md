# Example: Nike Instagram Post

This example walks through a complete interaction using the Brand Creative Skill for a Nike social creative.

---

## Prompt

```
Make me a Nike Instagram post (feed portrait) for a new running shoe launch.
Headline: "Your fastest mile starts here."
CTA: "Shop Now"
```

---

## What the Skill Does

1. **Identifies output type** → Social media creative (Instagram feed portrait)
2. **Loads Nike brand tokens silently:**
   ```
   primary_color   #000000
   secondary_color #FFFFFF
   accent_color    #F5821F
   font_vibe       condensed-bold
   logo_style      mark-only (the Swoosh)
   tone_keywords   athletic, empowering, aspirational, bold
   tagline         Just Do It
   layout_pref     full-bleed
   ```
3. **Reads Section 9.1** → Instagram feed portrait = 1080 × 1350 · 4:5 · 50px safe zone
4. **Applies brand application rules (Lite fidelity):**
   - Black full-bleed background
   - White headline in condensed bold type
   - Orange (#F5821F) CTA button
   - Swoosh mark as dominant visual element
   - Copy voice: punchy, action-oriented ("starts here" not "begins here")
5. **Runs quality checklist** before output

---

## Expected Output Characteristics

- 1080 × 1350px HTML artifact (renders at correct aspect ratio)
- Black background, white heading text
- "YOUR FASTEST MILE STARTS HERE." in all-caps condensed type
- Nike Swoosh as the anchor visual
- Orange "Shop Now" CTA button
- "Just Do It" in small footer type
- All critical elements within 50px safe zone

---

## Variations to Try

```
Full fidelity version — same prompt but say "use Full fidelity"
```

```
Co-brand version: Nike × Apple Watch running launch
```

```
Different format: "Make it a Stories format instead"
```

```
Different platform: "Same campaign, now make a YouTube thumbnail"
```
