# VitePress Scaffold Template

This document contains the exact files the LLM should generate for the docs site.
Copy these templates, replacing placeholders with actual scan data.

---

## package.json

```json
{
  "name": "{{PROJECT_SLUG}}-docs",
  "private": true,
  "scripts": {
    "docs:dev": "vitepress dev docs",
    "docs:build": "vitepress build docs",
    "docs:preview": "vitepress preview docs"
  },
  "devDependencies": {
    "vitepress": "^1.6.3",
    "vitepress-plugin-mermaid": "^2.0.17",
    "mermaid": "^11.4.1"
  }
}
```

Replace `{{PROJECT_SLUG}}` with the project name in kebab-case. For multi-repo,
use a descriptive name like `platform-docs` or `my-org-docs`.

---

## docs/.vitepress/config.mts

### Single-repo config

```typescript
import { defineConfig } from 'vitepress'
import { withMermaid } from 'vitepress-plugin-mermaid'

export default withMermaid(
  defineConfig({
    title: '{{PROJECT_NAME}} Docs',
    description: 'Product documentation for {{PROJECT_NAME}}',

    themeConfig: {
      nav: [
        { text: 'Home', link: '/' },
      ],

      sidebar: [
        {
          text: 'Overview',
          items: [
            { text: 'Executive Summary', link: '/' },
            { text: 'Architecture', link: '/architecture' },
          ]
        },
        {
          text: 'Features & Surface',
          items: [
            { text: 'Routes & Endpoints', link: '/routes' },
            { text: 'User Inputs', link: '/inputs' },
            { text: 'Analytics & Tracking', link: '/analytics' },
          ]
        },
        {
          text: 'Technical Details',
          items: [
            { text: 'Dependencies', link: '/dependencies' },
            { text: 'Data Model', link: '/data-model' },
            { text: 'Auth & Security', link: '/auth' },
            { text: 'Configuration', link: '/configuration' },
          ]
        },
        {
          text: 'Insights',
          items: [
            { text: 'Risk Register', link: '/risks' },
            { text: 'Suggested Improvements', link: '/improvements' },
          ]
        },
        {
          text: 'Reference',
          items: [
            { text: 'Acronyms & Glossary', link: '/glossary' },
          ]
        }
      ],

      search: {
        provider: 'local'
      },

      outline: {
        level: [2, 3]
      }
    },

    mermaid: {},
    mermaidPlugin: {
      class: 'mermaid'
    }
  })
)
```

### Multi-repo config

For multi-repo, use VitePress's "multiple sidebars" feature. Each project directory
gets its own sidebar, and the root pages get a top-level sidebar.

```typescript
import { defineConfig } from 'vitepress'
import { withMermaid } from 'vitepress-plugin-mermaid'

export default withMermaid(
  defineConfig({
    title: '{{ORG_NAME}} Platform Docs',
    description: 'Product documentation across all projects',

    themeConfig: {
      nav: [
        { text: 'Overview', link: '/' },
        // Add one nav item per project:
        { text: '{{Project A}}', link: '/{{project-a}}/' },
        { text: '{{Project B}}', link: '/{{project-b}}/' },
        // ...
      ],

      sidebar: {
        // Root-level cross-cutting pages
        '/': [
          {
            text: 'System Overview',
            items: [
              { text: 'Home', link: '/' },
              { text: 'Cross-Repo Dependencies', link: '/cross-repo-deps' },
              { text: 'API Contracts', link: '/api-contracts' },
              { text: 'Shared Dependencies', link: '/shared-deps' },
              { text: 'Risk Register', link: '/risks' },
              { text: 'Glossary', link: '/glossary' },
            ]
          },
          {
            text: 'Projects',
            items: [
              // One link per project to its index page:
              { text: '{{Project A}}', link: '/{{project-a}}/' },
              { text: '{{Project B}}', link: '/{{project-b}}/' },
            ]
          }
        ],

        // Per-project sidebar (shown when navigating within that project)
        '/{{project-a}}/': [
          {
            text: '← All Projects',
            items: [
              { text: 'Back to Overview', link: '/' }
            ]
          },
          {
            text: '{{Project A}}',
            items: [
              { text: 'Summary', link: '/{{project-a}}/' },
              { text: 'Architecture', link: '/{{project-a}}/architecture' },
              { text: 'Routes & Endpoints', link: '/{{project-a}}/routes' },
              { text: 'User Inputs', link: '/{{project-a}}/inputs' },
              { text: 'Analytics', link: '/{{project-a}}/analytics' },
              { text: 'Dependencies', link: '/{{project-a}}/dependencies' },
              { text: 'Data Model', link: '/{{project-a}}/data-model' },
              { text: 'Auth & Security', link: '/{{project-a}}/auth' },
              { text: 'Configuration', link: '/{{project-a}}/configuration' },
              { text: 'Risks', link: '/{{project-a}}/risks' },
              { text: 'Improvements', link: '/{{project-a}}/improvements' },
            ]
          }
        ],

        // Repeat for each project...
      },

      search: {
        provider: 'local'
      },

      outline: {
        level: [2, 3]
      }
    },

    mermaid: {},
    mermaidPlugin: {
      class: 'mermaid'
    }
  })
)
```

---

## Page templates

Every Markdown page should start with YAML frontmatter. Below are templates for each page.

### Home page (single-repo): `docs/index.md`

