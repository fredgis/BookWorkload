# Building Production-Ready Workloads for Microsoft Fabric

## V1.0.0 rework log

**Editorial review date:** September 17, 2026  
**Technical reference date:** September 17, 2026  
**Audit source:** supplied file `bilan-rework-kb-fabric-workloads.md`  
**Excluded scope:** Section 9 and its illustrations

This log maps the audit items T01 through T15 to the V1.0.0 manuscript. It separates documentary verification from commands and tenant flows that were executed.

## Audit item status

| ID | Status | Main changes | Evidence in the manuscript |
|---|---|---|---|
| T01 | Complete | Added primary audience, assumed knowledge, learning outcomes, reading paths, and role-based next actions | Opening reader contract, section 18 |
| T02 | Complete | Corrected `acquireFrontendAccessToken` result use, separated v1 and v2 audience rules, and stated that token validation does not replace resource authorization | Sections 0, 4, 6.2, Appendix G |
| T03 | Complete | Separated frontend hosting, frontend-to-publisher calls, Fabric-to-remote calls, resource access, item-type manifests, and item-instance definitions | Sections 2.1, 3.1, 4, Appendix A |
| T04 | Complete | Grouped prerequisites by scenario and added actor, permission, starting directory, action, adapted parameters, observable result, and source to the main procedures | Sections 0, 5, 6.1, Appendix B |
| T05 | Complete | Consolidated workload capabilities, publisher work, conditions, sources, and implementation references in one matrix | Section 2.2, Appendix A |
| T06 | Complete with qualified uncertainties | Added a pinned reference baseline, local sources for architecture-changing rules, and a provenance register. First-party contradictions remain labeled and bounded | Opening reference record, sections 1.3 and 16.2, Appendix F |
| T07 | Complete | Connected security boundaries to controls, owners, and expected evidence | Sections 11 and 12, Appendix E |
| T08 | Complete | Separated configuration, design, and operational proof. Defined the release compatibility set, one version source, validation stages, and forward recovery | Sections 10, 13, 17, Appendix E |
| T09 | Complete | Replaced symptom shortcuts with plausible causes, a discriminating test, expected observation, and next action | Sections 0 and 8, Appendices E and G |
| T10 | Complete | Reframed AI assistance as a reusable Frame, Ground, Bound, Review, Verify workflow and separated repository guidance from executable platform capabilities | Section 7, Appendix C |
| T11 | Complete | Added observable product-fit criteria, alternatives, durable costs, and distribution-path decisions without sales claims | Sections 1.1, 1.3, 16.3 |
| T12 | Complete | Assigned primary homes to recurring rules, shortened contextual reminders, and made the patterns section decision-based | Sections 2, 4, 6, 12, 14 |
| T13 | Complete | Rebuilt Appendices A through G as package, capability, command, AI, snippet, release, diagnostic, glossary, source, and quick-reference material | Appendices A through G |
| T14 | Complete | Checked heading hierarchy, anchors, internal references, code fences, Mermaid blocks, artifact links, and maintenance rules | Table of contents, Appendix F, validation record below |
| T15 | Complete | Added a stage-by-stage distribution matrix and separated publish, activate, consent, and assign | Sections 15 and 16 |

## Source and uncertainty decisions

| Topic | Decision used in V1.0.0 |
|---|---|
| Toolkit baseline | Pin the official toolkit implementation commit, record the separate release-tag commit, and state the SDK dependency range and missing lockfile instead of implying an exact resolved version |
| Setup scripts | Record exact signatures and working directories, but classify the pinned sequence as source-inspected because `SetupWorkload.ps1` passes an unsupported `-Force` to the package build |
| Publishing validator | Pin `v2025.12.1` at commit `78e17c3...`, use the effective Node.js 20 minimum from its lockfile, and classify the CLI as source-inspected rather than executed |
| Remote hosting | Describe the current remote contracts, but retain a target-stage verification warning while first-party publishing pages disagree |
| Trial requirement | Cite both first-party publishing pages and require confirmation from the active validator or publishing team |
| Package upload | Treat upload as an Admin Portal action because the public Admin Workloads API does not document an upload operation |
| Tenant and publication behavior | Mark documentation-backed behavior as current reference. Do not call it tenant-tested |

