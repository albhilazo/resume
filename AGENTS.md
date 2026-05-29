# resume

This repository contains my personal resume, authored in HTML and CSS and exported to PDF.

## Key Files

| File | Purpose |
|---|---|
| `index.html` | Resume content and structure |
| `style.css` | Print-optimised styling (uses paper-css) |
| `Albert-Hilazo-Aguilera-resume.pdf` | Generated PDF output |

## PDF Generation

The PDF is generated from `index.html` using `electron-pdf`.

- **Locally:** `npm run pdf`
- **CI:** GitHub Actions automatically regenerates the PDF on every push that does not already include a PDF change

## Development

- `npm run dev` — starts a live-reload server for local editing
- Edit `index.html` directly for content changes
- Edit `style.css` for layout and print formatting