```markdown
---
layout: doc
title: Executive Summary
---

# {{PROJECT_NAME}}

<Badge type="tip" text="{{Repo Type}}" /> <Badge type="info" text="{{Language}}" />

*Generated on {{DATE}}*

## At a Glance

| Metric | Value |
|--------|-------|
| **Repo type** | {{Frontend / Backend / BFF / etc.}} |
| **Primary language** | {{TypeScript / Python / etc.}} |
| **Routes / Endpoints** | {{count}} |
| **Dependencies** | {{count}} runtime, {{count}} dev |
| **Analytics events** | {{count}} tracked, {{count}} gaps identified |
| **Last updated** | {{date from git}} |

## What This Project Does

{{3–5 sentence plain-English summary. A PM who reads only this section should understand
the product purpose, who it serves, and why it exists.}}

## Architecture at a Glance

```mermaid
graph TD
    {{architecture diagram here}}
```

## Key Findings

::: tip 💡 Highlights
{{2-3 most notable positive findings}}
:::

::: warning ⚠️ Top Risks
{{2-3 most important risks, each one line}}
:::

## Pages in This Documentation

| Page | What You'll Learn |
|------|-------------------|
| [Architecture](./architecture) | How the system is structured |
| [Routes & Endpoints](./routes) | Every page or API the system exposes |
| [User Inputs](./inputs) | Every form, search bar, and file upload |
| [Analytics](./analytics) | What's tracked, what's missing |
| [Dependencies](./dependencies) | Libraries and external services |
| [Data Model](./data-model) | Database tables and relationships |
| [Auth & Security](./auth) | Login flow, roles, permissions |
| [Configuration](./configuration) | Environment variables and config |
| [Risk Register](./risks) | All risks ranked by severity |
| [Improvements](./improvements) | Actionable recommendations |
| [Glossary](./glossary) | Technical terms explained |
```

### Home page (multi-repo): `docs/index.md`

```markdown
---
layout: doc
title: System Overview
---

# {{ORG / SYSTEM NAME}} — Platform Documentation

*Generated on {{DATE}}*

## Projects in This System

| Project | Type | Language | Endpoints | Status |
|---------|------|----------|-----------|--------|
| [{{Project A}}](./{{project-a}}/) | <Badge type="tip" text="Frontend" /> | TypeScript | {{N}} routes | {{Last commit date}} |
| [{{Project B}}](./{{project-b}}/) | <Badge type="info" text="Backend" /> | Python | {{N}} endpoints | {{Last commit date}} |
| ... | | | | |

## How They Connect

```mermaid
graph LR
    {{system-wide dependency diagram}}
```

## Cross-Cutting Documentation

| Page | What You'll Learn |
|------|-------------------|
| [Cross-Repo Dependencies](./cross-repo-deps) | Which projects depend on which |
| [API Contracts](./api-contracts) | Who produces and consumes each API |
| [Shared Dependencies](./shared-deps) | Libraries used across multiple projects |
| [Risk Register](./risks) | All risks across all projects |
| [Glossary](./glossary) | Unified technical glossary |
```

### Standard page template

Use this structure for all other pages (routes, inputs, analytics, etc.):

```markdown
---
title: {{Page Title}}
---

# {{Page Title}}

{{1-2 sentence summary of what this page covers and why a PM should care.}}

## {{First major section}}

{{Content — tables, prose, diagrams as appropriate.}}

```mermaid
{{diagram if applicable}}
```

::: tip 💡 Insight
{{Notable finding with product implication}}
:::

::: warning ⚠️ Risk
{{Risk finding with product implication}}
:::

## {{Next section}}

...
```

### Glossary page: `docs/glossary.md`

```markdown
---
title: Acronyms & Glossary
---

# Acronyms & Glossary

## Acronyms

| Acronym | Stands For | What It Means for the Product |
|---------|------------|-------------------------------|
| API | Application Programming Interface | A way for different software systems to talk to each other. When we say "the API," we mean the set of URLs that other systems (or our frontend) call to get or send data. |
| BFF | Backend-for-Frontend | A thin server layer that sits between the frontend app and the real backend services. It reshapes data so the frontend gets exactly what it needs in one call instead of many. |
| JWT | JSON Web Token | A small signed data packet the app uses to prove a user is logged in. It avoids checking the database on every request, but it can't be revoked once issued (until it expires). |
| {{...}} | {{...}} | {{...}} |

## Glossary

| Term | Definition |
|------|-----------|
| **Endpoint** | A specific URL that the backend responds to. Think of it like a phone number — each one connects you to a different service or piece of data. |
| **Middleware** | Code that runs automatically before a request reaches its main handler. Like a security checkpoint — it can check authentication, log the request, or reject it before it goes further. |
| **Migration** | A versioned change to the database structure. Like a blueprint revision — it records what changed so every copy of the database can be updated consistently. |
| **Rate limiting** | A cap on how many requests a user or system can make in a time window. Prevents abuse and keeps the system stable under load. |
| {{...}} | {{...}} |
```

The glossary should include ALL technical terms used anywhere in the documentation.
Write definitions for a non-engineer. Don't define jargon with more jargon.

For multi-repo, the root glossary is a consolidated merge. If a term means something
different in different projects, note it:
> **Order** — In `web-app`, refers to the user-facing shopping cart order. In
> `fulfillment-service`, refers to the warehouse pick-pack-ship instruction.

---

## Omitted pages

If a page is not applicable (e.g., "Data Model" for a pure frontend), still create
the file but with a short note:

```markdown
---
title: Data Model
---

# Data Model

This project is a frontend application and does not have its own database layer.
Data is fetched from the [API Server](../api-server/) backend.
```

This prevents broken links in the sidebar and tells the reader it was intentionally
skipped, not forgotten.

---

## Build verification

After generating all files, run a quick sanity check:

```bash
# Verify all sidebar links have matching files
# (list all .md files and compare against config)
find docs -name "*.md" | sort
```

Ensure every link in `config.mts` has a corresponding `.md` file and vice versa.
