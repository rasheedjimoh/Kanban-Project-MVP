# Project Report: Kanban Project MVP

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/f892f7da-c80b-4cab-ac65-679612cce2b4" />


## 1. Executive Summary

Kanban Project MVP is a client-rendered Next.js web application for managing a simple project board. It provides one board with five fixed columns, editable column names, cards with title and details, card creation, card deletion, and drag-and-drop card movement.

The application is for portfolio review, project planning demonstrations, and lightweight task-board workflows. It solves the narrow problem of visualizing project work across fixed workflow stages without requiring accounts, persistence, a backend, or deployment infrastructure.

The inspected codebase does not contain verified Agentic AI functionality. No LLM prompts, agent orchestration, tool-calling logic, model integrations, or AI workflow code were found. The value of the current project is a polished, tested frontend MVP rather than an autonomous AI system.

<img width="1600" height="1000" alt="image" src="https://github.com/user-attachments/assets/e4635833-7ee9-4820-94cb-ba9904f51140" />


## 2. Project Purpose

The project exists to demonstrate a focused Kanban-style project management interface with a small, well-tested feature set. It addresses the need for a clean local demo of task movement, board organization, and simple in-memory workflow management.

The useful outcome is a working single-board UI that can be opened locally, reviewed visually, and validated through automated tests. Because there is no persistence or backend, the project is intentionally low-risk and easy to reset.

## 3. Key Features Verified

| Feature name | What it does | How it was verified | Evidence |
|---|---|---|---|
| Single Kanban board | Renders one project board UI titled "Kanban Project MVP". | Code inspection of `app/page.tsx` and screenshot capture. | `evidence/04-homepage-dashboard.png` |
| Five fixed columns | Displays Backlog, Ready, In Progress, Review, and Done columns from dummy data. | Code inspection of `lib/dummy-board.ts`, component test, screenshot. | `evidence/01-command-output-unit-tests.txt`, `evidence/04-homepage-dashboard.png` |
| Dummy cards | Loads sample cards such as "Map project intake" and "Build dashboard shell". | Code inspection, component test, E2E test, screenshot. | `evidence/01-command-output-unit-tests.txt`, `evidence/03-command-output-e2e-tests.txt` |
| Add card | Adds a card to a selected column using an inline form. | Component test, E2E test, automated screenshot workflow. | `evidence/01-command-output-unit-tests.txt`, `evidence/03-command-output-e2e-tests.txt`, `evidence/05-kanban-workflow-result.png` |
| Delete card | Removes an existing card from the board state. | Component test and E2E test. | `evidence/01-command-output-unit-tests.txt`, `evidence/03-command-output-e2e-tests.txt` |
| Rename column | Allows a column title to be edited in place. | Component test, E2E test, automated screenshot workflow. | `evidence/01-command-output-unit-tests.txt`, `evidence/03-command-output-e2e-tests.txt`, `evidence/05-kanban-workflow-result.png` |
| Drag-and-drop movement | Moves cards between columns with dnd-kit. | Code inspection of `KanbanBoard.tsx`, E2E drag test, screenshot. | `evidence/03-command-output-e2e-tests.txt`, `evidence/05-kanban-workflow-result.png` |
| Escaped user-rendered text | User-entered HTML-like strings render as text rather than executing as markup. | Automated browser workflow entered script-like content and captured rendered output. | `evidence/06-security-escaped-user-input.png` |


<img width="1600" height="1000" alt="image" src="https://github.com/user-attachments/assets/c664ce28-a7d9-4554-9b7b-107694ff4814" />


## 4. Architecture Overview

The project is a frontend-only Next.js application under `frontend/`. It uses React client components, TypeScript types, Tailwind CSS, dnd-kit for drag-and-drop, Vitest/React Testing Library for component tests, and Playwright for browser tests.

No backend components, API routes, databases, authentication services, queues, model providers, or external application services were found.

