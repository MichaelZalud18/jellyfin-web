# Jellyfin Web Agent Guide

Routing and boundaries for agents working in this repository. Detailed procedures live in the
documents linked below; do not duplicate them here.

## Read first

- [`README.md`](README.md) for what the project is and how to build it.
- [`CONTRIBUTING.md`](CONTRIBUTING.md) for code rules, architecture, and directory structure.
- The [LLM development policy](https://jellyfin.org/docs/general/contributing/llm-policies)
  before using code assistance on any change.

## Where things live

- Book and comic reader plugins: `src/plugins/bookPlayer/` (EPUB), `src/plugins/comicsPlayer/`
  (comic archives), `src/plugins/pdfPlayer/` (PDF).
- Application architecture and legacy-to-modern mapping: [`CONTRIBUTING.md`](CONTRIBUTING.md#application-architecture).
- Build, lint, and test commands: `scripts` in [`package.json`](package.json).
- CI implementation: [`.github/workflows/`](.github/workflows/).
- Supported browsers: the `browserslist` section of [`package.json`](package.json).

## Boundaries

- New code is TypeScript and uses the Jellyfin TypeScript SDK for API calls.
- Do not edit translations other than `en-us`; other languages come from Weblate.
- Do not rename existing translation keys without a strong reason.
- Keep a change to a single focus and match the surrounding code style.
- Respect the supported-browser floor; avoid features those engines cannot run or polyfill.
- Do not copy code from GPL or AGPL projects into this repository.
- Commit, push, or open pull requests only with explicit authorization.

## Validation

Run lint, type check, and unit tests through the `package.json` scripts before reporting work
as done. State what ran and what was not verified.

## Pull requests

Complete every section of [`.github/pull_request_template.md`](.github/pull_request_template.md),
including Code assistance, as described in [`CONTRIBUTING.md`](CONTRIBUTING.md#pull-requests).
