# Code Lens

`code-lens` is a Codex skill for turning a codebase into a VitePress documentation site
written for product managers and other non-engineering stakeholders.

## Features

- Detects single-repo, multi-repo, and monorepo layouts before generating docs.
- Classifies projects as frontend, backend, BFF, fullstack, library/SDK, infrastructure,
  or other.
- Uses focused checklists to scan routes, inputs, analytics, dependencies, auth,
  configuration, data models, and operational risks.
- Produces VitePress-ready page structures with Mermaid diagrams and PM-friendly writing
  guidance.
- Covers both project-level docs and cross-repo system views such as dependency maps,
  API contracts, shared dependencies, risks, and glossary entries.
- Includes scaffold templates for `package.json`, VitePress config, sidebars, and page
  skeletons.

## Repo Contents

- `SKILL.md`: Main skill trigger and workflow.
- `REQUIREMENTS.md`: Required and recommended tools, install commands, and verification steps.
- `frontend-checklist.md`: Frontend scan guidance.
- `backend-checklist.md`: Backend scan guidance.
- `bff-checklist.md`: BFF-specific scan guidance.
- `site-structure.md`: Page-by-page content requirements.
- `vitepress-scaffold.md`: Template output structure and example content.
