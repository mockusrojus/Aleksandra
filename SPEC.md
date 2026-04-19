# Site Specification

## SEO Requirements

### Semantic HTML
- Proper heading hierarchy on all pages
- One H1 per page, H2 for sections, H3 for subsections

### Meta Tags
- Unique meta title: 50–60 characters per page
- Unique meta description: 150–160 characters per page
- Open Graph and Twitter Card meta tags on all pages
- Canonical URL on every page to prevent duplicate content

### Structured Data
- JSON-LD on homepage, about, and blog pages

### Crawling
- `robots.txt` at site root — block `/api` routes from crawlers
- `sitemap.xml` auto-generated; update when pages are added or removed

### Images
- WebP format, compressed
- Descriptive `alt` text on every image
- Explicit `width` and `height` attributes on every `<img>` to prevent layout shift (CLS)

### Performance (Core Web Vitals)
- LCP under 2.5s
- No layout shift (CLS = 0)
- Fast FID / INP

### Responsive
- Mobile responsive at all breakpoints (see CLAUDE.md for breakpoints)
