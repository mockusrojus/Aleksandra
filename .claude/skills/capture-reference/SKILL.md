# Skill: capture-reference

## When to activate
Use this skill when the user provides a design reference URL before building a site. This runs as **Phase 0** before any design or build work begins. Trigger phrases: "use this as reference", "based on this design", "copy the style of", "inspired by this site", or when the user pastes any URL alongside a build request.

---

## What this skill does

Autonomously (no user input needed):
1. Navigate to the reference URL with Puppeteer
2. Take a full-page screenshot
3. Extract all CSS custom properties and key computed styles from the page
4. Analyze the design language from the screenshot
5. If the project is a personal brand landing page — web-search the subject person
6. Present findings and wait for user feedback before building

---

## Phase 0-A — Capture the reference site

### Step 1: Navigate
```
puppeteer_navigate → { url: "<reference-url>" }
```

### Step 2: Full-page screenshot
```
puppeteer_screenshot → { name: "reference", fullPage: true }
```

### Step 3: Extract styles
Run this via `puppeteer_evaluate`:
```js
(() => {
  const vars = {};
  const computed = getComputedStyle(document.documentElement);

  // Extract all CSS custom properties from every stylesheet
  for (const sheet of document.styleSheets) {
    try {
      for (const rule of sheet.cssRules) {
        if (rule.style) {
          for (const prop of rule.style) {
            if (prop.startsWith('--')) {
              vars[prop] = rule.style.getPropertyValue(prop).trim();
            }
          }
        }
      }
    } catch (e) {} // cross-origin sheets will throw — skip them
  }

  // Key computed styles from root
  const keys = ['font-family', 'font-size', 'line-height', 'color', 'background-color'];
  const rootStyles = {};
  for (const k of keys) rootStyles[k] = computed.getPropertyValue(k).trim();

  return { cssVars: vars, rootStyles };
})()
```

---

## Phase 0-B — Analyze the design language

From the screenshot and extracted styles, identify and summarize:

| Category | What to extract |
|----------|----------------|
| **Color palette** | Primary bg, secondary bg, accent, text colors — translate `display-p3` values to closest hex |
| **Typography** | Font families (heading + body), weights used, line-height feel |
| **Layout feel** | Spacing density (tight / airy), max-width, section rhythm |
| **Aesthetic vibe** | 1–2 sentences describing the overall feel (e.g. "dark, technical, high-contrast with a warm orange accent") |
| **Notable patterns** | Cards, gradients, borders, shadows, animations worth noting |

Present this as a short design brief, not a raw CSS dump.

---

## Phase 0-C — Research the subject (landing pages only)

If the project is a personal brand site, run web searches **in parallel** with the capture above:

1. `WebSearch` → person's name + profession
2. `WebFetch` → every promising profile URL (LinkedIn, clinic/agency site, directories)
3. Collect: workplace, address, phone, email, education, experience, reviews, photos

---

## Phase 0-D — Present findings

Show the user:

1. **Screenshot** — full-page capture of the reference site
2. **Design brief** — plain-English summary of colors, fonts, vibe (not raw CSS)
3. **Raw CSS vars** — collapsed/summarized block if the user wants to reference them
4. **Person research** — (landing pages only) what was found vs what is still missing
5. **Prompt:** "Does this match the direction you want, or should I adjust the interpretation before building?"

Wait for the user to confirm or redirect before proceeding to Phase 1 of the build skill.

---

## Handoff to build skill

After user confirms, pass this context forward:
- The design brief (colors, fonts, vibe)
- Any departures from the default design system in CLAUDE.md (note what to override)
- The person research results

The build skill (design-landing-page or design-ecommerce) picks up from Phase 1 with this context already in hand.
