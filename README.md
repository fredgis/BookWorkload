<p align="center">
  <img src="assets/nfo-banner.svg" alt="Building Microsoft Fabric Workloads with the Extensibility Toolkit" width="760">
</p>

# Building Microsoft Fabric Workloads with the Extensibility Toolkit

A single book chapter on extending Microsoft Fabric with the Extensibility Toolkit, from the first local prototype to a workload published in a tenant.

## Files

- `workload-chapter.pdf` — a print-ready PDF with an interactive table of contents: a structured Contents page (movements as colored cards), clickable entries, and PDF bookmarks (the viewer's outline sidebar). Cover page, syntax-highlighted code, repo screenshots, and the diagrams rendered inline.
- `workload-chapter.html` — the chapter as a styled, self-contained web page with two tabs (the outline and the full text). It needs no internet connection: the diagrams are inline SVG, the code is syntax-highlighted, the screenshots are embedded, and there are no external scripts.
- `workload-chapter-full.md` — the full written chapter (~110,000 characters).
- `workload-chapter-outline.md` — the detailed outline.
- `assets/` — the screenshots used to illustrate the GreenGrid and SkyNav examples.

## What it covers

The chapter follows one workload across four movements: understanding the model, developing it (by hand and with the toolkit's AI assistant), taking it to production, and distributing it. Two real workloads illustrate the development and production phases, GreenGrid and SkyNav, each explained where it appears and shown with screenshots from its own repository.

The frontend code is TypeScript and React, because that is what Fabric loads in the iframe. The example services are written in Python.

Open `workload-chapter.html` in any browser to read it, or `workload-chapter.pdf` for the print version with bookmarks.