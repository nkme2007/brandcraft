---
name: brand-creative
version: 1.0
description: >
  Brand-aware creative skill. Use whenever a user wants to create any visual or written
  artifact — HTML creatives, presentations, social media posts, official letters, brand
  documents, email templates, invoices, event creatives, or reports — following a specific
  brand's visual identity. Triggers when user says things like "make me a post for Nike",
  "create a branded deck", "design a social creative in Apple's style", "write a letter
  on Tata's letterhead", "build an HTML landing page for Zomato", or "co-branded creative
  for Nike and Adidas". Also triggers for "custom brand" creative requests where the user
  provides their own colors and style. Contains an embedded index of 300 global companies
  (US · India · China · Europe) plus full social media format specifications. Always use
  this skill for any brand-driven creative output, even if the request seems simple.
---

# Brand Creative Skill

A single-file skill for producing brand-accurate creative artifacts across any output type.
All data — 300 company brand tokens, social media format specs, and co-brand rules — is
embedded below. No external files needed.

---

## Table of Contents

1. [Elicitation Protocol](#1-elicitation-protocol)
2. [Brand Token Schema](#2-brand-token-schema)
3. [Output Routing Table](#3-output-routing-table)
4. [Brand Application Rules](#4-brand-application-rules)
5. [Co-Brand Merge Rules](#5-co-brand-merge-rules)
6. [Quality Checklist](#6-quality-checklist)
7. [Brand Index — 300 Companies](#7-brand-index)
8. [Custom Brand Intake](#8-custom-brand-intake)
9. [Social Media Formats Reference](#9-social-media-formats-reference)

---

## 1. Elicitation Protocol

Run these steps in order. Present each step's options as a clear numbered or button list.
**Never ask more than one question per step. Never ask what can be inferred.**

### Step 1 — What are we making?
Ask the user to pick an output type. Options:
- HTML creative (landing page, banner, microsite)
- Presentation / pitch deck (PPTX)
- Social media creative (image, carousel, story, reel)
- Official letter / formal document (DOCX/PDF)
- Brand guidelines document (PDF)
- Email template (HTML)
- Report / whitepaper (PDF)
- Invoice / business form (PDF/DOCX)
- Event creative (invite, banner, sponsorship deck)

### Step 2 — Which brand(s)?
Ask the user to choose:
- **Pick from 300** → show region filter (US / India / China / Europe), then ask for company name. Look it up in Section 7 and load tokens silently.
- **My own brand** → proceed to Section 8 (Custom Brand Intake).
- **Co-brand (two companies)** → repeat brand selection twice, then apply Section 5 merge rules.

### Step 3 — Brand fidelity depth?
Ask: **Lite** (colors + tone only — faster) or **Full** (colors, fonts, logo approach, layout, imagery direction)?
- Default to Lite if user seems in a hurry or hasn't specified.
- Full unlocks extended token fields; ask the additional questions in Section 8 only if needed.

### Step 4 — Content inputs (minimum questions)
Ask only what is essential for the chosen output type:

| Output type | Ask for |
|---|---|
| HTML creative | Headline, key message, CTA text, target audience |
| Presentation | Title, agenda / key sections (3–5), audience |
| Social creative | Platform + format (use Section 9), headline, body copy (optional), CTA |
| Official letter | Recipient name + org, purpose of letter, key points (3 max) |
| Brand guidelines | Which brand (already known), sections to include |
| Email template | Subject line, body sections, CTA, send context |
| Report | Title, section headings, key data points |
| Invoice / form | Company name, recipient, line items (ask user to paste) |
| Event creative | Event name, date, venue, theme/mood |

### Step 5 — Generate
- If output is social → read Section 9 and apply exact platform dimensions.
- If your tool supports downstream document-generation modules (PPTX, DOCX, PDF), invoke the appropriate one before generating. See `adapters/` for tool-specific integration notes.
- Apply brand tokens per Section 4.
- Run Section 6 checklist before delivering.

---

## 2. Brand Token Schema

Every brand — preset (Section 7) or custom (Section 8) — is normalised into this schema before generation.

### Core tokens (Lite + Full)
```
primary_color     #HEX           Main brand color — backgrounds, headers, CTAs
secondary_color   #HEX           Supporting color — subheadings, borders, accents
accent_color      #HEX           Highlight / call-to-action color
bg_color          #HEX / "auto"  Default background (auto = white or near-white)
text_color        #HEX           Body text color (auto = dark on light, light on dark)
tone_keywords[]   string[]       e.g. ["bold", "modern", "aspirational"] — drives copy voice
tagline           string         Brand's signature line (use as design element when relevant)
industry          string         Context for imagery direction and content defaults
```

### Extended tokens (Full fidelity only)
```
font_primary      string         e.g. "Trade Gothic", "Helvetica Neue", "Garamond"
font_secondary    string         Body / supporting face
font_vibe         string         One of: geometric-sans | humanist-sans | condensed-bold |
                                  classic-serif | modern-serif | display | mono | script
logo_style        string         One of: wordmark | lettermark | mark-only | emblem | combo
heading_case      string         One of: title | upper | sentence | lower
layout_pref       string         One of: full-bleed | centered | editorial | grid | minimal
imagery_dir       string         e.g. "action-sport", "people-first", "product-hero", "abstract"
border_radius     string         One of: none | subtle (4px) | rounded (8px) | pill
```

---

## 3. Output Routing Table

| Output type | File format | Read downstream skill? | Key constraint |
|---|---|---|---|
| HTML creative | .html artifact | Yes → use tool's HTML/frontend generation | Render inline; CSS vars for brand colors |
| Presentation | .pptx file | Yes → use tool's PPTX generation | Brand colors on master slide |
| Social creative | .html artifact | No (use Section 9 dimensions) | Exact px dimensions matter |
| Official letter | .docx or .pdf | Yes → use tool's DOCX or PDF generation | Letterhead from brand tokens |
| Brand guidelines | .pdf | Yes → use tool's PDF generation | Showcase all tokens visually |
| Email template | .html artifact | No | Inline CSS only; 600px max width |
| Report / whitepaper | .pdf | Yes → use tool's PDF generation | Cover page uses brand colors |
| Invoice / form | .pdf or .docx | Yes → use tool's PDF or DOCX generation | Brand header + clean grid layout |
| Event creative | .html + .pptx | Yes → use both HTML and PPTX generation | Co-brand safe if applicable |

---

## 4. Brand Application Rules

### 4.1 Lite fidelity (colors + tone)
- Replace all primary UI colors with `primary_color`
- Use `secondary_color` for borders, dividers, subheadings
- Use `accent_color` for CTAs, highlights, links
- Set `bg_color` as page/slide background
- Write copy in the style of `tone_keywords` — bold brands get punchy copy, elegant brands get measured prose
- Use `tagline` as a footer or supporting visual element when space allows
- Font: use a system sans that matches `font_vibe` (geometric → Inter/DM Sans; humanist → system-ui; condensed-bold → Impact/Barlow Condensed; serif → Georgia/Playfair)

### 4.2 Full fidelity (all tokens)
Everything in Lite, plus:
- Reference `font_primary` by name in CSS / slide master (use closest Google Font if custom)
- Apply `heading_case` to all headings (upper → text-transform: uppercase, etc.)
- Apply `layout_pref`: full-bleed = color covers full slide/page; editorial = wide margins + pull quotes; grid = structured columns; minimal = lots of white space
- Apply `border_radius` consistently to all cards, buttons, images
- Frame imagery descriptions using `imagery_dir` — describe what photos/illustrations should show
- Apply `logo_style` to determine how the brand mark is referenced in layout (mark-only = geometric shape placeholder; wordmark = text-based logo zone; emblem = badge/shield zone)

### 4.3 Co-brand application
Follow Section 5 merge rules, then apply as if it were a single merged token set.
Use a split visual treatment where both brand identities are visible but neither dominates fully.

---

## 5. Co-Brand Merge Rules

When two brands are selected, produce a merged token set as follows:

| Token | Rule |
|---|---|
| `primary_color` | Brand A's primary (dominant partner — first selected or larger brand) |
| `secondary_color` | Brand B's primary (supporting partner) |
| `accent_color` | Brand A's accent, or a neutral bridge (white / mid-grey) if colors clash |
| `bg_color` | White or very light neutral — never either brand's color alone |
| `font_primary` | Brand A's font, OR system-safe neutral (Inter / Helvetica) if fonts conflict |
| `font_secondary` | Brand B's font or same neutral |
| `tone_keywords` | Union of both sets, deduplicated, max 6 keywords |
| `layout_pref` | Always "grid" or "editorial" — never full-bleed (that belongs to one brand) |
| `logo_style` | "combo" — both marks shown side by side with separator |
| `tagline` | Do not use either brand's tagline; omit or use context-specific line |

**Visual treatment rules for co-brand layouts:**
- Use a vertical or horizontal divider to give each brand a zone when showing both marks
- Brand A zone: primary_color dominant | Brand B zone: secondary_color dominant
- Shared body area: neutral background, both colors as accent only
- Never mix the two brand's primary colors as gradients — use them as flat adjacent zones

---

## 6. Quality Checklist

Before delivering any artifact, verify:

- [ ] Brand primary color is applied to at least one dominant element (header, background, CTA)
- [ ] Secondary and accent colors appear at least once each
- [ ] Copy tone matches `tone_keywords` — read it back mentally before outputting
- [ ] If Full fidelity: font, heading case, layout pref, and border radius are all applied
- [ ] If social: dimensions match the exact platform + format spec from Section 9
- [ ] If co-brand: both brand colors appear; layout is neutral-base; no single brand dominates
- [ ] File is in the correct format and exported/saved via your tool's file output method
- [ ] Tagline appears somewhere tasteful if space allows
- [ ] No placeholder text (Lorem Ipsum, [INSERT HERE]) left in output

---

## 7. Brand Index

Compact format per entry:
`ID | Company | Colors (primary,secondary,accent) | Font vibe | Logo style | Tone keywords | Tagline`

Font vibe codes: `geo` geometric-sans · `hum` humanist-sans · `cond` condensed-bold · `cser` classic-serif · `mser` modern-serif · `disp` display · `scr` script
Logo style codes: `wm` wordmark · `lm` lettermark · `mk` mark-only · `emb` emblem · `cmb` combo

---

### 7.1 United States (100)

```
US-001 | Apple | #000000,#FFFFFF,#A2AAAD | geo | mk | innovative,aspirational,minimal,premium | Think Different
US-002 | Microsoft | #0078D4,#FFFFFF,#FFB900 | geo | cmb | empowering,inclusive,professional,modern | Empower every person
US-003 | Amazon | #FF9900,#232F3E,#146EB4 | hum | cmb | customer-obsessed,friendly,vast,reliable | Work hard. Have fun. Make history.
US-004 | Google | #4285F4,#FFFFFF,#FBBC04 | geo | mk | helpful,playful,trustworthy,curious | Do the right thing
US-005 | Meta | #0082FB,#1C2B33,#FFFFFF | geo | wm | connecting,bold,optimistic,social | Give people the power to build community
US-006 | NVIDIA | #76B900,#000000,#FFFFFF | geo | cmb | technical,powerful,pioneering,bold | The way it's meant to be played
US-007 | Berkshire Hathaway | #003087,#FFFFFF,#C9A84C | cser | wm | conservative,trustworthy,long-term,stable | —
US-008 | Tesla | #E31937,#000000,#FFFFFF | geo | mk | innovative,electric,bold,sustainable | Accelerating the world's transition to sustainable energy
US-009 | Walmart | #0071CE,#FFFFFF,#FFC220 | hum | cmb | friendly,value-driven,everyday,inclusive | Save money. Live better.
US-010 | JPMorgan Chase | #003087,#FFFFFF,#D1A123 | cser | cmb | authoritative,trustworthy,global,premium | How can we help you?
US-011 | Visa | #1A1F71,#FFFFFF,#F7B600 | geo | wm | trusted,global,seamless,secure | Everywhere you want to be
US-012 | Johnson & Johnson | #CC0000,#FFFFFF,#D1A123 | scr | emb | caring,trusted,human,scientific | For all you love
US-013 | UnitedHealth Group | #002677,#FFFFFF,#00BED5 | hum | cmb | caring,expert,trusted,accessible | Health care that works for all of us
US-014 | ExxonMobil | #CC0000,#002B5C,#FFFFFF | geo | cmb | reliable,engineering,global,strong | Taking on the world's toughest energy challenges
US-015 | Chevron | #0066B2,#FFFFFF,#F0AB00 | hum | cmb | reliable,forward-thinking,responsible,strong | Human Energy
US-016 | Procter & Gamble | #003087,#FFFFFF,#0070C0 | hum | wm | trusted,caring,innovative,consumer-first | —
US-017 | Home Depot | #F96302,#000000,#FFFFFF | hum | cmb | helpful,DIY,practical,bold | How doers get more done
US-018 | Bank of America | #012169,#FFFFFF,#E31837 | hum | emb | professional,accessible,responsible,modern | Life's better when we're connected
US-019 | Mastercard | #EB001B,#F79E1B,#FFFFFF | geo | mk | seamless,inclusive,trusted,global | Start Something Priceless
US-020 | Pfizer | #0093D0,#FFFFFF,#E31837 | hum | wm | scientific,trusted,caring,life-affirming | Breakthroughs that change patients' lives
US-021 | Comcast/NBCUniversal | #CB333B,#FFFFFF,#000000 | hum | mk | entertaining,bold,diverse,connected | The power of now
US-022 | AbbVie | #071D49,#FFFFFF,#00ADEF | hum | wm | science-driven,patient-centered,bold | Remarkable impact on patients' lives
US-023 | Costco | #003399,#FFFFFF,#E31837 | hum | wm | value,members-first,straightforward | —
US-024 | Walt Disney | #003082,#FFFFFF,#F5C518 | scr | emb | magical,nostalgic,family,storytelling | Where dreams come true
US-025 | Cisco | #049FD9,#000000,#FFFFFF | geo | cmb | connected,innovative,secure,trusted | The bridge to possible
US-026 | Salesforce | #00A1E0,#FFFFFF,#FF6600 | hum | mk | customer-success,innovative,fun,connected | We bring companies and customers together
US-027 | Nike | #000000,#FFFFFF,#F5821F | cond | mk | athletic,empowering,aspirational,bold | Just Do It
US-028 | AT&T | #00A8E0,#FFFFFF,#000000 | hum | mk | connected,reliable,modern,accessible | Your world. Delivered.
US-029 | Intel | #0071C5,#FFFFFF,#00AEEF | hum | cmb | innovative,intelligent,precise,global | Experience what's inside
US-030 | Goldman Sachs | #1B1B1B,#FFFFFF,#7399C6 | cser | wm | premium,elite,trusted,discreet | —
US-031 | Adobe | #FF0000,#FFFFFF,#1473E6 | geo | mk | creative,empowering,innovative,digital | Creativity for all
US-032 | Netflix | #E50914,#000000,#FFFFFF | cond | wm | bold,entertainment,global,addictive | See what's next
US-033 | Airbnb | #FF5A5F,#00A699,#FC642D | hum | mk | belonging,warm,adventurous,inclusive | Belong Anywhere
US-034 | Uber | #000000,#FFFFFF,#276EF1 | geo | wm | direct,confident,modern,global | Move the way you want
US-035 | Starbucks | #00704A,#FFFFFF,#CBA258 | hum | emb | warm,community,premium,personal | To inspire and nurture the human spirit
US-036 | PayPal | #003087,#FFFFFF,#009CDE | geo | lm | secure,simple,global,trusted | The way forward
US-037 | 3M | #FF0000,#000000,#FFFFFF | hum | lm | innovative,scientific,versatile,reliable | Science. Applied to life.
US-038 | McDonald's | #DA291C,#FFFFFF,#FFC72C | hum | mk | fun,accessible,family,global | I'm Lovin' It
US-039 | Coca-Cola | #F40009,#FFFFFF,#000000 | scr | wm | joyful,refreshing,timeless,universal | Taste the Feeling
US-040 | IBM | #0F62FE,#000000,#FFFFFF | geo | lm | innovative,trusted,enterprise,smart | Let's create
US-041 | Lockheed Martin | #002B5C,#FFFFFF,#C9A84C | hum | emb | precision,patriotic,mission-driven,trusted | We never forget who we're working for
US-042 | Boeing | #0033A0,#FFFFFF,#C9A84C | hum | wm | engineering,reliable,pioneering | Forever New Frontiers
US-043 | General Motors | #0178BF,#FFFFFF,#E31837 | geo | mk | American,innovative,electric,reliable | Everybody In
US-044 | Ford | #003478,#FFFFFF,#C9A84C | hum | emb | trustworthy,tough,American,forward | Built Ford Tough
US-045 | Caterpillar | #FFCD11,#000000,#FFFFFF | cond | wm | tough,reliable,industrial,powerful | Built for it
US-046 | Raytheon | #003082,#FFFFFF,#C9A84C | hum | emb | innovative,patriotic,precision,trusted | Clear to engage
US-047 | CVS Health | #CC0000,#1A5276,#FFFFFF | hum | cmb | caring,community,accessible,health-first | Health is everything
US-048 | Elevance Health | #1957A6,#FFFFFF,#78BE20 | hum | mk | empowering,caring,bold,trustworthy | Live whole, whole life
US-049 | Cigna | #006747,#FFFFFF,#00965E | hum | mk | personalized,caring,global,motivated | Together, all the way
US-050 | Humana | #007BC4,#FFFFFF,#78BE20 | hum | mk | inclusive,human,hopeful,guiding | Guided by your health
US-051 | Honeywell | #EB0028,#FFFFFF,#000000 | hum | mk | innovative,safe,connected,industrial | The future is what we make it
US-052 | Qualcomm | #3253DC,#FFFFFF,#E31837 | geo | wm | connected,innovative,5G,technical | Inventing the tech the world loves
US-053 | Texas Instruments | #BD1723,#FFFFFF,#000000 | hum | wm | precise,innovative,engineering-first | Think Possible
US-054 | John Deere | #367C2B,#FFDE00,#000000 | hum | emb | dependable,agricultural,innovative | Nothing Runs Like a Deere
US-055 | Colgate-Palmolive | #DA291C,#FFFFFF,#003399 | hum | emb | caring,healthy,family,trusted | Every life is worth a smile
US-056 | General Electric | #3A5DAE,#FFFFFF,#000000 | hum | lm | innovative,industrial,trusted,pioneering | Imagination at Work
US-057 | American Express | #006FCF,#FFFFFF,#D1A123 | cser | emb | premium,trusted,service,exclusive | Don't live life without it
US-058 | Wells Fargo | #CC0000,#FFCD11,#FFFFFF | hum | emb | caring,diverse,established,trusted | Together we'll go far
US-059 | Target | #CC0000,#FFFFFF,#000000 | hum | mk | fun,stylish,accessible,friendly | Expect more. Pay less.
US-060 | Lowe's | #004990,#FFFFFF,#00A550 | hum | wm | helpful,practical,home-focused | Do it right for less
US-061 | FedEx | #4D148C,#FF6600,#FFFFFF | geo | wm | fast,precise,reliable,global | The World on Time
US-062 | UPS | #351C15,#FFB500,#FFFFFF | hum | emb | reliable,global,trusted | What can Brown do for you?
US-063 | Verizon | #CD040B,#000000,#FFFFFF | cond | wm | reliable,connected,powerful,simple | The network more people rely on
US-064 | T-Mobile | #E20074,#000000,#FFFFFF | cond | wm | bold,disruptive,fun,customer-champion | Connecting You to the World
US-065 | PepsiCo | #004B93,#E3000F,#FFFFFF | geo | mk | youthful,fun,bold,global | Live for Now
US-066 | Kraft Heinz | #C41F3E,#FFFFFF,#009A44 | hum | emb | trusted,heritage,tasty,accessible | Let's talk about food
US-067 | General Mills | #C41F3E,#FFFFFF,#000000 | hum | mk | nourishing,family,heritage,trusted | Nourishing Lives
US-068 | Marriott | #8B0000,#FFFFFF,#D1A123 | cser | wm | welcoming,premium,global,reliable | Travel brilliantly
US-069 | Hilton | #003087,#FFFFFF,#D1A123 | cser | mk | hospitable,premium,timeless,global | Be Hospitable
US-070 | Zoom | #2D8CFF,#FFFFFF,#000000 | hum | mk | simple,reliable,connecting,approachable | Meet Happy
US-071 | Slack | #4A154B,#36C5F0,#2EB67D | geo | mk | fun,collaborative,productive,friendly | Make work life simpler
US-072 | Snowflake | #29B5E8,#FFFFFF,#3A4F6E | geo | mk | data-driven,innovative,scalable,trusted | Mobilize your data
US-073 | Stripe | #635BFF,#0A2540,#FFFFFF | mser | wm | developer-first,elegant,powerful,global | Financial infrastructure for the internet
US-074 | SpaceX | #000000,#FFFFFF,#C0C0C0 | geo | wm | pioneering,audacious,engineering-first | Making life multiplanetary
US-075 | Palo Alto Networks | #FA582D,#FFFFFF,#000000 | hum | mk | secure,innovative,vigilant,trusted | The cybersecurity partner of choice
US-076 | Workday | #005CB9,#FFFFFF,#F4C430 | hum | mk | human-centered,simple,enterprise-smart | Your whole self, at work
US-077 | ServiceNow | #62D84E,#293E40,#FFFFFF | hum | wm | digital,efficient,enterprise,helpful | The intelligent platform
US-078 | CrowdStrike | #FC4C02,#000000,#FFFFFF | cond | mk | fearless,fast,protection-first,elite | Stop breaches. Period.
US-079 | HubSpot | #FF7A59,#2D3E50,#FFFFFF | hum | mk | helpful,inbound,growth,friendly | Grow better
US-080 | Shopify | #96BF48,#5C6AC4,#FFFFFF | hum | cmb | merchant-first,empowering,accessible | Make commerce better for everyone
US-081 | Block/Square | #000000,#FFFFFF,#3E9B4F | geo | mk | democratic,simple,modern | Economic empowerment for all
US-082 | Lyft | #FF00BF,#000000,#FFFFFF | cond | mk | fun,community,safe,expressive | It matters how you get there
US-083 | DoorDash | #FF3008,#FFFFFF,#000000 | hum | mk | fast,friendly,food-first,local | Delicious. Delivered.
US-084 | Instacart | #43B02A,#FFFFFF,#003366 | hum | mk | fresh,convenient,helpful,local | Get groceries you love delivered
US-085 | Peloton | #D80032,#000000,#FFFFFF | hum | mk | motivating,community,premium,bold | Together we go far
US-086 | Sonos | #000000,#FFFFFF,#C0C0C0 | geo | mk | audiophile,premium,minimal,home | The home sound system
US-087 | Warby Parker | #253746,#FFFFFF,#E8D9C8 | hum | wm | witty,accessible,stylish,purpose-driven | Glasses for good
US-088 | Rivian | #009B77,#FFFFFF,#1E1E1E | hum | mk | adventurous,sustainable,bold,outdoorsy | Electric adventure
US-089 | Lucid Motors | #B8985A,#000000,#FFFFFF | mser | mk | luxury,precision,forward,calm | Inspire the revolution
US-090 | Twilio | #F22F46,#FFFFFF,#000000 | hum | mk | developer-empowering,bold,communication | Build the future of communications
US-091 | Datadog | #632CA6,#FFFFFF,#774AA4 | hum | mk | observability,technical,smart | Monitor everything. Decide fast.
US-092 | Palantir | #000000,#FFFFFF,#6C6C6C | cser | mk | analytical,precise,elite | The world's leading AI platform
US-093 | Robinhood | #00C805,#000000,#FFFFFF | hum | mk | democratizing,bold,millennial-friendly | Investing for everyone
US-094 | Expedia | #FBCC34,#003087,#FFFFFF | hum | cmb | adventurous,open,accessible,joyful | Travel with confidence
US-095 | Adobe | #FF0000,#FFFFFF,#1473E6 | geo | mk | creative,empowering,innovative,digital | Creativity for all
US-096 | Kellogg's | #E10000,#FFFFFF,#000000 | scr | emb | breakfast,nutritious,family | Gr-r-reat!
US-097 | Colgate | #DA291C,#FFFFFF,#003399 | hum | emb | caring,healthy,family,trusted | Smile for good
US-098 | 3M | #FF0000,#000000,#FFFFFF | hum | lm | innovative,scientific,versatile | Science. Applied to life.
US-099 | Motorola | #005A9C,#FFFFFF,#E31837 | geo | lm | connected,innovative,reliable | Hello Moto
US-100 | Harley-Davidson | #FF5C00,#000000,#FFFFFF | cond | emb | freedom,American,bold,heritage | All for freedom, freedom for all
```

---

### 7.2 India (100)

```
IN-001 | Reliance Industries | #003087,#FFFFFF,#CC0000 | hum | cmb | ambitious,national,diversified,powerful | Growth is Life
IN-002 | TCS | #003087,#FFFFFF,#C0C0C0 | hum | cmb | reliable,global,trustworthy,innovative | Building on belief
IN-003 | HDFC Bank | #00497A,#FFFFFF,#D60017 | hum | cmb | trusted,smart,customer-first | We understand your world
IN-004 | Infosys | #007CC2,#FFFFFF,#C0C0C0 | hum | mk | innovative,humble,global,reliable | Navigate your next
IN-005 | ICICI Bank | #F36F21,#003087,#FFFFFF | hum | mk | modern,accessible,innovative | Khayal Aapka
IN-006 | Wipro | #341F6A,#75B443,#FFFFFF | hum | mk | innovative,inclusive,reliable,tech | Innovating to help the world move forward
IN-007 | HCL Technologies | #0076CF,#FFFFFF,#C0C0C0 | hum | mk | innovative,agile,global,reliable | Superseding expectations
IN-008 | Bajaj Finance | #003087,#D40000,#FFFFFF | hum | wm | trustworthy,innovative,accessible | You are our business
IN-009 | Kotak Mahindra Bank | #C00000,#FFFFFF,#000000 | geo | mk | progressive,smart,value-driven | Think Investments, Think Kotak
IN-010 | Axis Bank | #970028,#FFFFFF,#C0C0C0 | hum | wm | progressive,customer-oriented,reliable | Badhte ka naam Zindagi
IN-011 | L&T | #003087,#FFFFFF,#C9A84C | hum | wm | engineering,nation-building,trusted | It's all about Imagineering
IN-012 | Asian Paints | #FF6600,#003087,#FFFFFF | hum | emb | colorful,aspirational,home-first,Indian | Har Ghar Kuch Kehta Hai
IN-013 | Maruti Suzuki | #003087,#C00000,#FFFFFF | hum | mk | reliable,affordable,trustworthy | Count on us
IN-014 | Sun Pharma | #003087,#F5A623,#FFFFFF | hum | mk | life-saving,reliable,scientific,global | Touching lives improving life
IN-015 | ITC Limited | #5B4A00,#FFFFFF,#CC0000 | cser | wm | trusted,heritage,sustainable,diverse | Enduring Value
IN-016 | Titan | #7B3F00,#C0922A,#FFFFFF | cser | mk | precision,trust,craftsmanship,aspirational | Encapsulating excellence
IN-017 | UltraTech Cement | #003087,#F5A623,#FFFFFF | hum | mk | strength,nation-building,reliable | The engineer's choice
IN-018 | Power Grid Corp | #005DAA,#FFD700,#FFFFFF | hum | emb | national,reliable,essential | Powering India
IN-019 | NTPC | #0059AB,#E8212D,#FFFFFF | hum | emb | reliable,energy,large-scale | Powering India's growth
IN-020 | Tech Mahindra | #8B0000,#FFFFFF,#C0C0C0 | hum | mk | digital,connected,bold,IT | Rise
IN-021 | Bajaj Auto | #003087,#D40000,#FFFFFF | hum | wm | speed,reliability,heritage | Distinctly Ahead
IN-022 | Hero MotoCorp | #004B9B,#E11D30,#FFFFFF | hum | mk | leadership,freedom,dynamic | Built for India
IN-023 | Adani Enterprises | #003087,#FF6600,#FFFFFF | hum | mk | ambitious,national,infrastructure | Changing India together
IN-024 | Mahindra & Mahindra | #CC0000,#FFFFFF,#000000 | hum | mk | rise,rural,innovative,Indian | Rise
IN-025 | SBI | #2D6496,#FFFFFF,#C9A84C | hum | emb | trusted,accessible,national,mass | With you all the way
IN-026 | Tata Motors | #1B3A6E,#FFFFFF,#C0C0C0 | hum | emb | innovative,reliable,Indian | Connecting aspirations
IN-027 | JSW Steel | #003087,#C00000,#FFFFFF | hum | wm | strength,growth,nation-first | Building Tomorrow
IN-028 | Tata Steel | #1B3A6E,#C00000,#FFFFFF | hum | emb | responsible,pioneering,heritage | Values stronger than steel
IN-029 | Vedanta | #AE0000,#FFFFFF,#C9A84C | hum | mk | transforming,bold,growth | Transforming for Good
IN-030 | Godrej | #003087,#00A86B,#FFFFFF | hum | mk | trustworthy,innovative,heritage | Brighter Living
IN-031 | Hindustan Unilever | #003087,#0057A8,#FFFFFF | hum | mk | consumer-first,purposeful,sustainable | Making sustainable living commonplace
IN-032 | Nestle India | #CC0000,#FFFFFF,#000000 | hum | emb | nourishing,trusted,quality,family | Good Food, Good Life
IN-033 | Dr. Reddy's | #003087,#FFFFFF,#FF6600 | hum | wm | affordable,global,scientific | Good Health Can't Wait
IN-034 | Cipla | #003087,#FF6600,#FFFFFF | hum | wm | caring,accessible,life-saving | Caring for Life
IN-035 | Divi's Laboratories | #003087,#FFFFFF,#C9A84C | hum | wm | scientific,reliable,global | —
IN-036 | Bharti Airtel | #CC0000,#FFFFFF,#000000 | geo | mk | bold,connected,digital,passionate | Open Network
IN-037 | BPCL | #CC0000,#FFD700,#FFFFFF | hum | emb | energizing,reliable,essential | Energising Lives
IN-038 | HPCL | #003087,#FFD700,#FFFFFF | hum | emb | India-fuel,reliable,national | Powering progress
IN-039 | Indian Oil | #CC0000,#FFD700,#003087 | hum | emb | national,reliable,energy | Serving the nation
IN-040 | ONGC | #003087,#FF6600,#FFFFFF | hum | mk | national,exploratory,pride | Making tomorrow brighter
IN-041 | Pidilite | #CC0000,#FFFFFF,#FFD700 | hum | wm | bonding,trusted,household,essential | Har mushkil ka hall
IN-042 | Marico | #003087,#FFFFFF,#FFD700 | hum | wm | natural,healthy,trusted,Indian | Consumer-centric innovation
IN-043 | Dabur | #003087,#FFD700,#FFFFFF | hum | emb | natural,Ayurvedic,trustworthy | Celebrating Life
IN-044 | Britannia | #003087,#E8212D,#FFFFFF | hum | emb | trusted,nutritious,daily,heritage | Eat Healthy, Think Better
IN-045 | Tata Consumer | #1B3A6E,#FFFFFF,#C9A84C | hum | emb | trusted,household,quality | Transforming everyday life
IN-046 | Avenue Supermarts (DMart) | #003087,#E8212D,#FFFFFF | hum | wm | value,everyday,reliable | Khushiyon ki dukan
IN-047 | Zomato | #E23744,#FFFFFF,#000000 | hum | wm | vibrant,food-loving,bold,relatable | Delivering joy
IN-048 | Swiggy | #FC8019,#1C2430,#FFFFFF | hum | wm | fast,local,urban,youthful | Swiggy it
IN-049 | Ola Cabs | #3DBE29,#000000,#FFFFFF | hum | wm | Indian,tech-forward,affordable | Move India
IN-050 | Flipkart | #2874F0,#FFD700,#FFFFFF | hum | emb | joyful,Indian,accessible | Ab har wish hogi poori
IN-051 | Paytm | #00BAF2,#002970,#FFFFFF | geo | wm | simple,digital,Indian,trusted | Paytm Karo
IN-052 | PhonePe | #5F259F,#FFFFFF,#C0C0C0 | hum | mk | simple,trusted,digital | India ka payment app
IN-053 | CRED | #0F0F0F,#FFD700,#FFFFFF | geo | wm | exclusive,premium,creditworthy | Rewarding for you
IN-054 | PolicyBazaar | #F17125,#003087,#FFFFFF | hum | mk | transparent,empowering,India-first | Insured hai toh tensed nahi
IN-055 | BajajFinserv | #003087,#D40000,#FFFFFF | hum | wm | modern,accessible,smart | Life Goals. Aasaan ho jaate hain.
IN-056 | Muthoot Finance | #9B1B30,#FFD700,#FFFFFF | cser | emb | trusted,gold-lending,accessible | Gold's good here
IN-057 | HDFC Life | #00497A,#FFFFFF,#D60017 | hum | wm | caring,trusted,life-planning | Sar Utha ke Jiyo
IN-058 | SBI Life | #2D6496,#E8212D,#FFFFFF | hum | mk | trusted,national,accessible | Apno ko apna banaiye
IN-059 | LIC of India | #003087,#FFD700,#FFFFFF | hum | emb | national,security,mass | Your welfare is our responsibility
IN-060 | IndiGo Airlines | #003087,#E8212D,#FFFFFF | geo | wm | on-time,affordable,cheerful | On time is a beautiful thing
IN-061 | Air India | #CC0000,#FFD700,#FFFFFF | cser | emb | Indian-pride,warm,hospitable | The Maharaja
IN-062 | MakeMyTrip | #D90429,#FFFFFF,#000000 | hum | mk | joyful,trusted,travel-first | Dil toh roaming hai
IN-063 | Havells | #CC0000,#003087,#FFFFFF | hum | wm | innovative,life-enhancing,quality | The original HAVELLS
IN-064 | Voltas | #003087,#FF6600,#FFFFFF | hum | wm | cooling,reliable,Tata-trust | Cooling India since 1954
IN-065 | Boat Lifestyle | #000000,#FF3D00,#FFFFFF | cond | emb | bold,youth,energetic,audio | Plug into nirvana
IN-066 | Persistent Systems | #003087,#FF6600,#FFFFFF | hum | mk | digital,reliable,innovative | Ahead of change
IN-067 | Mphasis | #00A651,#003087,#FFFFFF | hum | mk | agile,digital,reliable | Apply intelligence everywhere
IN-068 | Tata Elxsi | #1B3A6E,#FFFFFF,#C9A84C | hum | wm | design-forward,innovative,art-tech | Think. Design. Innovate.
IN-069 | Jio Platforms | #0B3D8C,#FF6600,#FFFFFF | hum | wm | digital-India,affordable,connected | Poora India Jio
IN-070 | Nykaa | #F76C6C,#1A1A2E,#FFFFFF | hum | wm | aspirational,beauty,Indian-woman | Your beauty, our passion
IN-071 | Meesho | #9D0FC9,#FFFFFF,#C0C0C0 | hum | wm | empowering,micro-entrepreneur,India | Shopping ki nayi duniya
IN-072 | Urban Company | #000000,#F4C430,#FFFFFF | geo | mk | trusted,premium,home-services | Ab ghar bana salon
IN-073 | ShareChat | #5E2D79,#FFFFFF,#C0C0C0 | hum | mk | inclusive,vernacular,regional | Apni Bhasha Mein
IN-074 | Freshworks | #25C16F,#1E1E1E,#FFFFFF | hum | mk | simple,delight,innovative | Refreshingly simple software
IN-075 | Zoho | #E42527,#003087,#FFFFFF | hum | mk | bootstrapped,customer-first,innovative | Your business runs on Zoho
IN-076 | Info Edge (Naukri) | #4C9BE8,#003087,#FFFFFF | hum | mk | empowering,trusted,career-first | Things to do. Places to be.
IN-077 | Vodafone Idea (Vi) | #CC0000,#FFA500,#FFFFFF | hum | mk | connected,together,digital | Together for Tomorrow
IN-078 | Sun TV Network | #CC0000,#FFFFFF,#FFD700 | hum | wm | entertainment,trusted,regional | Entertainment for all
IN-079 | Zee Entertainment | #003087,#FFFFFF,#C0C0C0 | hum | wm | entertainment,diverse,mass | Extraordinary Together
IN-080 | Tata Consultancy Services | #003087,#FFFFFF,#C0C0C0 | hum | cmb | reliable,innovative,global | Building on belief
IN-081 | Wipro Lighting | #341F6A,#75B443,#FFFFFF | hum | mk | innovative,sustainable,smart | Lighting the way
IN-082 | Coal India | #003087,#FFFFFF,#FFD700 | hum | emb | essential,national,mass | Fuelling growth
IN-083 | REC Limited | #003087,#FFD700,#FFFFFF | hum | wm | infrastructure,reliable,national | Power for all
IN-084 | Grasim Industries | #003087,#FFFFFF,#C0C0C0 | hum | wm | textile,diversified,reliable | —
IN-085 | Hindalco | #CC0000,#003087,#FFFFFF | hum | wm | sustainable,global,metal-strength | Creating value, transforming lives
IN-086 | Ambuja Cements | #003087,#FF6600,#FFFFFF | hum | mk | strong,nation-building,quality | Building Strong India
IN-087 | ACC Limited | #CC0000,#FFFFFF,#000000 | hum | emb | trusted,strong,pioneering | Hai kuch baat
IN-088 | Emami | #003087,#FFD700,#FFFFFF | hum | mk | natural,healing,Indian,affordable | Making people feel better
IN-089 | Manappuram Finance | #003087,#FFD700,#FFFFFF | hum | wm | trusted,gold,regional | Where your gold gets real value
IN-090 | Dixon Technologies | #003087,#FFFFFF,#C0C0C0 | hum | wm | manufacturing,reliable,India-made | —
IN-091 | Coforge | #003087,#E8212D,#FFFFFF | hum | mk | digital,innovative,agile | Bringing clarity to digital
IN-092 | Tanla Platforms | #6C1D6E,#FFFFFF,#C0C0C0 | hum | wm | communications,reliable,digital | Make the world communicate better
IN-093 | InMobi | #003087,#FF6600,#FFFFFF | hum | mk | mobile-first,global,data-driven | Transform the way brands connect
IN-094 | Snapdeal | #E40046,#FFFFFF,#000000 | hum | emb | value,deals,budget | Unbox zindagi
IN-095 | Noise (Nexxbase) | #000000,#A9D234,#FFFFFF | hum | wm | youthful,smart,energetic,wearable | Charge up your life
IN-096 | Aurobindo Pharma | #003087,#FF6600,#FFFFFF | hum | wm | scientific,affordable,global | —
IN-097 | Zensar Technologies | #003087,#FFFFFF,#C0C0C0 | hum | mk | customer-centric,innovative,digital | Driving change together
IN-098 | Didi (India ops) | #FF6F00,#FFFFFF,#000000 | hum | mk | mobility,accessible,fast | Make travel better
IN-099 | Piramal Group | #003087,#C00000,#FFFFFF | cser | wm | trusted,healthcare,financial | Doing good and doing well
IN-100 | CRISIL | #003087,#FFFFFF,#FF6600 | hum | wm | analytical,trusted,India-ratings | Making markets function better
```

---

### 7.3 China (50)

```
CN-001 | Alibaba | #FF6A00,#FFFFFF,#1C1C1C | hum | mk | ambitious,merchant-first,digital,global | To make it easy to do business anywhere
CN-002 | Tencent | #1DB954,#003087,#FFFFFF | hum | mk | connecting,playful,expansive,social | Value for Users, Tech for Good
CN-003 | ICBC | #CC0000,#FFD700,#FFFFFF | hum | emb | trustworthy,national,stable | Your Trustworthy Bank
CN-004 | China Construction Bank | #CC0000,#003087,#FFFFFF | hum | mk | reliable,national,infrastructure | Building a Better Life
CN-005 | Agricultural Bank of China | #007A3D,#CC0000,#FFFFFF | hum | mk | agricultural,national,trustworthy | Growing together
CN-006 | Bank of China | #CC0000,#003087,#FFFFFF | hum | emb | global,trusted,century-old | Global Service
CN-007 | PetroChina | #003087,#CC0000,#FFD700 | hum | mk | national-energy,reliable,large-scale | Pursuing Excellence in Energy
CN-008 | China Mobile | #003087,#00AEEF,#FFFFFF | hum | mk | connected,mass,modern,reliable | Moving the World
CN-009 | Sinopec | #CC0000,#003087,#FFFFFF | hum | emb | national,large-scale,industrial | Energizing China
CN-010 | China Life | #CC0000,#FFD700,#FFFFFF | hum | mk | security,national,trustworthy | Lifetime Guardian
CN-011 | Ping An | #CC0000,#003087,#FFFFFF | hum | mk | comprehensive,tech-driven,secure | Professional Creates Value
CN-012 | China Merchants Bank | #CC0000,#FFFFFF,#000000 | geo | mk | customer-centric,innovative,progressive | You Are My Bank
CN-013 | Meituan | #FFD700,#000000,#FFFFFF | hum | mk | local,food-first,youthful,fast | Eat better, live better
CN-014 | JD.com | #CC0000,#FFFFFF,#000000 | hum | mk | authentic,fast,trust,quality | Joy of shopping
CN-015 | ByteDance/TikTok | #000000,#FF0050,#00F2EA | geo | mk | youthful,creative,global,addictive | Make every second count
CN-016 | Xiaomi | #FF6900,#FFFFFF,#1A1A1A | geo | lm | innovation-for-all,affordable,youthful | Innovation for everyone
CN-017 | Huawei | #CF0A2C,#FFFFFF,#000000 | hum | mk | innovative,trusted,global,pioneering | Building a Fully Connected World
CN-018 | China Telecom | #007AC2,#003087,#FFFFFF | hum | mk | reliable,connected,national | Create a better future
CN-019 | China Unicom | #CC0000,#003087,#FFFFFF | hum | mk | connected,modern,national | Aspire to be the Best
CN-020 | CNOOC | #003087,#FFD700,#FFFFFF | hum | mk | offshore,national,reliable | Creating Energy for a Better Life
CN-021 | Baidu | #2932E1,#FFFFFF,#C0C0C0 | hum | mk | intelligent,search-first,AI,national | Use Baidu to find what you need
CN-022 | NetEase | #CC0000,#FFFFFF,#000000 | hum | mk | creative,gaming,entertainment,youth | Creativity is the driving force
CN-023 | BYD | #1B3A6E,#CC0000,#FFFFFF | hum | wm | green-energy,innovative,EV,national | Build Your Dreams
CN-024 | CATL | #003087,#00AEEF,#FFFFFF | geo | wm | battery-innovation,green,global | Powered by CATL
CN-025 | SAIC Motor | #003087,#CC0000,#FFFFFF | hum | mk | automotive,national,reliable | In Cars We Trust
CN-026 | Kweichow Moutai | #CC0000,#FFD700,#FFFFFF | scr | emb | heritage,luxury,national,premium | China's National Liquor
CN-027 | Wuliangye | #CC0000,#FFD700,#FFFFFF | scr | emb | heritage,ceremonial,premium,culture | Quality, Tradition, Excellence
CN-028 | Geely Auto | #003087,#CC0000,#FFFFFF | hum | mk | global,quality,affordable | New Future
CN-029 | Lenovo | #CC0000,#FFFFFF,#000000 | hum | wm | smarter-technology,global,reliable | Smarter Technology for All
CN-030 | ZTE | #003087,#CC0000,#FFFFFF | hum | wm | connectivity,5G,global,innovative | Innovative together
CN-031 | Anta Sports | #CC0000,#FFFFFF,#000000 | cond | mk | champion,Chinese-sport,accessible | Keep Moving
CN-032 | Li Ning | #CC0000,#FFFFFF,#000000 | hum | mk | Chinese-pride,street-culture,bold | Anything is Possible
CN-033 | Haier | #CC0000,#FFFFFF,#000000 | hum | wm | smart-home,quality,trusted | From Haier, For the World
CN-034 | Midea | #CC0000,#003087,#FFFFFF | hum | mk | innovative,smart-home,quality,global | Make Yourself at Home
CN-035 | Gree Electric | #003087,#FFD700,#FFFFFF | hum | wm | quality,reliable,manufacturing | Mastering the Core Technology
CN-036 | Didi Chuxing | #FF6F00,#FFFFFF,#000000 | hum | mk | fast,accessible,China-mobility | Make travel better
CN-037 | Pinduoduo/Temu | #FF2B2B,#FFFFFF,#000000 | hum | mk | value,social,bargain,farm-to-consumer | Together, more savings, more fun
CN-038 | Trip.com | #003087,#FF6A00,#FFFFFF | hum | mk | travel,global,reliable | More Choices, More Fun
CN-039 | Bilibili | #00A1D6,#FB7299,#FFFFFF | hum | mk | youth,ACG,creative,community | You Like, I Like
CN-040 | Kuaishou | #FF4906,#FFFFFF,#000000 | hum | mk | real,grassroots,authentic,rural | Record the world, record you
CN-041 | Weibo | #FF8200,#FFFFFF,#000000 | hum | mk | open,trending,social,real-time | New Discovery, New Connections
CN-042 | Ant Group/Alipay | #1677FF,#FFFFFF,#000000 | geo | mk | trusted,digital,inclusive,China | Simplify the world's payment
CN-043 | NIO | #00AEEF,#000000,#FFFFFF | hum | mk | premium-EV,life-design,innovative | Your Natural Element
CN-044 | Shein | #000000,#FF3366,#FFFFFF | geo | wm | ultra-fast,youth,value,global | Stay Trendy
CN-045 | Country Garden | #007A3D,#FFD700,#FFFFFF | hum | mk | quality-life,green-living,luxury | Hope for the City
CN-046 | CITIC Group | #CC0000,#003087,#FFFFFF | hum | wm | national,trusted,diversified | —
CN-047 | China Railway | #003087,#CC0000,#FFFFFF | hum | emb | national,connectivity,engineering | —
CN-048 | China State Construction | #CC0000,#003087,#FFFFFF | hum | emb | nation-building,scale,reliable | Build a better life
CN-049 | Xiaohongshu (RED) | #FF2442,#FFFFFF,#000000 | hum | wm | lifestyle,aspirational,youth,sharing | Mark the good life
CN-050 | Mango TV / iQIYI | #00BE06,#000000,#FFFFFF | hum | wm | entertainment,streaming,creative | —
```

---

### 7.4 Europe (50)

```
EU-001 | LVMH (France) | #1A1A2E,#C9A84C,#FFFFFF | cser | emb | timeless,luxury,craftsmanship,exclusive | The art of luxury
EU-002 | Nestlé (Switzerland) | #CC0000,#FFFFFF,#000000 | hum | emb | nourishing,trusted,quality,family | Good Food, Good Life
EU-003 | ASML (Netherlands) | #003087,#00A3E0,#FFFFFF | geo | wm | precision,enabling,semiconductor,global | Moving the world forward
EU-004 | SAP (Germany) | #003087,#0070C0,#FFFFFF | geo | mk | intelligent-enterprise,reliable,global | Run Simple
EU-005 | Siemens (Germany) | #009999,#FFFFFF,#000000 | hum | wm | innovative,reliable,engineering,digital | Ingenuity for life
EU-006 | Volkswagen (Germany) | #003087,#C0C0C0,#FFFFFF | hum | mk | reliable,German,affordable,global | Das Auto
EU-007 | Allianz (Germany) | #003087,#FFFFFF,#C9A84C | hum | emb | trustworthy,global,secure,caring | With you every step of the way
EU-008 | TotalEnergies (France) | #CC0000,#003087,#FFCC00 | hum | mk | energy-transition,responsible,global | Energy is a common good
EU-009 | BNP Paribas (France) | #003087,#00965E,#FFFFFF | hum | mk | global,trusted,sustainable,modern | The bank for a changing world
EU-010 | Hermès (France) | #FF7700,#FFFFFF,#000000 | cser | emb | craftsmanship,exclusivity,timeless,artisanal | —
EU-011 | L'Oréal (France) | #000000,#FFFFFF,#C8A951 | hum | wm | science-of-beauty,inclusive,aspirational | Because you're worth it
EU-012 | AXA (France) | #003087,#FFFFFF,#FF1721 | hum | wm | trustworthy,caring,global,protective | Know you can
EU-013 | Airbus (France/Germany) | #003087,#00AEEF,#FFFFFF | hum | wm | innovative,pioneering,safe,global | Future in the sky
EU-014 | BASF (Germany) | #004A97,#FFFFFF,#E31837 | hum | wm | sustainable-chemistry,reliable | We create chemistry
EU-015 | Bayer (Germany) | #003087,#10A0C8,#FFFFFF | hum | mk | science,life-improving,trusted | Science for a better life
EU-016 | BMW (Germany) | #1C1C1C,#0066CC,#FFFFFF | geo | mk | premium,joy-of-driving,innovative,precise | The Ultimate Driving Machine
EU-017 | Mercedes-Benz (Germany) | #1C1C1C,#C0C0C0,#FFFFFF | hum | mk | luxury,prestige,engineering,timeless | The best or nothing
EU-018 | Adidas (Germany) | #000000,#FFFFFF,#FF0000 | hum | mk | performance,street-culture,iconic,sport | Impossible is Nothing
EU-019 | Inditex/Zara (Spain) | #000000,#FFFFFF,#C0C0C0 | cser | wm | trendy,accessible,fast-fashion,clean | —
EU-020 | Unilever (UK/Netherlands) | #003087,#0057A8,#FFFFFF | hum | mk | purposeful,sustainable,consumer-first | Making sustainable living commonplace
EU-021 | Shell (Netherlands/UK) | #FFD500,#CC0000,#FFFFFF | hum | mk | energy-transition,reliable,global | Let's go
EU-022 | Sanofi (France) | #7F2182,#FFFFFF,#C0C0C0 | hum | mk | life-saving,patient-first,innovative | Empowering life
EU-023 | Novartis (Switzerland) | #003087,#CB0060,#FFFFFF | hum | mk | science-driven,patient-focused,innovative | Reimagining medicine
EU-024 | Roche (Switzerland) | #003087,#C8102E,#FFFFFF | hum | mk | pioneering,diagnostic,patient-first | Doing now what patients need next
EU-025 | Philips (Netherlands) | #0B5ED7,#FFFFFF,#C0C0C0 | hum | mk | innovative,health-tech,human-centered | Innovation and you
EU-026 | ING (Netherlands) | #FF6200,#FFFFFF,#003087 | hum | emb | empowering,innovative,simple,warm | Do your thing
EU-027 | Santander (Spain) | #CC0000,#FFFFFF,#000000 | hum | mk | simple,personal,fair | Simple, Personal and Fair
EU-028 | HSBC (UK) | #CC0000,#FFFFFF,#000000 | hum | mk | global,trustworthy,local,premium | Open to a world of opportunity
EU-029 | BP (UK) | #007B40,#FFD700,#FFFFFF | hum | mk | energy-transition,responsible,progressive | Beyond Petroleum
EU-030 | Rio Tinto (UK/Australia) | #CC0000,#FFFFFF,#000000 | hum | wm | sustainable,reliable,global,mining | Finding better ways
EU-031 | Diageo (UK) | #003087,#FFFFFF,#C9A84C | cser | wm | celebrating-life,premium,global | Celebrating life, every day
EU-032 | Rolls-Royce (UK) | #002B5C,#C0C0C0,#FFFFFF | cser | emb | engineering,precision,iconic,British | Inspiring the next generation
EU-033 | GSK (UK) | #F36523,#FFFFFF,#000000 | hum | mk | ambitious,scientific,trusted,bold | Ahead together
EU-034 | AstraZeneca (UK/Sweden) | #003087,#C00000,#FFFFFF | hum | wm | patient-first,scientific,innovative | What science can do
EU-035 | Ericsson (Sweden) | #003087,#00A3E0,#FFFFFF | hum | wm | innovative,5G,trusted,global | Connecting the World
EU-036 | Volvo (Sweden) | #003087,#C0C0C0,#FFFFFF | hum | emb | safety,Scandinavian,sustainable,trusted | For life
EU-037 | H&M (Sweden) | #CC0000,#FFFFFF,#000000 | hum | wm | accessible,sustainable,inclusive | Fashion and quality at the best price
EU-038 | IKEA (Sweden) | #003087,#FFD700,#FFFFFF | hum | emb | democratic-design,accessible,Scandinavian | The wonderful everyday
EU-039 | Novo Nordisk (Denmark) | #003087,#C8102E,#FFFFFF | hum | emb | diabetes,patient-focused,innovative | Changing lives for good
EU-040 | Maersk (Denmark) | #003087,#FFD700,#FFFFFF | hum | mk | global-logistics,reliable,essential | All the way
EU-041 | ABB (Switzerland) | #CC0000,#FFFFFF,#000000 | hum | mk | electrification,automation,reliable | Powering smarter
EU-042 | Zurich Insurance (Switzerland) | #003087,#FFFFFF,#C0C0C0 | hum | mk | trustworthy,caring,global,precise | The Zurich way
EU-043 | UBS (Switzerland) | #CC0000,#FFFFFF,#000000 | cser | emb | wealth,trusted,elite,Swiss-precision | You're not alone
EU-044 | Stellantis (Netherlands) | #003087,#C0C0C0,#FFFFFF | hum | mk | mobility,global,accessible | Freedom of Movement
EU-045 | Renault (France) | #FFD700,#000000,#FFFFFF | geo | mk | French-design,accessible,electric,bold | Passion for Life
EU-046 | Pernod Ricard (France) | #FFD700,#003087,#FFFFFF | cser | mk | conviviality,premium,French-heritage | Créateurs de convivialité
EU-047 | Capgemini (France) | #003087,#0070AD,#FFFFFF | hum | mk | innovative,collaborative,tech,responsible | Get the future you want
EU-048 | Booking.com (Netherlands) | #003580,#FFFFFF,#FFCC00 | hum | mk | simple,best-price,traveler-first | Booking.yeah
EU-049 | Ferrari (Italy) | #CC0000,#FFD700,#000000 | cser | emb | racing,passion,exclusivity,Italian | We are the competition
EU-050 | Spotify (Sweden) | #1DB954,#000000,#FFFFFF | hum | mk | music,joyful,discovery,free-spirited | Music for everyone
```

---

## 8. Custom Brand Intake

Use this when the user selects "My own brand" or when a brand is not in Section 7.

### Required questions (always ask — 5 max)

1. **Primary color** — "What is your main brand color? (hex code, or describe it)"
2. **Secondary + accent** — "Any supporting colors? (or say 'auto' to let me choose complements)"
3. **Font personality** — "Which feels closest to your brand's type style?"
   - Options: Clean & modern (geometric sans) | Friendly & approachable (humanist sans) | Bold & punchy (condensed) | Classic & trustworthy (serif) | Elegant & refined (modern serif) | Expressive (display/script)
4. **3 personality words** — "Describe your brand in 3 words (e.g. bold, minimal, warm)"
5. **Industry / context** — "What industry or sector is this for?"

### Extended questions (Full fidelity only — ask after above)

6. Logo style — wordmark / lettermark / mark only / emblem / combination
7. Heading preference — Title Case / ALL CAPS / Sentence case / all lowercase
8. Layout preference — Full-bleed color / Centered / Editorial margins / Grid / Minimal white space
9. Imagery direction — Action/people / Product hero / Abstract / Lifestyle / Illustration
10. Corner style — Sharp / Subtle rounded / Rounded / Pill-shaped

### Normalisation rules

- If user gives a color name (e.g. "deep navy"), convert to closest hex: deep navy → #003087
- If secondary/accent = "auto": generate two harmonious variants from primary (split-complementary or analogous)
- If font personality = "Clean & modern" → font_vibe = "geometric-sans", font_primary = "Inter or DM Sans"
- If personality words include "luxury" or "premium" → default layout_pref = "minimal", border_radius = "none"
- If personality words include "bold" or "energetic" → default layout_pref = "full-bleed", heading_case = "upper"
- If industry = "healthcare" or "finance" → default tone = trustworthy, safe, clear (override user words if conflicting)

---

## 9. Social Media Formats Reference

Read this section whenever output type = "social media creative". Pick the exact platform and format before generating.

**Global design principles for social creatives:**
- Always work at 2x resolution mentally (design at the stated px, export/render at 2x for retina)
- Keep all critical content (text, faces, logos) within the safe zone — content outside may be cropped on some devices
- Text overlay: keep text coverage below 20% of image area (Facebook/Instagram suppress reach above this)
- Minimum font size: 24px for body, 36px for headlines (at stated dimensions)
- Always include a visual hierarchy: one dominant element, one supporting element, one CTA

---

### 9.1 Instagram

| Format | Dimensions (px) | Aspect ratio | Safe zone | Char limits | Key notes |
|---|---|---|---|---|---|
| Feed — square | 1080 × 1080 | 1:1 | 50px all sides | Caption: 2200 · Hashtags: 30 | Most universal; safe for all placements |
| Feed — portrait | 1080 × 1350 | 4:5 | 50px all sides | Caption: 2200 · Hashtags: 30 | Max feed real estate; best for mobile |
| Feed — landscape | 1080 × 566 | 1.91:1 | 60px all sides | Caption: 2200 · Hashtags: 30 | Good for wide scenes; less impact mobile |
| Stories | 1080 × 1920 | 9:16 | Top/bottom 14% (268px) · Sides 7% (76px) | Text overlay only | Tap zones top/bottom; avoid placing CTAs there |
| Reels | 1080 × 1920 | 9:16 | Top 14%, bottom 20% (UI overlays) | Caption: 2200 | Native/authentic style outperforms polished |
| Carousel slide | 1080 × 1080 | 1:1 | 50px all sides | Per-slide: no limit | Up to 10 slides; first = hook, last = CTA; consistent style across all slides |
| Profile photo | 320 × 320 min | 1:1 | Circle crop; 20px inner margin | — | Displayed at 110px; no fine text |

**Instagram design principles:**
- Vertical formats (4:5, 9:16) consistently outperform square and landscape on engagement
- First frame of Reels must work as a static thumbnail
- Carousel swipe line: leave a visual "peek" of next slide at right edge to signal swipeability
- Stories CTAs: place swipe-up / link button zone in bottom 20% only

---

### 9.2 LinkedIn

| Format | Dimensions (px) | Aspect ratio | Safe zone | Char limits | Key notes |
|---|---|---|---|---|---|
| Feed image post | 1200 × 628 | 1.91:1 | 60px all sides | Post text: 3000 · Headline: 150 | Standard share; also renders as 1200×627 |
| Document / carousel | 1080 × 1080 or 1080 × 1350 | 1:1 or 4:5 | 50px all sides | Per-page description | PDF upload; up to 300 pages; page 1 = thumbnail |
| Article cover | 1128 × 376 | 3:1 | 80px all sides | — | Used in articles and newsletters |
| Company banner | 1536 × 768 | 2:1 | 80px all sides | — | Desktop crop; keep logo top-left or centred |
| Personal profile banner | 1584 × 396 | 4:1 | Left 220px (avatar overlap) · Top/bottom 40px | — | Profile photo overlaps bottom-left on desktop |

**LinkedIn design principles:**
- Professional tone; avoid overly casual aesthetics
- Document carousels perform best with data, insights, or step-by-step frameworks
- First page must communicate full value proposition — used as preview thumbnail
- Text-heavy images underperform; lead with visual, support with text

---

### 9.3 X / Twitter

| Format | Dimensions (px) | Aspect ratio | Safe zone | Char limits | Key notes |
|---|---|---|---|---|---|
| In-stream image | 1200 × 675 | 16:9 | 60px all sides | Tweet: 280 · Alt text: 1000 | Feed preview crops to ~2:1; full image on expand |
| Multi-image (2 imgs) | 1200 × 675 each | 16:9 | 60px all sides | Tweet: 280 | Side-by-side layout auto-applied |
| Multi-image (4 imgs) | 1200 × 675 each | 16:9 | 60px all sides | Tweet: 280 | 2×2 grid auto-applied |
| Profile header | 1500 × 500 | 3:1 | Centre 60% safe · Sides may crop on mobile | — | Keep key brand elements centred |
| Profile photo | 400 × 400 | 1:1 | Circle crop; 30px inner margin | — | Displayed at 48px in feed |

**X / Twitter design principles:**
- Short shelf life — bold visuals that read in 0.5 seconds
- Alt text is mandatory for accessibility
- Cards: when sharing links, OG image (1200×630) overrides tweet image

---

### 9.4 Facebook

| Format | Dimensions (px) | Aspect ratio | Safe zone | Char limits | Key notes |
|---|---|---|---|---|---|
| Feed post — photo | 1200 × 1200 | 1:1 | 50px all sides | Post: 63206 · Link description: 250 | Square performs best on mobile feed |
| Feed post — link | 1200 × 630 | 1.91:1 | 60px all sides | Headline: 25 · Description: 30 | Override with Open Graph meta tags |
| Story | 1080 × 1920 | 9:16 | Top/bottom 14% (268px) · Sides 7% (76px) | Caption: 2200 | 24-hour lifespan; interactive stickers supported |
| Cover photo | 820 × 312 | 2.63:1 | Centre 640×312 safe | — | Mobile crops to 640×360; focus centred |
| Event cover | 1920 × 1005 | 1.91:1 | 100px all sides | Event name: 64 | Shown at various sizes; avoid edge text |
| Group cover | 1640 × 856 | 1.91:1 | 100px all sides | — | — |

**Facebook design principles:**
- Algorithm favors video and carousel over static images
- Minimal text on image (below 20%) for best reach
- Stories: vertical real estate — use full height, place CTA mid-screen

---

### 9.5 YouTube

| Format | Dimensions (px) | Aspect ratio | Safe zone | Char limits | Key notes |
|---|---|---|---|---|---|
| Video thumbnail | 1280 × 720 | 16:9 | 40px all sides | Title: 100 · Description: 5000 | Single most impactful asset; high contrast essential |
| Channel art | 2560 × 1440 | 16:9 | Safe zone: 1546 × 423 centred | Channel name: 100 | Safe zone visible on all devices; rest may crop |
| Profile photo | 800 × 800 | 1:1 | Circle crop; 40px inner margin | — | Displayed at 98px in search results |
| Shorts thumbnail | 1080 × 1920 | 9:16 | Same as Stories | — | Auto-generated from video if not uploaded |
| End screen elements | 1280 × 720 | 16:9 | Outer 10% reserved for end screen zone | — | Clickable elements placed last 20 seconds |

**YouTube design principles:**
- Thumbnail rule of three: one face, one bold text element, one color pop
- Channel art: test on TV (2560px wide), desktop (max 2120px), mobile (1546×423 safe zone)
- Shorts: first 3 seconds must hook — native/candid outperforms polished

---

### 9.6 TikTok

| Format | Dimensions (px) | Aspect ratio | Safe zone | Char limits | Key notes |
|---|---|---|---|---|---|
| Feed video | 1080 × 1920 | 9:16 | Top 8% (154px) · Bottom 25% (480px) · Right 10% (108px) for UI buttons | Caption: 2200 · Hashtags: no hard limit | Native/lo-fi aesthetic outperforms polished |
| Profile photo | 200 × 200 | 1:1 | Circle crop; 20px inner margin | Username: 24 · Bio: 80 | — |
| Spark Ad / TopView | 1080 × 1920 | 9:16 | Same as feed video | Headline: 50 · CTA: 25 | Must feel native; avoid obvious ad look |
| Static image post | 1080 × 1080 or 1080 × 1350 | 1:1 or 4:5 | 50px all sides | Caption: 2200 | Image posts available in select regions |

**TikTok design principles:**
- Platform rewards authenticity over polish — brand aesthetic should feel native, not broadcast
- Sound is default-on — design for both sound-on and sound-off
- Text overlays must avoid the right-side UI button zone and bottom caption area
- First 1–3 seconds are critical for completion rate

---

### 9.7 Pinterest

| Format | Dimensions (px) | Aspect ratio | Safe zone | Char limits | Key notes |
|---|---|---|---|---|---|
| Standard pin | 1000 × 1500 | 2:3 | 30px all sides | Title: 100 · Description: 500 | Optimal ratio; more vertical = more screen = more saves |
| Square pin | 1000 × 1000 | 1:1 | 30px all sides | Title: 100 · Description: 500 | Less preferred but grid-consistent |
| Long/tall pin | 1000 × 2100 | 1:2.1 | 30px all sides | Title: 100 · Description: 500 | Max allowed; use for step-by-step or infographic |
| Idea pin (Story) | 1080 × 1920 | 9:16 | Top/bottom 12% | Title: 100 · per-page text | Up to 20 pages; built-in text overlay tools |
| Shopping pin | 1000 × 1500 | 2:3 | 30px all sides | Product title: 100 · Price required | Lifestyle product photo outperforms flat lay |

**Pinterest design principles:**
- Vertical always wins — 2:3 is the sweet spot
- Warm, bright, high-contrast images outperform dark/moody
- Text overlay: one clear value prop, large serif or bold sans
- Seasonal content: pin 30–45 days before the season (long content shelf life)

---

### 9.8 WhatsApp

| Format | Dimensions (px) | Aspect ratio | Safe zone | Char limits | Key notes |
|---|---|---|---|---|---|
| Status | 1080 × 1920 | 9:16 | Top/bottom 15% · Sides 5% | Caption: 700 | Disappears in 24h; link in bio not clickable in status |
| Channel banner | 1600 × 800 | 2:1 | 100px all sides | Channel name: 25 | Cropped on mobile; keep brand centred |
| Shared image | 1200 × 630 recommended | 1.91:1 | 40px all sides | Message: 65536 | Compressed on send; keep file <5MB for quality |

**WhatsApp design principles:**
- Personal, conversational feel works better than broadcast advertising
- Status: 7-second view; bold single message only
- Compression: JPEG quality drops on send — use high-contrast designs that survive compression

---

### 9.9 Google Business Profile

| Format | Dimensions (px) | Aspect ratio | Safe zone | Char limits | Key notes |
|---|---|---|---|---|---|
| Business post image | 1200 × 900 | 4:3 | 80px all sides | Post text: 1500 · CTA: button label | Shown in Maps + Search; minimal text performs best |
| Cover photo | 1080 × 608 | 16:9 | 60px all sides | — | Primary visual on Business Profile |
| Profile/logo photo | 250 × 250 min | 1:1 | Circle crop; 20px inner margin | — | Represents brand in search |
| Product photo | 1200 × 900 | 4:3 | 40px all sides | Product name: 58 · Description: 1000 | Clean product-hero; white or simple background |

**Google Business design principles:**
- Authentic photos (actual location/product) rank better than stock
- Cover photo: no text overlays — Google may flag promotional images
- Post images: convey the offer visually; text in the post body, not image

---

### 9.10 Cross-platform quick reference

| Platform | Best format | Worst mistake | Brand token tip |
|---|---|---|---|
| Instagram | 4:5 portrait feed | Tiny text in stories | Use primary_color as full-bleed background |
| LinkedIn | 1.91:1 image or carousel | Casual tone/aesthetics | Use secondary_color for data callouts |
| X / Twitter | 16:9 with bold hook | Text-heavy image | Use accent_color for single bold stat |
| Facebook | 1:1 square | Image text > 20% | Split primary/secondary between image zones |
| YouTube | 1280×720 thumbnail | Plain title card | High-contrast: primary bg + white text |
| TikTok | 9:16 native-feel video | Over-polished / ad-like | Limit to one brand color; keep rest natural |
| Pinterest | 2:3 vertical | Dark or low-contrast | Warm tones; brand color on text overlay zone |
| WhatsApp | 9:16 status | Long text / multiple messages | One-line message; primary_color background |
| Google Business | 4:3 authentic photo | Stock images / text overlays | Logo in corner; no heavy brand treatment |

---

*End of BRAND_CREATIVE.md — All data self-contained. Add this file as context in any AI assistant to activate brand-aware creative generation. See `adapters/` for tool-specific setup notes.*
