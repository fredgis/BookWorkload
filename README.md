# Building Microsoft Fabric Workloads with the Extensibility Toolkit

A single book chapter on extending Microsoft Fabric with the Extensibility Toolkit, from the first local prototype to a workload published in a tenant.

## Files

- `workload-chapter.html` — the chapter as a styled, self-contained web page with two tabs (the outline and the full text). It needs no internet connection: the diagrams are embedded as inline SVG and there are no external scripts.
- `workload-chapter-full.md` — the full written chapter (~103,000 characters).
- `workload-chapter-outline.md` — the detailed outline.

## What it covers

The chapter follows one workload across four movements: understanding the model, developing it (by hand and with the toolkit's AI assistant), taking it to production, and distributing it. Two real workloads illustrate the development and production phases, GreenGrid and SkyNav, and each is explained where it appears.

The frontend code is TypeScript and React, because that is what Fabric loads in the iframe; the example services are written in Python.

Open `workload-chapter.html` in any browser to read it.