# Skill: design-landing-page

## When to activate
Use this skill when the user asks to build a personal brand website, landing page, or single-page site for a person or service. Trigger phrases: "build a landing page", "create a website for", "personal brand site", "one-page site".

---

## Phase 0 — Design reference (if URL provided)

If the user provided a design reference URL, run the `capture-reference` skill first. It will:
- Screenshot the reference site
- Extract and summarize the design language (colors, fonts, vibe)
- Run person research in parallel
- Present findings and wait for confirmation

Only proceed to Phase 1 after the user confirms the direction.

---

## Phase 1 — Research first (always)

Before writing a single line of HTML, gather real information:

1. **Web search** the person's name + profession to find:
   - Current workplace and address
   - Phone and email
   - Education and work history
   - Profile pages (manodaktaras.lt, LinkedIn, Facebook, clinic sites, rekvizitai.lt)
   - Any photos (check all profile pages for img src URLs)
   - Patient/client reviews (verbatim quotes if available)

2. **Fetch** every promising URL — clinic websites, professional directories, social profiles
3. **Note what's missing** — mark those sections as placeholders so the user knows to fill them in
4. **Ask the user** for a photo if none found online — they save it as `image.png` in the site folder

Only start building after research is complete.

---

## Phase 2 — Project setup

Create the site at: `Projekts/<site-name>/index.html`

Name the folder after the person or brand (lowercase, no spaces, e.g. `aleksandra`, `john-smith`).

---

## Phase 3 — Build the page

Single file: all CSS in `<style>`, all JS at bottom in `<script>`. No external dependencies except Google Fonts.

### Head
```html
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,500;1,300;1,400;1,500&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet" />
```

### Design system
Colors are provided per project. Apply them using the **60/30/10 rule**:
- **60%** → dominant: backgrounds and white space
- **30%** → secondary: dark sections, text, structural elements
- **10%** → accent: CTAs, highlights, hover states only

Always declare as CSS custom properties. Include the type scale (base 16px, ratio 1.25):

```css
:root {
  --max-width: 105rem;

  /* Colors — fill in per project brief */
  --color-dominant:   /* 60% */;
  --color-secondary:  /* 30% */;
  --color-accent:     /* 10% */;
  --text-on-dark:     #ffffff;
  --text-on-dark-sub: rgba(255,255,255,0.55);
  --text-on-light:    /* derive from secondary */;
  --text-on-light-sub:/* 50% opacity of text-on-light */;

  --card-shadow:       0px 2px 8px rgba(0,0,0,0.08), 0px 1px 2px rgba(0,0,0,0.10);
  --card-shadow-hover: 0px 8px 24px rgba(0,0,0,0.14);
  --serif: 'Playfair Display', serif;
  --sans:  'DM Sans', sans-serif;

  /* Type scale — Major Third (×1.25) */
  --text-xs:   0.64rem;   /* 10.24px — labels, badges */
  --text-sm:   0.8rem;    /* 12.8px  — captions, meta */
  --text-base: 1rem;      /* 16px    — body copy */
  --text-md:   1.25rem;   /* 20px    — lead text, card titles */
  --text-lg:   1.5625rem; /* 25px    — subheadings */
  --text-xl:   1.953rem;  /* 31.25px — section headings */
  --text-2xl:  2.441rem;  /* 39.06px — page headings */
  --text-3xl:  3.052rem;  /* 48.83px — hero headline */
}
```

### Conversion principles applied per section

#### General rules (apply everywhere)
- **F-pattern layout:** place the most important content top-left; let text and visuals flow left → right, then down
- **Whitespace:** section padding = `9rem 0` top/bottom (1.5× the base 6rem)
- **Type scale:** always use `--text-*` tokens — never hardcode font sizes
- **60/30/10 colors:** accent (`--color-accent`) only on primary CTAs, active states, and key highlights — nowhere else

---

#### Section-by-section instructions