```text
Browser
  |
  v
Next.js App Router
  |
  v
app/page.tsx
  |
  v
KanbanBoard client component
  |
  +-- React useState board state
  +-- dummyBoard initial data
  +-- board-actions pure functions
  +-- dnd-kit drag sensors and drop handling
  |
  v
KanbanColumn -> EditableColumnTitle / KanbanCard / AddCardForm
```

Frontend components:
- `KanbanBoard`: owns board state, dnd-kit context, add/delete/rename/move callbacks.
- `KanbanColumn`: renders each column, drop zone, empty state, cards, and add-card form.
- `KanbanCard`: renders card title/details, drag handle, and delete button.
- `AddCardForm`: collects title/details and creates cards.
- `EditableColumnTitle`: provides inline title editing.

State management:
- React `useState` in `KanbanBoard`.
- No Zustand, Redux, localStorage, cookies, backend persistence, or file persistence.

Data flow:
- `dummyBoard` initializes the board.
- User actions call pure functions in `lib/board-actions.ts`.
- Updated board objects are stored in React state.
- Refreshing the browser resets to dummy data.

Security boundaries:
- The browser is the only runtime boundary.
- There are no server-side APIs accepting user input.
- There are no secrets or environment variables required by the app.

## 5. Agentic AI Workflow

No Agentic AI workflow was verified in this codebase.

The inspected files do not include:
- LLM model calls.
- Agent prompts or system instructions.
- Tool-call orchestration.
- Retrieval or memory components.
- Autonomous planning loops.
- Human approval gates for agent actions.
- Agent-specific output validation.

The actual workflow is a deterministic UI workflow:

```text
User opens board
  -> React loads dummy board state
  -> User adds, deletes, renames, or drags cards
  -> Board action functions update in-memory state
  -> UI re-renders immediately
```

Error handling is intentionally minimal. Empty card titles are ignored by the add form. Empty column names are rejected by `renameColumn`, which returns the existing board unchanged.

## 6. Security Design

The project has a small security surface because it is frontend-only and has no persistence, backend APIs, authentication, or third-party data submission.

Verified controls:
- User text is rendered through React text interpolation, which escapes markup by default.
- No `dangerouslySetInnerHTML` usage was found in the scoped code scan.
- No secrets, `.env` files, API keys, tokens, or credentials were found.
- No localStorage or cookie persistence was found.
- No API routes or backend request handlers were found.
- Build telemetry was disabled during evidence build to avoid writing outside the project sandbox.

Observed security evidence:
- Script-like and image-event-handler input was added as card content and rendered as visible text rather than executed markup. See `evidence/06-security-escaped-user-input.png`.

<img width="1600" height="1000" alt="image" src="https://github.com/user-attachments/assets/2bd0b788-d4cd-431b-93bb-48189f1798f6" />


Gaps:
- No Content Security Policy is configured.
- No dependency vulnerability remediation was performed as part of this report.
- No explicit input length limits are implemented.
- No rate limiting exists because there is no backend.
- No audit logging exists.
- No authentication or authorization exists.
- No Agentic AI guardrails exist because no Agentic AI system exists in the inspected codebase.

## 7. OWASP Agentic AI and LLM Risk Mapping