## Validation record

| Layer | Status | What was checked |
|---|---|---|
| Audit traceability | Complete | Each T01 through T15 requirement mapped to manuscript sections and appendices |
| Primary-source review | Complete | Microsoft Learn, Entra documentation, Partner Center documentation, and the pinned official toolkit repository |
| Excluded-scope preservation | Complete | Section 9 matches commit `6f02e1c` byte for byte within its section boundaries, SHA-256 `55585676885f32529552b1fae389e0b971f8656b153f3dd93df08ea684e5723a` |
| Markdown structure | Complete | Heading hierarchy, table of contents anchors, internal references, code-fence balance, local links, numbered-section alignment, and appendix alignment |
| Code and data parsing | Complete | 15 Python blocks, JSON, XML, YAML, 10 TypeScript/TSX blocks, seven PowerShell blocks, and the Bicep block passed available parsers or compilers |
| Bash syntax | Not run | Three Bash blocks remain simple environment/setup commands. No Bash executable was available on this Windows host |
| Mermaid | Complete | All 16 outline and manuscript diagrams rendered with Mermaid CLI 11.16.0 |
| External citations | Complete | 104 cited HTTP/HTTPS links returned a non-error response during the final check |
| HTML | Complete | Named HTML and `index.html` are byte-identical, contain unique IDs and 16 inline SVG diagrams, and have no external script dependency |
| HTML behavior | Complete | Outline/Manuscript click and keyboard navigation passed. Mobile emulation at 390 by 844 and desktop width checks found no page-level horizontal overflow or runtime exception |
| Color contrast | Complete | Web chrome, movement headers, code text, muted text, and cover text met WCAG AA normal-text contrast for the tested color pairs |
| PDF | Complete | 53 pages, 77 bookmarks, 243 clickable links, the previous edition's designed cover and spacious contents treatment, rasterized Mermaid diagrams, correct title metadata, and no blank spill page |
| Fabric tenant execution | Not run | No tenant was modified and the first-run, assignment, consent, or remote endpoint flows were not executed for this editorial release |
| Fabric publishing validator | Not run | The manuscript documents its inputs and outputs but does not claim a validator result |
| Marketplace lifecycle | Not run | No offer, webhook, fulfillment, or billing flow was created or exercised |

## Visual system

**Design read:** technical field guide for ISV, developer, architecture, security, and publishing readers, using the established Fabric field-guide visual language at ENERGY 1 / RHYTHM 2 / MOTION 1.

| Decision | Reason |
|---|---|
| Blue, green, orange, purple, and teal movement colors | The palette identifies the four book movements and reference material. It is a content-navigation system, not decoration |
| Segoe UI body with Consolas code | The pair matches the Microsoft technical context and keeps prose and code clearly distinct |
| Compact navy hero | The title remains recognizable without pushing repository content below an oversized banner |
| Banner gradient and circular motif | The existing navy depth and restrained data-ring motif identify the Fabric/OneLake context and remain confined to the hero |
| Two-tab HTML layout | Outline and manuscript share one self-contained artifact while keeping one primary reading surface visible |
| Restrained radius and shadow | Cards separate the document from the page background without making every element float |
| Movement-colored heading rule | The left rule identifies heading level and the current movement, so the color carries navigation meaning |
| Responsive single-column fallback | Tables and movement groups remain readable on a narrow screen |
| Static interaction | Only the document tabs move. The field guide does not need decorative animation |
| Fixed light theme | The web artifact mirrors the print edition and prioritizes long-form technical reading |

The GreenGrid screenshots remain because they show the workload inside Fabric and belong to the audit's excluded section. No generic illustration was added.

## Residual maintenance work

Recheck the pinned baseline whenever the toolkit commit, SDK package, manifest schema, setup scripts, package limits, remote hosting rules, publishing validator, selected-tenant rules, Marketplace requirements, or Admin Workloads APIs change. Record the technical verification date separately from ordinary editorial edits.