**1 — Nav** *(60% dominant bg)*
- **Hick's Law:** maximum 5 navigation links — fewer choices = faster decisions
- Logo (serif italic) left; links right; one CTA button in accent color
- Hamburger on mobile
- CTA button must use `--color-accent` — it is the only accent element in the nav

**2 — Hero** *(30% secondary bg — full viewport)*
- **PAS copywriting framework** for H1 + subtext:
  - **Problem:** H1 names the visitor's core pain directly (e.g. "Struggling with…", "Tired of…")
  - **Agitate:** subline makes the cost of inaction vivid and personal
  - **Solution:** a second line or badge positions the subject as the direct answer
- **F-pattern:** H1 top-left, subtext below, CTAs below that — eye lands left and flows down naturally
- **Fitts's Law:** primary CTA button must be large (`padding: 1rem 2.5rem`, `font-size: var(--text-md)`) and placed directly below the subtext where the eye already rests. Secondary CTA smaller and lower contrast.
- **Social proof above the fold:** include a small badge or trust strip in the hero (e.g. "★ 4.9 — 120+ clients" or a single short pull-quote). This is the only social proof element that must appear without scrolling.
- H1: `var(--text-3xl)`, Playfair Display italic; subtext: `var(--text-md)`, DM Sans 300

**3 — Services** *(60% dominant bg — 4-col card grid)*
- **F-pattern:** card titles left-aligned, descriptions below — never centered body text in cards
- **Hick's Law:** 4 services maximum — if there are more, group them
- Card: SVG icon top, serif title `var(--text-lg)`, description `var(--text-base)`, soft shadow

**4 — About** *(30% secondary bg — 2-col)*
- Photo left (F-pattern entry point), bio text right
- Education + Work Experience as vertical timelines
- Bio lead line `var(--text-md)`; body `var(--text-base)`

**5 — Reviews** *(60% dominant bg — 3-col cards)*
- Star rating SVG, italic pull-quote `var(--text-md)`, reviewer name `var(--text-sm)`
- Use verbatim quotes from research where possible
- Note: the hero badge already covers above-the-fold social proof; this section provides depth

**6 — Statistics** *(60% dominant bg — 4-col strip)*
- 4 numbers only — **Hick's Law:** don't list more
- Number: `var(--text-3xl)` serif italic; label: `var(--text-sm)` uppercase spaced

**7 — Booking** *(60% dominant bg)*
- 5-col day grid Mon–Fri; available/full slot chips
- **Fitts's Law:** email CTA button large and centered below the grid

**8 — Contact** *(30% secondary bg)*
- **Fitts's Law:** final CTA button large, centered, `--color-accent` — last conversion opportunity
- Email, phone, address, hours with icons; tagline above

**9 — Footer** *(darkest available color)*
- Name left, copyright right — minimal, no extra links

### Responsive breakpoints
- `1024px`: 2-col grids, about stacks
- `768px`: 1-col, hamburger nav, hero image hidden

### JS (at bottom)
- Mobile nav toggle
- Smooth scroll with 72px offset for fixed nav

---

## Phase 4 — After building

1. Open the file in the browser: `start "Projekts/<site-name>/index.html"`
2. If Puppeteer MCP is available, take a full-page screenshot automatically and show it to the user so they can review without opening the browser manually
3. Tell the user:
   - What's real vs placeholder
   - To save their photo as `image.png` in the site folder if not already done
   - What info still needs to be filled in (education dates, missing contacts, etc.)

---

## Content rules

- **Language:** match the subject's language (Lithuanian site → Lithuanian text, English → English)
- **Reviews:** use verbatim quotes from real sources if found; otherwise write realistic placeholders and flag them
- **Photos:** use `image.png` for the person's photo; Unsplash URLs only for review avatars
- **Placeholders:** always mark with `— papildyti —` (LT) or `— fill in —` (EN) so the user sees them
- **Don't invent:** education, credentials, or contact details — mark as placeholder if not found
