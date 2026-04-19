# Claude Instructions

## Project Overview
Building websites — personal brand landing pages and multi-page e-commerce sites — stored in `Projekts/`. Each site gets its own subfolder.

```
Praktika/
├── Claude.MD
├── .claude/
│   └── skills/
│       ├── design-landing-page/SKILL.md
│       └── design-ecommerce/SKILL.md
└── Projekts/
    ├── aleksandra/
    │   ├── index.html
    │   └── image.png
    └── <next-site>/
```

---

## Skills

Skills live in `.claude/skills/`. Each Skill = a `SKILL.md` file with instructions Claude reads to know how to execute that type of task. Claude auto-activates the right skill based on the request.

- `capture-reference` — **runs first whenever a design reference URL is provided.** Uses Puppeteer to screenshot the site, extract CSS custom properties, and analyze the design language. For landing pages, runs a web search on the subject person in parallel. Presents findings before any build work starts.
- `design-landing-page` — single-page personal brand sites
- `design-ecommerce` — multi-page product/shop sites

Don't create new skills without asking first.

---

## Web Design Preferences

### Stack
- **Single-page sites:** one `index.html` with embedded `<style>` and vanilla JS — no frameworks, no build tools
- **Multi-page sites:** separate `.html` per page + one shared `style.css` + `assets/` folder
- **Fonts:** Google Fonts only (no commercial fonts)
- **Images:** local files preferred; Unsplash URLs for placeholders

### Design System
- **Aesthetic:** minimal, elegant, calm — inspired by Assembly Coffee
- **Headings:** Playfair Display — italic, weight 300–400
- **Body:** DM Sans — weight 300–500
- **Max width:** `105rem`, container padding `2.5rem`
- **Card shadow:** `0px 2px 8px rgba(42,61,56,0.08), 0px 1px 2px rgba(42,61,56,0.10)`

Colors, type scale, and responsive breakpoints are defined in the skill files.

### Landing Page — Standard Sections
1. Sticky nav (dark bg, logo left, links right, CTA button)
2. Hero (full-viewport, dark, large italic serif H1, photo, badge)
3. Services / categories (cream bg, 4-col card grid)
4. About (dark bg, 2-col: photo + bio, education + experience timelines)
5. Reviews (soft white, 3-col cards, star ratings)
6. Statistics (cream bg, 4 numbers in a row)
7. Booking (availability grid Mon–Fri, open/full slots)
8. Contact (dark bg, email + phone + address + hours)
9. Footer (minimal dark)

### E-commerce — Standard Pages
- `index.html` — hero + featured products + categories
- `shop.html` — product grid with filters
- `product.html` — single product detail
- `contact.html` — contact form + info

---

## SEO Rules (apply to every page by default)

- **Internal links:** use plain `<a href="...">` with descriptive anchor text — never bare URLs
- **Images:** always include explicit `width` and `height` attributes to prevent layout shift
- See [`SPEC.md`](SPEC.md) for all other SEO and performance requirements

## Specifications

Full site requirements (SEO, performance, structured data) are in [`SPEC.md`](SPEC.md). Follow all rules there on every build.

---

## Operating Principles

- Always read existing files before editing
- Do web searches to find real info (photos, contacts, bio) before using placeholders
- Keep placeholder text clearly marked so the user knows what to fill in
- Open the browser after building so the user can see the result immediately
- Don't add frameworks, dependencies, or complexity that wasn't asked for
- When something is unclear, ask — don't assume
