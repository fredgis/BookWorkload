# Building Production-Ready Workloads for Microsoft Fabric

*From the ISV business case and architecture to secure delivery, operations, and distribution*

> Keep only a short editorial date near the opening. Put the implementation pins, validator version, source record, discrepancies, and validation scope in Appendix F.

## Contents

**[Choose your path](#choose-your-path)**

**[0. Before you begin](#0-before-you-begin)**

**Understand**

- **[1. What a workload is, and why an ISV would build one](#1-what-a-workload-is-and-why-an-isv-would-build-one)**
- **[2. How a workload runs: architecture, the host, and one request](#2-how-a-workload-runs-architecture-the-host-and-one-request)**
- **[3. The manifest package: the contract with Fabric](#3-the-manifest-package-the-contract-with-fabric)**
- **[4. Identity and access with Microsoft Entra](#4-identity-and-access-with-microsoft-entra)**

**Develop**

- **[5. The toolkit and the development environment](#5-the-toolkit-and-the-development-environment)**
- **[6. Building an item: editor, data, and capabilities](#6-building-an-item-editor-data-and-capabilities)**
- **[7. Developing with AI assistance](#7-developing-with-ai-assistance)**
- **[8. Diagnostics and debugging](#8-diagnostics-and-debugging)**
- **[9. Illustration: GreenGrid](#9-illustration-greengrid)**

**Go to production**

- **[10. From developer mode to production](#10-from-developer-mode-to-production)**
- **[11. Hosting, domain, and identity](#11-hosting-domain-and-identity)**
- **[12. Security and compliance](#12-security-and-compliance)**
- **[13. Packaging, validation, and CI/CD](#13-packaging-validation-and-cicd)**
- **[14. Patterns and anti-patterns](#14-patterns-and-anti-patterns)**

**Distribute**

- **[15. Make it available in your tenant](#15-make-it-available-in-your-tenant)**
- **[16. Publish across tenants: Workload Hub and Microsoft Marketplace](#16-publish-across-tenants-workload-hub-and-microsoft-marketplace)**
- **[17. The post-publish lifecycle](#17-the-post-publish-lifecycle)**
- **[18. Recap and next steps](#18-recap-and-next-steps)**

**Appendices**


---


> A self-contained manuscript for the Workload part of the book (target: about 190 to 200 pages when expanded).
> Topic: extending Microsoft Fabric with the **Extensibility Toolkit**.
> Audience: ISVs, software development companies, developers, and architects building their first Fabric workload.

---

## How the chapter is organized

The manuscript starts with the ISV business case, then teaches the workload model in four movements. GreenGrid appears at the end of Develop as a worked illustration, not a reference architecture.

- **Understand**, what a workload is and how it runs inside Fabric.
- **Develop**, how you build a workload by hand and with the repository's AI guidance. Then an illustration with **GreenGrid**.
- **Go to production**, shipping a workload, its security and compliance, packaging and CI/CD.
- **Distribute**, make the workload available in a tenant, publish it to the marketplace with its review and monetization, and run it through its post-publish life.

```mermaid
flowchart LR
    U["UNDERSTAND<br/>business and model"]
    D["DEVELOP<br/>by hand and with AI"]
    PR["GO TO PRODUCTION<br/>ship, secure, package"]
    DI["DISTRIBUTE<br/>tenant, Workload Hub,<br/>Marketplace lifecycle"]
    U ==> D ==> PR ==> DI
    GG["Illustration: GreenGrid"]
    GG -.-> D
    style U fill:#1565c0,stroke:#0d47a1,color:#ffffff
    style D fill:#2ea44f,stroke:#1b7a37,color:#ffffff
    style PR fill:#e36209,stroke:#b14e00,color:#ffffff
    style DI fill:#8957e5,stroke:#6e40c9,color:#ffffff
    style GG fill:#e8f6ec,stroke:#2ea44f,color:#1b7a37
```

### The worked illustration

| Illustration | Appears in | Why it is there |
|---|---|---|
| **GreenGrid** | Develop | A workload built locally with the Dev Gateway, explained from scratch |

### Indicative page budget

| Sections | Movement | Pages |
|---|---|---|
| 0 | Before you begin | ~5 |
| 1-4 | Understand | ~35 |
| 5-9 | Develop (by hand, with AI, diagnostics, GreenGrid) | ~80 |
| 10-14 | Go to production | ~45 |
| 15-18 | Distribute | ~30 |
| Appendices A-G | Reference | ~10 |
| **Total** | | **~205** |

---

## Choose your path

- State the primary audience: ISV/SDC product and engineering teams, with developer, security, operations, administration, and publishing readers.
- State assumed knowledge: web applications, REST/JSON, OAuth concepts, source control, and basic Fabric workspaces/items. TypeScript/React and Python are scenario-specific.
- State learning outcomes: decide whether a workload fits, explain boundaries, build and diagnose an item, define a release unit, and choose a distribution path.

| Path | Sections | Outcome |
|---|---|---|
| **Fast track** | 0 → 2 → 5 → 6 → 9 | Follow the first-workload path and connect the GreenGrid example |
| **Production track** | 10 through 17 | Turn the prototype into a hosted, secured, packaged, assigned, and supportable product |
| **Product and business** | 1 → 16 → 18 | Define the customer, commercial path, pilot, and value signals |

- Use one general snippet convention. Keep local warnings only where a simplification affects security, reliability, or deployment.

# 0. Before you begin

- 0.1 Environment, accounts, and assumed concepts. Entra app creation rights and an F, P, or Trial capacity are required. An Azure subscription is optional until Azure hosting is used. Node.js, PowerShell, .NET, Azure CLI, and the toolkit scripts. Administrator, capacity, workspace, publisher, and user roles kept separate.
- 0.2 A short first-run path in action order: clone, setup, start Dev Server, start Dev Gateway, enable Developer Mode, open Hello World. Keep one local warning and move script details to Appendix B.
- 0.3 Common pitfalls, the environment traps that catch people before any code, capacity, developer mode, env files, token audience, framing, and package version.

---

# Understand

> What a workload is, how Fabric runs it, and the contract that makes it native, anchored by one concrete request.

## 1. What a workload is, and why an ISV would build one

- Introduce GreenGrid immediately as the running example: an energy-scoring SaaS that works with approved OneLake data while its scoring IP stays in the publisher cloud.
- 1.1 The ISV business case: bring your service to Fabric data. Distinguish an ISV extending a product from an SDC productizing repeated project work. Cover targeted account access, faster first value, lower integration effort, recurring use, pilot conversion, support cost, Marketplace, and conditional co-sell.
- 1.2 What Fabric gives you, and what a workload adds. Lead with the user benefit of one item and workspace experience. Refer capability details to Appendix A.
- 1.3 The toolkit, when to use it, and its limits. Add an observable decision matrix comparing a Fabric workload, Power BI custom visual, Activator, pipeline/notebook/function, and separate SaaS experience. Include persistent item, dedicated editor, lifecycle, distribution, required skills, SDK compatibility, publishing review, consent, support, and operations.

```mermaid
flowchart LR
    USER["Fabric user"]
    subgraph CUSTOMER["Customer's Fabric tenant"]
        ITEM["ISV workload item"]
        OL[("OneLake and Fabric data")]
        ITEM -->|"resource-scoped token"| OL
    end
    subgraph PUBLISHER["ISV / SDC cloud"]
        SERVICE["Existing SaaS or PaaS"]
        IP["Domain logic and IP"]
        SERVICE --> IP
    end
    USER --> ITEM
    ITEM -->|"approved API and data flow"| SERVICE
    style USER fill:#8957e5,stroke:#6f42c1,color:#ffffff
    style CUSTOMER fill:#e3f0fb,stroke:#1565c0,color:#0d47a1
    style ITEM fill:#1565c0,stroke:#0d47a1,color:#ffffff
    style OL fill:#0d9488,stroke:#0a6b62,color:#ffffff
    style PUBLISHER fill:#e8f6ec,stroke:#2ea44f,color:#1b7a37
    style SERVICE fill:#2ea44f,stroke:#1b7a37,color:#ffffff
    style IP fill:#e36209,stroke:#b14e00,color:#ffffff
```

## 2. How a workload runs: architecture, the host, and one request

- 2.1 The Fabric host, your frontend, and an optional backend. Keep the architecture overview here. Move remote header and token subtleties to section 4 and Appendix F.

```mermaid
flowchart TB
    subgraph BROWSER["User browser"]
        direction LR
        HOST["Fabric portal"]
        IFRAME["Workload iframe<br/>publisher frontend runs here"]
        HOST <-->|"host API"| IFRAME
    end
    subgraph FAB["Microsoft Fabric"]
        direction LR
        APIS["Fabric public REST APIs"]
        OL[("OneLake")]
    end
    subgraph CLOUD["Publisher cloud"]
        direction TB
        FE["Frontend host<br/>serves SPA assets"]
        BE["Remote backend<br/>(optional)"]
    end
    ENTRA["Microsoft Entra ID"]
    IFRAME -->|"loads assets"| FE
    IFRAME -->|"request resource token"| HOST
    HOST -->|"delegated token flow"| ENTRA
    IFRAME -->|"Fabric token"| APIS
    IFRAME -->|"Storage token"| OL
    IFRAME -->|"frontend API call"| BE
    APIS -->|"jobs / lifecycle<br/>SubjectAndAppToken1.0"| BE
    BE -->|"validate, OBO, or app token"| ENTRA
    style BROWSER fill:#f4f7fb,stroke:#6e5494,color:#553c7b
    style FAB fill:#e3f0fb,stroke:#1565c0,color:#0d47a1
    style CLOUD fill:#e8f6ec,stroke:#2ea44f,color:#1b7a37
    style HOST fill:#1565c0,stroke:#0d47a1,color:#ffffff
    style IFRAME fill:#1565c0,stroke:#0d47a1,color:#ffffff
    style APIS fill:#1565c0,stroke:#0d47a1,color:#ffffff
    style OL fill:#0d9488,stroke:#0a6b62,color:#ffffff
    style FE fill:#2ea44f,stroke:#1b7a37,color:#ffffff
    style BE fill:#e36209,stroke:#b14e00,color:#ffffff
    style ENTRA fill:#8957e5,stroke:#6e40c9,color:#ffffff
```

- Add a relationship table separating frontend → Fabric/OneLake, frontend → publisher API, Fabric → remote endpoint (`SubjectAndAppToken1.0`), and publisher backend → target resource.
- 2.2 Items as native artifacts, with explicit integrations. Control-plane item definitions are separate from OneLake data. Catalog, monitoring, Git, deployment, jobs, and lifecycle behavior each need configuration and validation.
- 2.3 A request, step by step, from opening an item to acquiring a resource-scoped delegated token and calling the matching Fabric, OneLake, or publisher endpoint.

```mermaid
sequenceDiagram
    participant U as User
    participant H as Fabric host
    participant W as Workload (iframe)
    participant E as Microsoft Entra
    participant A as Fabric APIs / OneLake
    U->>H: Open the item
    H->>W: Load iframe, bootstrap from manifest
    W->>H: acquireFrontendAccessToken(scopes)
    H->>E: Request delegated resource token
    E-->>H: Resource-scoped token
    H-->>W: Return token to iframe
    W->>A: Call with the bearer token
    A-->>W: Data
    W-->>U: Render the screen
```

## 3. The manifest package: the contract with Fabric

- 3.1 Workload, product, and paired item manifests. Separate source tree, built package, hosted application, and item-instance definitions. Keep detailed limits in Appendix A.
- 3.2 Identity and naming, `Org.[Name]` for one tenant versus `[Publisher].[Workload]` for cross-tenant publication. Permanent reservation on first confirmation, assignment scopes, public publishing requirements, and the separate Microsoft Marketplace SaaS offer.

| Aspect | `Org.[Name]` (internal) | `[Publisher].[Workload]` (cross-tenant) |
|---|---|---|
| Audience | Your own tenant | Other tenants |
| Registration | No separate workload-name registration | Name and publishing tenant reserved permanently |
| Availability | Upload, activate, and assign | Selected tenants, Preview, then GA |
| Requirements | General hosting, identity, and manifest requirements | General, workload, item, attestation, support, and review requirements |
| Commerce | Not required | Microsoft Marketplace SaaS offer required for Preview and GA |
| Name length | No separate limit established by the cited publishing overview | Workload portion at most 32 characters |

## 4. Identity and access with Microsoft Entra

- 4.1 Delegated frontend tokens and backend OBO. Use the documented token result property, one acquisition API but separate resource audiences and tokens, and OBO only for backend exchange.
- 4.2 Entra applications, scopes, and service identities. Distinguish v2 client-ID audiences from v1 audience rules, pair issuer/version/audience, and state that token and scope validation do not replace tenant/workspace/item authorization. Keep `Fabric.Extend`, hosting-mode-specific redirects, backend credentials, and managed identity roles separate.

---

# Develop

> How a workload is built, first by hand, then with the repository's AI guidance, followed by diagnosis and the GreenGrid illustration.

## 5. The toolkit and the development environment

- 5.1 The starter kit, setup script, and Entra applications. Follow the action order and refer exact parameters and pinned defects to Appendix B.
- 5.2 Dev Server, Dev Gateway, and the Hello World checkpoint. Two terminals, tenant settings, Developer Mode, Local Network Access, and the observable Hello World result.

```mermaid
flowchart LR
    DEV["Your machine"]
    subgraph LOCAL["Localhost"]
        direction TB
        T1["Dev Server - workload UI and APIs"]
        T2["Dev Gateway - the bridge"]
    end
    PORTAL["Fabric portal"]
    DEV --> T1
    DEV --> T2
    T2 ==>|"registers the dev instance"| PORTAL
    PORTAL ==>|"renders the iFrame from"| T1
    style DEV fill:#6e5494,stroke:#553c7b,color:#ffffff
    style LOCAL fill:#e8f6ec,stroke:#2ea44f,color:#1b7a37
    style T1 fill:#2ea44f,stroke:#1b7a37,color:#ffffff
    style T2 fill:#0d9488,stroke:#0a6b62,color:#ffffff
    style PORTAL fill:#1565c0,stroke:#0d47a1,color:#ffffff
```

## 6. Building an item: editor, data, and capabilities

- 6.1 The item, its editor, and how it surfaces. Create the item, complete `ITEM_NAMES`, `Product.json`, locale, and route wiring, then show the compact definition and editor result. Exact command details stay in Appendix B.
- 6.2 Separating item definitions from OneLake data. Resource-specific Storage tokens, backend OBO for OneLake when needed, and no customer data, secrets, or large results in the item definition.
- 6.3 Remote jobs, lifecycle events, and other capabilities. Explain the observable capability result and keep script parameters, version quirks, and source discrepancies in Appendices A, B, and F.

## 7. Developing with AI assistance

> The toolkit repository includes AI guidance and a Copilot agent. These files are versioned project documentation and can lag behind Microsoft Learn. The optional UX MCP referenced by the toolkit is community-hosted, not a Microsoft Fabric service.

- Add the visual rule **AI output is a proposal, not platform truth**.
- Structure the method as Frame → Ground → Bound → Review → Verify, with an expected result for each step, followed by the four contract gates: SDK → manifests → build → runtime.

- 7.1 Repository guidance: useful, mutable, and versioned. Keep the distinction between written guidance and executable commands, with the detailed file map in Appendix C.
- 7.2 The Copilot agent and the optional community UX MCP. Retain one bounded prompt. Move MCP configuration and dependency review to Appendix C.
- 7.3 What it generates, and keeping it honest. Keep a short acceptance rule across SDK, manifests, build, and runtime.

## 8. Diagnostics and debugging

- 8.1 Reading the chain, and the token and manifest boundaries. Use observations and one discriminating test at a time instead of mapping one symptom to one cause. Keep resource scopes/audiences, tenant/issuer policy, redirects, local checks, and publishing validation distinct.
- 8.2 The Dev Gateway, the iframe boundary, and correlation. Browser Local Network Access, local-version precedence, and explicit propagation of `ActivityId`/`RequestId` only where those identifiers are present.

## 9. Illustration: GreenGrid

> A teaching sample, not a reference architecture. It isolates the development concepts and deliberately skips production concerns.

- 9.1 What GreenGrid is, a fictional ISV/SDC whose Entra-protected SaaS holds the scoring algorithm. The workload reads approved OneLake fields and sends a documented payload to the publisher boundary.

```mermaid
flowchart LR
    OL[("OneLake<br/>customer site data")]
    SA["GreenGrid SaaS<br/>Entra-protected scoring API"]
    W["GreenGrid workload<br/>the item you build"]
    R["Scorecard<br/>inside Fabric"]
    OL --> W
    W -->|"approved site fields"| SA
    SA -->|"scores"| W
    W --> R
    style OL fill:#0d9488,stroke:#0a6b62,color:#ffffff
    style SA fill:#8957e5,stroke:#6e40c9,color:#ffffff
    style W fill:#2ea44f,stroke:#1b7a37,color:#ffffff
    style R fill:#1565c0,stroke:#0d47a1,color:#ffffff
```

- 9.2 The scoring service, in Python. Protect it with a delegated GreenGrid API scope, then add unit tests and container packaging.
- 9.3 Milestone 1: the workload calls the service. Seed input and an authenticated SaaS call with no browser API key.
- 9.4 Milestones 2 and 3: real data and a native screen. A separate OneLake Storage token, real data behind the same function, then a graphical Fabric-themed view.

---

# Go to production

> What changes from developer mode, how you secure it, how you package and automate it, and the patterns that keep it sound.

## 10. From developer mode to production

- 10.1 What changes. Keep the dev/production comparison, then separate configuration substitutions, production design decisions, and operational proof with observable exit evidence.

| Concern | Developer mode | Production |
|---|---|---|
| Delivery to Fabric | Dev Gateway bridges localhost | Fabric loads your hosted URL directly |
| Frontend host | localhost | Your cloud, HTTPS, on a verified domain |
| Backend identity | Development app and local credentials | Backend app credential for remote flows, managed identity where supported |
| Telemetry | Console and local logs | Application Insights, correlation IDs retained |
| Reaching users | Your dev workspace | Upload, activate, assign, or publish across tenants |

- 10.2 What to verify across the transition: verified domain, HTTPS, CSP for Fabric and Power BI portal domains, CORS, resource-specific token audiences, new package version, and token lifetime.

## 11. Hosting, domain, and identity

- 11.1 Hosting, the verified domain, and the resource ID, a frontend and optional backend on your cloud. The frontend as a subdomain of a verified Entra domain, the resource ID format.
- 11.2 User delegation, backend credentials, and managed identities. Current remote OBO/S2S reference flow uses a rotated server-side backend credential. Managed identity remains appropriate for supported service calls.

## 12. Security and compliance

- 12.1 Data boundaries, labels, and publisher responsibility. Add a table mapping tenant isolation, resource authorization, publisher data transfers, logs, secrets, consent changes, and incident escalation to the control, responsible party, and release evidence. Preserve the warning that sensitivity labels/protection are not automatically applied.
- 12.2 Secrets, telemetry, monitoring, and support. Backend credentials in Key Vault, managed identity where supported, no secrets in frontend or URLs, `ActivityId`/`RequestId`, publisher SLA/help/livesite contacts, and the boundary between Microsoft platform support and publisher support.

## 13. Packaging, validation, and CI/CD

- 13.1 Package build, schema checks, and publishing validation. Keep the compatibility set, version source, package verification, and distinction between local checks and publishing review. Move validator particulars to Appendix B.
- 13.2 Automating the pipeline. Infrastructure as code, Windows runner for current PowerShell scripts, publisher-owned lockfile before `npm ci`, pinned `build:prod` script name, OIDC for Azure deployment, UI-based package upload, Admin API automation only for listing/assignment, and a validation-stage table with inputs, outputs, owners, and evidence.

## 14. Patterns and anti-patterns

- 14.1 Patterns that hold up. Tie each pattern to a product decision and its operational consequence.
- 14.2 Anti-patterns to avoid. Tie each shortcut to the concrete runtime, security, publication, or migration failure it creates.

---

# Distribute

> Put a finished workload in front of users, then keep it running.

## 15. Make it available in your tenant

- 15.1 Admin Portal upload, activation, and assignment. Publish versus Manage my tenant, unique versions, deactivation before deletion, and tenant/capacity/workspace assignment.
- 15.2 Internal publishing with Org.[Name]. No separate workload-name registration, but all general hosting, identity, manifest, security, and tenant requirements still apply.

## 16. Publish across tenants: Workload Hub and Microsoft Marketplace

- 16.1 Selected tenants, Preview, and GA. Use a readable stage table with audience, publisher action, customer/admin action, and result. Keep exact evidence in Appendix F. Define publish, activate, consent, and assign separately.

```mermaid
flowchart TB
    PKG["Workload package (.nupkg)"]
    subgraph A["Internal path"]
        direction TB
        UP["Admin Portal upload"]
        ACT["Activate and assign"]
        TEN["Tenant / capacity / workspace"]
        UP --> ACT --> TEN
    end
    subgraph B["Cross-tenant path"]
        direction TB
        SELECTED["Up to 20 selected tenants"]
        PREVIEW["Preview"]
        GA["General availability"]
        SELECTED --> PREVIEW --> GA
    end
    OFFER["Microsoft Marketplace<br/>SaaS offer"]
    HUB["Fabric Workload Hub<br/>discovery and assignment"]
    PKG --> UP
    PKG --> SELECTED
    OFFER --> PREVIEW
    PREVIEW --> HUB
    GA --> HUB
    style PKG fill:#1565c0,stroke:#0d47a1,color:#ffffff
    style A fill:#fdece0,stroke:#e36209,color:#8a3b00
    style B fill:#efe7fb,stroke:#8957e5,color:#4f2c91
    style UP fill:#e36209,stroke:#b14e00,color:#ffffff
    style ACT fill:#e36209,stroke:#b14e00,color:#ffffff
    style TEN fill:#e36209,stroke:#b14e00,color:#ffffff
    style SELECTED fill:#8957e5,stroke:#6e40c9,color:#ffffff
    style PREVIEW fill:#8957e5,stroke:#6e40c9,color:#ffffff
    style GA fill:#8957e5,stroke:#6e40c9,color:#ffffff
    style OFFER fill:#0d9488,stroke:#0a6b62,color:#ffffff
    style HUB fill:#1565c0,stroke:#0d47a1,color:#ffffff
```

- 16.2 Publishing requirements, commerce, and support. Explain the customer journey from value proposition through approval, setup, entitlement, and support. Distinguish existing SaaS customers, new customers, Marketplace models, and conditional co-sell. Move webhook code to Appendix D and requirement conflicts to Appendix F.
- 16.3 Choosing a path. Keep the distribution choices, then define a small selected-tenant pilot and success signals for value, onboarding effort, recurring use, conversion, and support.

## 17. The post-publish lifecycle

- 17.1 Updates, migration, and deprecation. Tie compatibility to customer trust: older items still open and migrate safely.
- 17.2 Monitoring, consent, rollback, and feature flags. Explain permission changes before consent and recover through a known-good forward version.

## 18. Recap and next steps

- 18.1 End with a customer, problem, pilot, value signal, and commercial path, followed by concise actions for the remaining roles.

---

## Appendices

- **Appendix A: Manifest package reference** (`WorkloadManifest.xml`, `Product.json`, paired item XML/JSON, release checks, limits, naming)
- **Appendix B: Setup, development, package, and validator commands** (actor, directory, exact parameters, expected output, versioned source, and source-inspected defects)
- **Appendix C: AI guidance reference** (guidance hierarchy, file map, optional MCP configuration, and SDK → manifests → build → runtime gates)
- **Appendix D: Python service reference** (example location, dependencies, boundary, production gaps, and Marketplace webhook)
- **Appendix E: Release and compliance checklist, diagnostics quick reference**
- **Appendix F: Glossary and resources** (reference baseline, provenance register, primary sources, and maintenance rule)
- **Appendix G: Quick reference** (commands and essential decisions, with links back to the full explanations)

---

*Illustration: GreenGrid ([fredgis/FY27FabricMotion](https://github.com/fredgis/FY27FabricMotion), micro hack 2). Primary references: Microsoft Learn Extensibility Toolkit documentation, Fabric Admin Workloads API reference, Microsoft Marketplace SaaS offer documentation, and the official microsoft/fabric-extensibility-toolkit repository.*
