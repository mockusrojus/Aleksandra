# Skill: design-ecommerce

## When to activate
Use this skill when the user asks to build a multi-page website for selling products or services. Trigger phrases: "e-commerce site", "online shop", "product website", "store website", "selling products", "multi-page site".

---

## Phase 0 — Design reference (if URL provided)

If the user provided a design reference URL, run the `capture-reference` skill first. It will:
- Screenshot the reference site
- Extract and summarize the design language (colors, fonts, vibe)
- Present findings and wait for confirmation

Only proceed to Phase 1 after the user confirms the direction.

---

## Phase 1 — Clarify before building

Ask the user:
1. **Brand name** and what they're selling
2. **How many products** (handful vs large catalogue)
3. **Language** of the site
4. **Do they have a logo or photos?** (save as `logo.png` / `product-1.png` etc. in the site folder)
5. **Any specific pages** beyond the standard set?

Only proceed once you have the brand name and product type.

---

## Phase 2 — Project setup

Create the site at: `Projekts/<brand-name>/`

```
Projekts/<brand-name>/
├── index.html       ← Home
├── shop.html        ← Product grid
├── product.html     ← Single product detail (reusable template)
├── contact.html     ← Contact form + info
├── style.css        ← Shared styles (all pages import this)
└── assets/
    ├── logo.png
    └── product-1.png, product-2.png ...
```

Google Fonts link goes in every HTML file's `<head>`. All pages link to `style.css`. No JS frameworks — vanilla only.

---

## Phase 3 — Design system (same as landing page)

Colors are provided per project. Apply them using the **60/30/10 rule**:
- **60%** → dominant: backgrounds and white space
- **30%** → secondary: dark sections, text, structural elements
- **10%** → accent: CTAs, highlights, hover states only

`style.css` opens with these variables:
```css
:root {
  --max-width: 105rem;

  /* Colors — fill in per project brief */
  --color-dominant:    /* 60% */;
  --color-secondary:   /* 30% */;
  --color-accent:      /* 10% */;
  --text-on-dark:      #ffffff;
  --text-on-dark-sub:  rgba(255,255,255,0.55);
  --text-on-light:     /* derive from secondary */;
  --text-on-light-sub: /* 50% opacity of text-on-light */;

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

---

## Phase 4 — Build each page

### Shared nav (same across all pages)
- Sticky, `--dark` background
- Logo (serif italic) left
- Links: Home · Shop · Contact
- Cart icon + item count badge right
- Active page link highlighted (sage color)
- Hamburger on mobile

### Shared footer
- Dark background, 3 columns: brand + tagline | quick links | contact info
- Bottom bar: copyright

---

### index.html — Home

| Section | Notes |
|---------|-------|
| Hero | Full-viewport dark. Large serif headline, subline, 2 CTAs (Shop Now + Learn More). Product/brand image right. |
| Featured products | 3-col card grid. Each: product image, name, price, "Add to cart" button. Cream background. |
| Categories | 2–4 category tiles with background image + label overlay. |
| Brand story | 2-col: image left, text right. Dark background. Who we are, what makes us different. |
| Testimonials | 3-col review cards. Star rating, quote, name. Soft-white background. |
| CTA banner | Full-width cream strip. Headline + button linking to shop. |

---

### shop.html — Product Grid

| Section | Notes |
|---------|-------|
| Page header | Dark bg, page title in serif italic, breadcrumb (Home › Shop) |
| Filter bar | Horizontal pill buttons for categories. Cream background. |
| Product grid | 3-col (desktop), 2-col (tablet), 1-col (mobile). Each card: image, name, short desc, price, "Add to cart" button. White cards, shadow. |
| Pagination | Simple prev / next or numbered. |

Product card structure:
```html
<div class="product-card">
  <div class="product-img-wrap"><img src="assets/product-1.png" alt="Product name" /></div>
  <div class="product-info">
    <h3 class="product-name">Product Name</h3>
    <p class="product-desc">Short description</p>
    <div class="product-footer">
      <span class="product-price">€49</span>
      <button class="btn-cart">Add to cart</button>
    </div>
  </div>
</div>
```

---

### product.html — Single Product

| Section | Notes |
|---------|-------|
| Breadcrumb | Home › Shop › Product Name |
| Product detail | 2-col: image gallery left (main + thumbnails), details right (name, price, description, size/variant selector, quantity, Add to cart button) |
| Product details tabs | Description · Ingredients/Specs · Shipping |
| Related products | 3-col grid, same card style as shop.html |

---

### contact.html — Contact

| Section | Notes |
|---------|-------|
| Page header | Dark bg, serif italic title |
| Contact split | 2-col: form left, info right (address, phone, email, hours, map embed placeholder) |
| Form fields | Name, Email, Subject, Message, Submit button |

Form uses `mailto:` action — no backend required.

---

## Phase 5 — After building

1. Open `index.html` in browser: `start "Projekts/<brand-name>/index.html"`
2. If Puppeteer MCP is available, take a full-page screenshot of each page and show them to the user for review
3. Tell the user:
   - How to add real product images (replace Unsplash placeholders with files in `assets/`)
   - That the cart is visual only — real checkout needs a payment integration (Stripe, Shopify Buy Button, etc.)
   - What placeholder text remains to be filled in

---

## Content rules

- **Language:** match the brand's target market
- **Prices:** use realistic placeholder prices; user replaces with real ones
- **Product images:** Unsplash placeholders until user provides real photos
- **Cart functionality:** visual/UI only unless user explicitly asks for JS cart logic
- **Don't over-engineer:** no React, no Node, no database — just clean HTML/CSS/JS
