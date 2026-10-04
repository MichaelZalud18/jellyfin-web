# Repository Agent Guidance (jellyfin-web)

Repository-scoped working guidance for agents and contributors working on the book and comic
reader. This directory lives only on the fork's working branches and never goes into a pull
request. Durable, product-relevant guidance lives in [`AGENTS.md`](../AGENTS.md) and the
canonical project docs it routes to.

## Directory contents

- `repo-guidance.md` — this entry point and inventory.
- [`tasks.md`](tasks.md) — outstanding shared reader work, grouped by component.
- [`reader-bugs.md`](reader-bugs.md) — known reader bugs used as test targets and regression guards.
- [`skills/`](skills/README.md) — reusable skill packages and their index.

## Objectives

- Close the root problem: reader code here is not tested enough, and the gap can surface as
  bugs in downstream clients that render or consume it. Treat downstream bug reports as
  evidence and test targets when their root cause plausibly lives in this repository.
- Build test infrastructure for the reader first: Playwright end-to-end coverage of core
  reading flows, then visual regression, interaction, persistence, performance, and
  accessibility coverage.
- Upgrade reader UX and format support on top of that safety net.
- Keep the reader consistent with Jellyfin's library model rather than inventing a separate one.

## Reader layout

| Component | Path | Engine |
| --- | --- | --- |
| EPUB reader | `src/plugins/bookPlayer/` (`BookOsd/` is the React overlay) | `epubjs` |
| Comic reader | `src/plugins/comicsPlayer/` | `libarchive.js`, `swiper` |
| PDF reader | `src/plugins/pdfPlayer/` | `pdfjs-dist` |

## Task tracking

- Every task in `tasks.md` belongs to the section of the component that owns it and uses that
  component's prefix plus a unique number. Prefixes follow the owning component, not the kind
  of work.

  | Prefix | Owner |
  | --- | --- |
  | `RDR` | Shared reader infrastructure: test harness, fixtures, cross-format behavior |
  | `EPUB` | `bookPlayer` |
  | `CMX` | `comicsPlayer` |
  | `PDF` | `pdfPlayer` |
  | `GUIDE` | Agent guidance (`AGENTS.md`, `.agents/`) |

- Each active task records remaining work, priority, acceptance criteria, evidence or status,
  and dependencies.
- When a task is completed, remove it from `tasks.md` in the completing commit and record the
  completion in the commit message with the internal `tasks-completed` tag and the task ID,
  for example `tasks-completed: RDR-1`. Git history is the completion record.
- Never reuse a task ID. Follow-up work gets a fresh ID and links the earlier one.

## Working expectations

- Follow [`AGENTS.md`](../AGENTS.md) and [`CONTRIBUTING.md`](../CONTRIBUTING.md).
- Validate with `npm run lint`, `npm run build:check`, and `npm test` before reporting work as done.
  Report which checks ran and what remains unverified.
- Keep each PR to a single focus. Record adjacent issues found while testing as new tasks.
- Treat test infrastructure as production code: lint it, keep it deterministic and isolated,
  and design harnesses to be reused by later reader PRs.
- Agent files stay on the fork. Jellyfin's [LLM policy](https://jellyfin.org/docs/general/contributing/llm-policies/) forbids committing LLM
  metafiles or other editor-created non-code files, so `AGENTS.md`, `CLAUDE.md`, `.agents/`,
  and `.claude/` never go into an upstream pull request. Cut PR branches from
  `upstream/master`, not from a branch carrying agent files, and bring over only code and test
  commits. Before opening a PR, this must print nothing:
  `git diff --name-only upstream/master...HEAD | grep -E '^(AGENTS\.md|CLAUDE\.md|\.agents/|\.claude/)'`.
  Do not add these paths to the tracked `.gitignore`; that would itself be a change in the PR.
- GPL and AGPL reference projects (Kavita, KOReader, Calibre-Web, Codexa) are design and
  testing references only. Do not copy their code. MIT projects (Komga, Prose Reader) still
  need license review before reuse.