| Risk category | How the project addresses it | Evidence | Remaining gap | Recommended improvement |
|---|---|---|---|---|
| Prompt injection | Not applicable to verified functionality; no prompts or LLM inputs exist. | Code inspection found no LLM integration. | If Agentic AI is added later, prompt injection controls will be required. | Add instruction separation, prompt injection tests, tool allowlists, and output validation before adding model calls. |
| Tool misuse / excessive agency | Not applicable; no agent tools or autonomous actions exist. | No tool orchestration code found. | No framework exists for constraining future agent tools. | Define a tool permission model and human approval gates before implementing agent actions. |
| Sensitive information disclosure | Low current exposure; no secrets or environment files were found. | Secret scan returned no matching project files. | No formal secret scanning workflow is configured. | Add pre-commit or CI secret scanning before GitHub publication. |
| Insecure output handling | React escapes user-rendered text; no raw HTML rendering found. | `evidence/06-security-escaped-user-input.png` | No CSP header is configured. | Add a CSP when deploying, especially if future rich text or AI-generated HTML is introduced. |
| Data poisoning / memory poisoning | Not applicable; no persistent memory, database, or retrieval system exists. | Code inspection found no persistence. | Future persistence would need validation and trust boundaries. | Add data validation and provenance controls if persistence is introduced. |
| Excessive permissions | Current app has no backend permissions or external service credentials. | Code inspection found no backend integrations. | Browser-only code still depends on third-party packages. | Maintain dependency review and avoid adding broad backend privileges. |
| Inadequate monitoring | No logging or audit trail exists. | Code inspection. | Not suitable for enterprise auditing. | Add structured logs only if backend or production use is introduced. |
| Model denial of service | Not applicable; no model calls exist. | Code inspection. | Future model usage could introduce cost and rate risks. | Add rate limits, quotas, timeouts, and budget controls before LLM integration. |

## 8. Testing and Validation

Unit and component testing:
- `npm test` ran Vitest and React Testing Library tests.
- Verified rendering five columns, dummy cards, adding a card, deleting a card, renaming a column, and pure move logic.
- Result: 2 test files passed, 9 tests passed.
- Evidence: `evidence/01-command-output-unit-tests.txt`.

End-to-end testing:
- `npm run test:e2e` ran Playwright against a locally started Next app.
- Verified app load, dummy data, add/delete card, rename column, and drag-and-drop card movement.
- Result: 4 Playwright tests passed.
- Evidence: `evidence/03-command-output-e2e-tests.txt`.

Build validation:
- `npm run build` completed a production Next.js build with TypeScript validation.
- Result: build completed and static route `/` was generated.
- Evidence: `evidence/02-command-output-build.txt`.

Runtime validation:
- Screenshot automation started the app locally on an isolated port and captured UI states.
- Evidence: `evidence/04-homepage-dashboard.png`, `evidence/05-kanban-workflow-result.png`.

Security validation:
- Manual/automated browser evidence entered HTML-like strings into card fields and observed them rendered as text.
- Evidence: `evidence/06-security-escaped-user-input.png`.

Missing tests:
- No lint script is defined.
- No accessibility audit is automated.
- No dependency audit evidence is included.
- No formal security test suite exists.
- No Agentic AI workflow tests exist because no Agentic AI workflow was found.

## 9. Evidence Summary

| Evidence ID | File path | What it proves | How it was captured | Why it matters |
|---|---|---|---|---|
| E01 | `evidence/01-command-output-unit-tests.txt` | Unit/component tests passed: 2 files, 9 tests. | `npm test` output redirected to file. | Proves core state and component behavior are covered. |
| E02 | `evidence/02-command-output-build.txt` | Production Next.js build completed successfully. | `NEXT_TELEMETRY_DISABLED=1 npm run build` output redirected to file. | Proves TypeScript/build readiness. |
| E03 | `evidence/03-command-output-e2e-tests.txt` | Playwright E2E tests passed: 4 tests. | `npm run test:e2e` output redirected to file. | Proves key user flows work in a browser. |
| E04 | `evidence/04-homepage-dashboard.png` | App dashboard loads with five columns and dummy data. | Playwright screenshot automation. | Provides visual proof of the running UI. |
| E05 | `evidence/05-kanban-workflow-result.png` | Rename, add-card, and drag movement were performed. | Playwright screenshot automation. | Provides visual proof of the core Kanban workflow. |
| E06 | `evidence/06-security-escaped-user-input.png` | HTML-like user input renders as text in a card. | Playwright screenshot automation. | Shows a relevant frontend output-safety behavior. |

## 10. Deployment Readiness

