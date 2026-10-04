# Outstanding Reader Work

Grouped by owning component. See [`repo-guidance.md`](repo-guidance.md#task-tracking) for ID
and completion rules.

## RDR: Shared reader infrastructure

### RDR-1: Reader test foundation with core reader flows

- **Priority:** High
- **Remaining work:** Add the test layer chosen in RDR-2 with a reusable harness,
  deterministic fixtures for one EPUB, one CBZ, and one PDF, and a small representative set of
  end-to-end flows (open, page forward/back, close, resume position). Playwright is the
  leading candidate, but RDR-2 decides.
- **Acceptance:**
  - Tests run headless locally and in CI with the same command.
  - Each test owns its state and can run in parallel without flakiness.
  - Harness code passes the repository's lint and type checks.
  - The harness is not coupled to one reader implementation, so format-specific PRs can extend it.
- **Status:** Not started.
- **Dependencies:** RDR-2.

### RDR-2: Assess current CI and test coverage against known reader bugs

- **Priority:** High
- **Remaining work:** Review the current CI workflows (`.github/workflows/`) and the Vitest
  setup against the bugs and failure modes in [`reader-bugs.md`](reader-bugs.md). For each
  bug, record which existing layer could have caught it, if any. Use that comparison to decide
  which additional test layers are justified. Real-browser end-to-end, isolated component, and
  visual regression testing (as used by Kavita and Komga) are options to evaluate, not
  decided requirements.
- **Acceptance:**
  - A written gap analysis mapping each listed bug to an existing or proposed test layer.
  - A recommendation for which new layers to add, with the bugs that justify each one.
  - No layer is recommended without at least one concrete bug or failure mode behind it.
- **Status:** Not started.
- **Dependencies:** None.

### RDR-3: Map CI dependencies for reader checks

- **Priority:** Medium
- **Remaining work:** Before finalizing workflow structure, map the reader checks recommended
  by RDR-2. Identify which checks are independent, which need build artifacts or shared setup,
  and which can run concurrently.
- **Acceptance:**
  - A dependency map covering every proposed reader check and its inputs.
  - Clear notes on what can run in parallel and what must be sequenced, used by RDR-4.
- **Status:** Not started.
- **Dependencies:** RDR-2.

### RDR-4: Design a narrowly scoped automatic trigger for reader checks

- **Priority:** Medium
- **Remaining work:** Design how new reader checks run automatically. Keep the initial trigger
  as narrow as practical: changes to the reader plugins (`src/plugins/bookPlayer/`,
  `src/plugins/comicsPlayer/`, `src/plugins/pdfPlayer/`), the test harness and fixtures, and
  the shared dependencies they rely on.
- **Acceptance:**
  - Path filters and trigger events are defined and justified.
  - The shared dependencies included in the trigger are listed with the reason for each.
  - Workflow sequencing follows the RDR-3 dependency map.
- **Status:** Not started.
- **Dependencies:** RDR-2, RDR-3.

### RDR-5: Document how to run reader checks manually

- **Priority:** Medium
- **Remaining work:** Write a runbook for maintainers and reviewers to force the reader checks
  when path-based triggering does not fire, for example through a manual workflow dispatch.
  Explain when doing so is appropriate, such as changes to shared code the path filters miss.
- **Acceptance:**
  - Step-by-step instructions that work for a maintainer without local setup.
  - Guidance on when a manual run is warranted and when it is not.
- **Status:** Not started.
- **Dependencies:** RDR-4.

### RDR-6: Document the staged adoption rationale

- **Priority:** Medium
- **Remaining work:** Explain in contributor-facing documentation that the reader test harness
  is being introduced cautiously. The narrow initial trigger lets the community build
  confidence in the checks and validate the tests themselves under real use before they gate
  normal reader development. State plainly that the narrow scope does not mean the checks are
  only relevant to those paths.
- **Acceptance:**
  - The rationale lives with the canonical documentation for the checks, or the PR description
    if no such document exists yet.
  - It says what would justify widening the trigger later.
- **Status:** Not started.
- **Dependencies:** RDR-4.

## EPUB: bookPlayer

_No outstanding tasks._

## CMX: comicsPlayer

_No outstanding tasks._

## PDF: pdfPlayer

_No outstanding tasks._

## GUIDE: Agent guidance

### GUIDE-1: Make AGENTS.md general to jellyfin-web (fork only)

- **Priority:** Medium
- **Remaining work:** Rewrite the root `AGENTS.md` so it serves any work in jellyfin-web,
  not just the book reader. It stays on the fork and never goes upstream (see
  [repo-guidance.md](repo-guidance.md#working-expectations)). Move reader-specific guidance (reader plugin routing, reference
  project licensing) into `.agents/repo-guidance.md`.
- **Acceptance:**
  - `AGENTS.md` has no reader-specific sections and still routes to canonical docs.
  - Nothing reader-specific is lost; it lives in `.agents/`.
  - `AGENTS.md` does not reference `.agents/`.
- **Status:** Not started.
- **Dependencies:** None.
