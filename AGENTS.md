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

## Content Conventions

- The resume targets a **two-page A4 layout**: page 1 contains recent professional experience (BioCatch and Thoughtworks), page 2 contains past experience (CAPSiDE, La Salle, T-Systems) plus sidebar skills.
- **Older positions** (CAPSiDE, La Salle, T-Systems) have no description bullets — only title, sector, location/period, and skills. This is intentional to preserve two-page fit.
- The `hidden` HTML attribute is used on elements kept in the file for reference but not shown in the rendered resume. This allows toggling content without deleting it.
