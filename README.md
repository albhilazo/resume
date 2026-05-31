# Resume

My resume composed using plain HTML and CSS, then exported to PDF.

#### [Download PDF](https://github.com/albhilazo/resume/raw/main/Albert-Hilazo-Aguilera-resume.pdf)

#### [LinkedIn Profile](https://linkedin.com/in/albhilazo)

## Key Files

| File | Purpose |
|---|---|
| `index.html` | Resume content and structure |
| `resume.css` | Resume layout and styling |
| `assets/colors_and_type.css` | Design tokens (colors, type scale, spacing) |
| `assets/fonts.css` | Loads IBM Plex Sans + Mono from Google Fonts |
| `assets/logo-monogram.svg` | "ah." monogram used in the page-2 header band |
| `Albert-Hilazo-Aguilera-resume.pdf` | Generated PDF output |

## Dependencies

- **Google Fonts** — IBM Plex Sans + IBM Plex Mono, loaded by `assets/fonts.css`.
- **Lucide icons** — loaded from `https://unpkg.com/lucide@latest` for contact icons.

## PDF Generation

Open `index.html` in a browser and print to PDF (A4, no margins). The `@media print` block in `resume.css` handles page breaks and grayscale-safe adjustments automatically.

## Development

- `npm run dev` — starts a live-reload server for local editing
- Edit `index.html` directly for content changes
- Edit `resume.css` for layout and print formatting; edit `assets/colors_and_type.css` for design tokens
