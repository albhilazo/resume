# resume

This repository contains my personal resume, authored in HTML and CSS and exported to PDF.

## Key Files

| File | Purpose |
|---|---|
| `index.html` | Resume content and structure |
| `resume.css` | Resume layout and styling |
| `assets/colors_and_type.css` | Design tokens (colors, type scale, spacing) — must load before `resume.css` |
| `assets/fonts.css` | Loads IBM Plex Sans + Mono from Google Fonts |
| `assets/logo-monogram.svg` | "ah." monogram used in the page-2 header band |
| `Albert-Hilazo-Aguilera-resume.pdf` | Generated PDF output |

## PDF Generation

Run `npm run pdf` to generate the PDF locally. The script starts a temporary HTTP server (required because the page loads Google Fonts and Lucide icons from CDN), renders the page with headless Chrome via Puppeteer, and writes `Albert-Hilazo-Aguilera-resume.pdf`.

The GitHub Actions workflow (`generate-pdf.yml`) runs automatically on any push that changes `index.html`, `resume.css`, or `assets/`. It regenerates the PDF and commits it back to the same branch, so the PDF is always up to date in PRs and on `main`.

## Development

- `npm run dev` — starts a browser-sync live-reload server (`localhost:3000`); CSS changes inject without a full page reload
- `npm run pdf` — generates the PDF locally
- Edit `index.html` directly for content changes
- Edit `resume.css` for layout and print formatting
- Edit `assets/colors_and_type.css` for design tokens (colors, spacing, type scale)

## Content Conventions

- The resume targets a **two-page A4 layout**: page 1 contains the masthead and the most recent professional experience (BioCatch), page 2 contains past experience (Thoughtworks, CAPSiDE, La Salle, T-Systems) plus sidebar education and languages.

---

## Experience section — building blocks & variants

All experience entries are built from a small vocabulary. A **company group**
(`.xp-company`) holds a head (`.xp-co-head`) and a roles rail (`.xp-roles`); the rail
contains one or more **role items** (`.xp-role-item`). Pick the variant that matches
how much detail the entry warrants.

**Which classes actually do something:**
- `.xp-company.is-tight` and `.xp-role-item.is-condensed` **carry CSS** (tighter
  spacing). Use them deliberately.
- `.xp-company.is-multi` and `.xp-role-item.is-extra` are **semantic markers with no
  CSS** — they document intent and reserve hooks for future styling. The visual
  difference between variants comes from *which child elements you include*, not from
  these classes.

### A. Full entry — single or multiple roles
Default. Company name + meta line, then role(s) with outcome bullets and a tech/skills
line. Add `is-multi` to the company when it has 2+ roles (BioCatch, Thoughtworks).

```html
<div class="xp-company is-multi">
  <div class="xp-co-head">
    <div class="xp-co-name">Company</div>
    <div class="xp-co-meta">Sector · type · location</div>
  </div>
  <div class="xp-roles">
    <div class="xp-role-item">
      <div class="xp-role-head">
        <span class="xp-role-title">Role title</span>
        <span class="xp-when">Mon YYYY — Mon YYYY</span>
      </div>
      <div class="xp-role-sub">Client / project</div>   <!-- optional italic line -->
      <ul class="xp-bullets"><li>Outcome bullet.</li></ul>
      <div class="xp-stack">Tech, Tech, Tech</div>       <!-- mono stack / skills -->
    </div>
    <!-- repeat .xp-role-item for more positions -->
  </div>
</div>
```

### B. Condensed role — `.xp-role-item.is-condensed`
Role with **no bullets**: keep the head, optional `.xp-role-sub`, and the `.xp-stack`
line. Used for older roles where the stack is enough (older Thoughtworks projects).
Just add `is-condensed` and omit the `<ul class="xp-bullets">`.

### Page-budget bullets — `<li hidden>`
When a bullet from the source notes exists but is intentionally excluded from the
rendered resume due to page space, keep it in the markup as `<li hidden>…</li>` inside
the existing `<ul class="xp-bullets">`. This preserves the content in the source
without affecting the visual output, and avoids silent divergence from the notes.