Ready:
- Local demo: ready. The app installs, builds, tests, and runs locally.
- Portfolio showcase: ready for a frontend MVP presentation.
- GitHub publication: mostly ready, assuming generated folders such as `node_modules`, `.next`, and evidence logs are not committed unless intentionally included.
- Recruiter or hiring manager review: ready as a polished frontend project.

Needs improvement:
- Vercel or cloud deployment: likely deployable as a standard Next.js app, but no live deployment was performed or verified in this report.
- Enterprise review: not ready. The project lacks authentication, authorization, audit logging, dependency governance evidence, CSP/security headers, and formal security testing.
- Agentic AI review: not ready as an Agentic AI project because no agent workflow was found.

## 11. Strengths

- Clean frontend-only architecture with a narrow, understandable scope.
- Strong automated test coverage for the MVP feature set.
- Playwright verifies realistic browser behavior including drag-and-drop.
- React state and pure board action functions keep behavior simple and reviewable.
- No backend, database, authentication, or persistence reduces operational and data-security risk.
- UI is visually polished enough for portfolio demonstration.
- User-supplied strings are rendered as escaped React text rather than raw HTML.

## 12. Gaps and Recommendations

| Gap | Risk | Recommended fix | Priority |
|---|---|---|---|
| No verified Agentic AI functionality | The project may be misrepresented if described as Agentic AI. | Either reframe it as a Kanban frontend MVP or add a real, tested agent workflow with clear guardrails. | High |
| No CSP or security headers | Future deployment has weaker browser hardening. | Add security headers in `next.config.ts` or deployment platform configuration. | Medium |
| No lint script | Style and quality issues may be missed outside tests. | Add ESLint or Next lint-compatible checks and run them in CI. | Medium |
| No dependency audit evidence | Known package vulnerabilities may go untracked. | Run `npm audit`, review findings, and document accepted risk or fixes. | Medium |
| No input length limits | Very large titles/details could degrade UI quality. | Add reasonable max lengths for card title, card details, and column names. | Low |
| No accessibility audit | Keyboard/screen reader issues may remain. | Add Playwright accessibility checks or manual accessibility review. | Low |
| No cloud deployment proof | Deployment readiness is not externally proven. | Deploy to Vercel or another platform and capture deployment evidence. | Low |

## 13. Final Verdict

Current maturity level: polished frontend MVP.

Portfolio-ready: yes, if presented as a Kanban project management frontend.

Demo-ready: yes for local demo and reviewer walkthrough.

Production-ready: no. It lacks production security headers, deployment verification, governance checks, auditability, and persistence requirements that a real multi-user project tool would usually need.

Agentic AI-ready: no. The inspected codebase does not implement an Agentic AI workflow.

The single highest-impact improvement is to align the project positioning with the implementation: either present it honestly as a tested Kanban MVP, or add a real Agentic AI workflow with explicit tool boundaries, prompt-injection testing, and evidence-backed guardrails.

## 14. Appendix

Commands run:

```bash
npm test
NEXT_TELEMETRY_DISABLED=1 npm run build
npm run test:e2e
node scripts/capture-evidence.mjs
```

Runtime commands used by project tooling:

```bash
npm run dev
npm run dev:background
npm run build
npm run start
npm run test:e2e
```

Evidence generated:
- `evidence/01-command-output-unit-tests.txt`
- `evidence/02-command-output-build.txt`
- `evidence/03-command-output-e2e-tests.txt`
- `evidence/04-homepage-dashboard.png`
- `evidence/05-kanban-workflow-result.png`
- `evidence/06-security-escaped-user-input.png`

Assumptions made:
- The report covers the code available in this workspace at review time.
- `frontend/` is the complete application because no backend or other app directories were present at the repository root.
- The project was evaluated as implemented, not as originally intended by any prior prompt.
- Evidence screenshots were captured from a local Next.js runtime using Playwright.
