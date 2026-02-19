# cobalt agent instructions

this file defines repository-specific guidance for coding agents working in this monorepo.

## repository layout

- `api/`: cobalt processing API (node.js + express, esm).
- `web/`: cobalt frontend (sveltekit + vite).
- `packages/`: shared workspace packages (`@imput/version-info`, `api-client`).
- `docs/`: operational and API documentation.

## product and scope constraints

- cobalt is for downloading **publicly accessible** content only.
- do not implement or extend behavior for:
  - paid/private content access bypasses,
  - DRM bypass workflows,
  - scraping unrelated user data.
- if a requested feature conflicts with the above, stop and flag it.

## change principles

- keep changes minimal and focused on the requested task.
- fix root causes; avoid broad refactors unless required.
- preserve existing architecture and naming unless there is a clear defect.
- update docs in `docs/` when behavior, config, or API shape changes.

## coding conventions (observed in repo)

- use ESM imports/exports.
- use 4-space indentation.
- use double quotes and semicolons in js/ts.
- prefer small, composable helpers over large inline logic blocks.
- follow existing error response patterns instead of inventing new ones.

## api-specific notes (`api/`)

- runtime: node `>=18`.
- key entry points:
  - `src/cobalt.js` bootstraps API startup.
  - `src/core/api.js` wires middleware, auth, and main routes.
  - `src/processing/` holds matching/request/service logic.
  - `src/stream/` handles stream and tunnel behavior.
- run checks relevant to changes:
  - `cd api && pnpm start` for startup sanity.
  - `cd api && pnpm test` for utility test flow.

## web-specific notes (`web/`)

- runtime: node `>=20`, pnpm `>=9`.
- key areas:
  - `src/lib/api/` API request/session integration.
  - `src/routes/` pages and route-level behavior.
  - `src/lib/state/` store-based app state.
- run checks relevant to changes:
  - `cd web && pnpm check` for type and svelte diagnostics.
  - `cd web && pnpm build` for production build validation.

## commands and workflow

- install dependencies from repo root with `pnpm install`.
- prefer targeted validation first, then broader checks if needed.
- do not add new dependencies unless necessary for the requested change.

## commit and pr guidance

- use conventional, scoped commit titles (example: `api/stream: handle invalid mux target`).
- keep commit history clean and focused.
- if creating a PR, use draft mode unless explicitly requested otherwise.

## licensing and branding caution

- api code is AGPL-3.0; web code is CC-BY-NC-SA-4.0.
- do not add guidance that enables commercial reuse of web branding/assets.
- treat branding/mascot assets as restricted per `web/README.md`.

## when uncertain

- prefer the simplest implementation consistent with existing patterns.
- if requirements are ambiguous, document assumptions in the PR/summary.