### C. Tight company — `.xp-company.is-tight` + `.xp-role-item.is-extra`
**Title + dates only.** No `.xp-co-meta` line; fold the location into the name with
`.xp-co-loc`. No bullets, no stack. Used for the oldest entries (CAPSiDE, La Salle,
T-Systems). Multiple `is-extra` items are allowed (e.g. La Salle's two roles).

```html
<div class="xp-company is-tight">
  <div class="xp-co-head">
    <div class="xp-co-name">Company<span class="xp-co-loc">Location</span></div>
  </div>
  <div class="xp-roles">
    <div class="xp-role-item is-extra">
      <div class="xp-role-head">
        <span class="xp-role-title">Role</span>
        <span class="xp-when">YYYY — YYYY</span>
      </div>
    </div>
  </div>
</div>
```

### D. Company continued across a page break
When one company spans both pages, repeat the `.xp-company` on the next page and mark
the name: `<div class="xp-co-name">Thoughtworks <span class="xp-cont">· continued</span></div>`.

**Consistency rule:** keep spacing uniform. If space is tight, prefer demoting a whole
older entry to a tighter variant (full → condensed → tight) over shrinking gaps on a
single entry — inconsistent per-entry spacing is what we explicitly avoided.

---

## Education entries

Every `.edu-item` uses the **same four-part shape**, in this order:

```html
<div class="edu-item">
  <div class="edu-type">Certification</div>               <!-- kicker: Certification / Course / Degree -->
  <div class="d">AWS Solutions Architect</div>            <!-- plain credential name — no code, no level -->
  <div class="q">Associate · <span class="code">SAA-C03</span></div>  <!-- level and/or code; codes in .code -->
  <div class="s">2023 · Amazon Web Services</div>         <!-- year FIRST, then issuer -->
</div>
```

Rules:
- **Title (`.d`)** is just the credential name. Never put exam codes or the level
  qualifier in the title.
- **Detail (`.q`)** carries the level (`Associate`, `Master's degree`) and/or the code.
  Wrap codes in `<span class="code">` (renders mono). Omit `.q` only if there's truly
  nothing; a code-only `.q` is fine.
- **Sub-line (`.s`)** is always **`year · issuer`** in that order, for consistent
  left-alignment of the leading token down the column.

---

## Page & layout structure

- Two `.page` elements, each a fixed **A4 sheet (794 × 1123px @96dpi)**, wrapped in
  `.page-wrap`. Print maps one `.page` → one physical A4 page.
- **Page 1** leads with the full masthead `.cv-head` (name, role kicker, summary),
  spanning both columns. **Every later page** uses the slim `.cv-band` instead
  (monogram + name + "Résumé · Page N of M").
- The page grid is `grid-template-columns: 256px 1fr; grid-template-rows: auto 1fr;`
  — the `1fr` row makes `.cv-rail` stretch to the page bottom. **Don't remove it** or
  the tinted rail stops short on short pages.
- **Keep each page's content within 1123px tall.** After any content change, check
  for overflow. To fit: demote older entries to tighter variants, trim bullets, or
  add a page. When the page count changes, update **every** `.page-label`
  ("Page X of N") and each `.cv-band-meta` ("Résumé · Page N of M").

---

## Copy & content rules

- **No em-dashes** anywhere **except** numeric date ranges in `.xp-when`
  (e.g. `Nov 2023 — Present`). In prose, bullets, titles, and education, use commas
  or rewrite — em-dash asides read as AI-generated.
- **Sentence case** for all readable text. Preserve tech/proper-noun casing exactly:
  AWS, TypeScript, Node.js, React.js, GraphQL, Datadog, Kubernetes. UPPERCASE is only
  for the mono kickers/labels (the CSS handles the casing/tracking — write them in
  normal case in the markup where a class applies it, or match existing entries).
- **No emoji.** Emphasis comes from weight, color, and the teal accent — never emoji.
- **Bullets:** one idea per line; lead with a past-tense action verb; outcome before
  mechanism; concrete, modest numbers. First person is implied — drop the subject
  ("Led a team…", not "I led…"). No hype words (synergy, rockstar, cutting-edge).
- **Tech/skills line** (`.xp-stack`): comma-separated, renders mono. Reused for both
  tech stacks and skill lists.

---

## Styling rules

- **Use only `var(--*)` tokens** from `assets/colors_and_type.css` — never hardcode a
  color. Teal is the only hue; bright teal is decorative, deep petrol teals carry text
  contrast.
- **Inline `.code` spans must keep the `padding: 0; background: none; border-radius: 0`
  override** in `resume.css`. The design system's base `code, .code` rule adds a teal
  pill with horizontal padding; that override suppresses it so codes align flush as
  plain mono text. If you add codes elsewhere in the résumé, carry the same override.
- The `@media print` block re-encodes teal accents for grayscale safety (weight/ink
  instead of hue). Preserve it when editing print styles.
