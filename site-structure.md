# Site Structure Reference

Detailed content requirements for each page in the generated docs site.
The LLM should write each page as a Markdown file following these specs.

---

## Single-Repo Pages

### index.md — Executive Summary

**The most important page.** A PM who reads only this page walks away understanding
the product purpose.

Content:
- Badge showing repo type + language
- "At a Glance" stats table (routes, deps, analytics events, last commit, contributors)
- 3–5 sentence plain-English summary
- Mermaid architecture diagram (high-level, ~5-10 nodes)
- "Key Findings" callout boxes (top highlights and top risks)
- Table of contents linking to every other page with one-line descriptions

### architecture.md — Architecture

Content:
- Mermaid `graph TD` or `graph LR` showing all major components and data flow
- Prose description of the pattern (monolith, microservices, serverless, etc.)
- For each component: what it does in product terms
- Deployment model if detectable (Docker, Vercel, AWS, etc.)
- If fullstack: separate diagrams for frontend and backend + a combined view
- Mermaid `sequenceDiagram` for a typical request flow

### routes.md — Routes & Endpoints

Content:
- Table: Route/Endpoint | Method | Purpose | Auth required? | Dynamic params? | Source
- Each row should include a source file and line number reference
- Frontend: Mermaid `graph TD` showing page hierarchy / navigation tree
- Backend: group by resource (all `/users/*` together, etc.)
- GraphQL: list queries and mutations separately
- Flag undocumented endpoints (exist in code but not in OpenAPI/README)

### inputs.md — User Inputs

Content:
- Table: Page/Route | Input type | Field names | Validation | Destination
- Mermaid `flowchart` for multi-step forms showing user journey
- Flag inputs lacking validation (UX + security risk)
- Flag inputs without error states

### analytics.md — Analytics & Tracking

Content split into two sections:

**Existing events:**
- Table: Event name | Trigger | Payload | Provider | Source File | Line
- Grouped by category: page views, clicks, form submissions, errors, custom
- Include code snippets (in `details` containers) for events with complex payloads
  or conditional tracking logic

**Gap analysis:**
- For each gap use this format:

  > **Unanswered question:** "What percentage of users complete the checkout flow?"
  >
  > **Suggested event:** `checkout_step_completed` on each step transition
  > with payload `{ step, total_steps }`
  >
  > **Impact:** Enables funnel analysis for the most revenue-critical flow.

- Organize gaps by AARRR (Acquisition, Activation, Revenue, Retention, Referral)
  or a similar product framework.

### dependencies.md — Dependencies

Content:
- **Runtime dependencies** table: Name | Category | Version | Flags
- **Dev dependencies** table: Name | Category | Purpose
- **External services** table: Service | Purpose | Failure impact
- Mermaid `graph` showing external service connections
- Mermaid `pie` chart of dependency categories
- Flag: outdated, deprecated, duplicated, or security-concern items

### data-model.md — Data Model

Skip for pure frontends (but create file with a note explaining why).

Content:
- Mermaid `erDiagram` showing entities and relationships
- Table per entity: Field | Type | Constraints | PM-relevant notes
- What each entity represents in product terms
- Migration status: is the schema managed by a migration tool?

### auth.md — Auth & Security

Content:
- Mermaid `sequenceDiagram` of the auth flow (login, token refresh, logout)
- Auth method description (JWT, sessions, OAuth2, API keys)
- Role/permission table: Role | Capabilities | Restrictions
- List of public vs protected endpoints/pages
- Security observations: CORS, rate limiting, input sanitization, HTTPS
- Include code snippets for auth middleware and role-checking logic with source references

### configuration.md — Configuration

Content:
- Table: Variable | Purpose | Required? | Default | Sensitivity
- Config files list with purpose
- Flag hardcoded secrets or missing defaults
- Deployment notes if detectable

### risks.md — Risk Register

Content:
- Table: Risk | Severity | Category | Affected area | Source | Recommendation
- Include source file and line references for each risk
- For high-severity risks, include a code snippet showing the problematic code
- Categories: Security, Reliability, Performance, Data Integrity, UX, Compliance,
  Maintainability
- Severity: High (data loss/breach/revenue), Medium (degrades over time), Low (improvement opportunity)
- Sort by severity (high first)
- Use `::: danger`, `::: warning`, `::: tip` containers for the top findings

### improvements.md — Suggested Improvements

Content:
- Group by category: Analytics, Security, Performance, UX, Testing, Documentation,
  Dependencies, Architecture
- Per improvement:
  - **What:** one-sentence description
  - **Why it matters:** product impact in PM terms
  - **Effort estimate:** Low / Medium / High
  - **Priority:** impact vs effort assessment
- Cross-reference with risks.md and analytics.md
- Frame as hypotheses: "Consider..." not "You must..."

### glossary.md — Acronyms & Glossary

Content:

**Acronyms table:**
- Acronym | Stands For | What It Means for the Product
- Include project-specific AND general technical acronyms used in the docs
- Don't just expand — explain in product terms

**Glossary table:**
- Term | Definition
- Every technical term used anywhere in the documentation
- Plain-language definitions for a non-engineer
- If a term has product implications, include them

---

## Multi-Repo Cross-Cutting Pages

Added when scanning multiple repositories. These live at the docs root.

### index.md — System Overview

Content:
- Table of all projects: Name | Type badge | Language | Endpoints | Last activity
- Mermaid `graph` showing how all projects connect
  (frontends → BFFs → backends → databases / external services)
- Brief system-purpose summary
- Quick stats: total repos, total endpoints, total deps, shared deps count
- Links to all cross-cutting pages and per-project sections

### cross-repo-deps.md — Cross-Repo Dependency Map

Content:
- Mermaid `graph TD` of inter-repo dependencies
  - Arrows labeled with dependency type (HTTP API, npm package, shared DB, etc.)
  - Color-code: frontend=green, backend=blue, BFF=orange, library=purple
- Table: Source repo | Depends on | Type | What breaks if target is down
- Flag circular dependencies
- Flag single points of failure

### api-contracts.md — API Contract Map

Content:
- Table: API Endpoint | Producer | Consumer(s) | Contract format
- Mermaid `graph LR`: producers left, consumers right, APIs in between
- Flag APIs with no formal contract (no OpenAPI/typed client)
- Flag APIs consumed by multiple repos (broad blast radius on changes)

### shared-deps.md — Shared Dependencies

Content:
- Table: Dependency | Used by | Version per repo | Mismatch?
- Flag version mismatches across repos
- Flag shared external services
- Recommend consolidation opportunities

### risks.md — Consolidated Risk Register

Content:
- Same as single-repo risk register, with added "Repo" column
- Sort by severity, then by repo
- Highlight cross-repo risks ("No repos have rate limiting")
- Summary: X high, Y medium, Z low risks across N repos

### glossary.md — Consolidated Glossary

Content:
- Merged and deduplicated from all project glossaries
- If a term means different things in different repos, note it:
  > **Order** — In `web-app`, the user-facing cart order.
  > In `fulfillment-service`, the warehouse pick-pack-ship instruction.
