# Outstanding Reader Work

Grouped by owning component. See [`repo-guidance.md`](repo-guidance.md#task-tracking) for ID
and completion rules.

## RDR: Shared reader infrastructure

### RDR-1: Playwright foundation with core reader flows

- **Priority:** High
- **Remaining work:** Add Playwright to the repository with a reusable harness, deterministic
  fixtures for one EPUB, one CBZ, and one PDF, and a small representative set of end-to-end
  flows (open, page forward/back, close, resume position).
- **Acceptance:**
  - Tests run headless locally and in CI with the same command.
  - Each test owns its state and can run in parallel without flakiness.
  - Harness code passes the repository's lint and type checks.
  - The harness is not coupled to one reader implementation, so format-specific PRs can extend it.
- **Status:** Not started.
- **Dependencies:** None.

## EPUB: bookPlayer

_No outstanding tasks._

## CMX: comicsPlayer

_No outstanding tasks._

## PDF: pdfPlayer

_No outstanding tasks._
