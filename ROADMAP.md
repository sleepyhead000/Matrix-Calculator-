# Matrix Calculator Full Plan

## Vision
Build a lightweight, browser-only matrix calculator that is mathematically correct, educational (step-by-step), mobile-friendly, and easy to deploy publicly via Vercel.

## Product Goals
- Keep it simple: single-page app, no backend required.
- Show complete steps for each operation.
- Maintain high trust in math correctness.
- Make it accessible on desktop and mobile.
- Keep one-click deployment and easy maintenance.

## Current Baseline (May 10, 2026)
- Single file app with dynamic matrix inputs and custom expression parser.
- Supports add, subtract, multiply, transpose, determinant, inverse.
- Displays step-by-step output and has EN/BN language support.
- Deployed form not yet standardized for hosted production.

## Major Issues to Address
1. Correctness policy mismatch:
- Currently pads matrices with zeros for invalid dimensions in add/subtract/multiply.
- This is useful as an optional mode, but should not be default in a learning tool.

2. Parser/operator mismatch:
- `/` appears in tokenizer/operator hints but division behavior is not clearly implemented as matrix algebra.

3. Step detail consistency:
- Some operations are fully expanded, some are summarized.
- Inverse path skips an explicit minors stage in presentation.

4. Maintainability:
- Entire app logic in one large file makes iteration risky.

5. Public release readiness:
- No explicit versioned release process and quality gate.

## Delivery Plan (Phased)

## Phase 0 - Deployment Foundation (Done in this repo)
Objective: make the app hostable immediately.

Deliverables:
- `index.html` entry file for static hosting.
- `vercel.json` to enforce static rewrite and basic security headers.
- Updated `README.md` with deployment instructions.

Acceptance criteria:
- App opens on Vercel URL and operations run client-side.
- Refreshing deep URL still serves the app.

## Phase 1 - Mathematical Correctness First
Objective: prioritize accurate linear algebra behavior.

Tasks:
- Enforce strict dimension checks by default:
  - Add/Subtract: same rows and cols only.
  - Multiply: cols(A) == rows(B) only.
- Introduce optional `Compatibility Mode` toggle for current zero-padding behavior.
- Standardize error messages for all invalid operations.

Acceptance criteria:
- Invalid dimensions return explicit educational errors.
- Optional compatibility mode reproduces old behavior when enabled.

## Phase 2 - Calculation Engine and Parser Hardening
Objective: make input behavior predictable and robust.

Tasks:
- Decide division policy:
  - Option A: remove `/` entirely from supported expression grammar.
  - Option B: implement scalar division only with clear constraints.
- Improve parser errors with token context.
- Add explicit expression validation before evaluation.
- Normalize number formatting for stability and readability.

Acceptance criteria:
- No ambiguous parser behavior.
- Invalid expressions fail with clear user-facing feedback.

## Phase 3 - Step-by-Step Pedagogy Expansion
Objective: guarantee complete and consistent educational output.

Tasks:
- Add unified step template for all operations.
- Expand inverse into explicit stages:
  1. Determinant
  2. Minors
  3. Cofactors
  4. Adjugate
  5. Divide by determinant
- Add `Brief` and `Detailed` step modes.
- Ensure each result element can be traced to sub-calculations.

Acceptance criteria:
- Every operation has a reproducible step trace.
- Large matrices remain usable via brief mode.

## Phase 4 - UX and Accessibility
Objective: improve usability while keeping simplicity.

Tasks:
- Improve mobile layout and step block readability.
- Add keyboard flow (tab order, Enter-to-calculate).
- Add ARIA labels and semantic structure for screen readers.
- Improve contrast and focus indicators.

Acceptance criteria:
- Smooth operation on common mobile viewport sizes.
- Keyboard-only usage is functional.
- No severe accessibility blockers in manual audit.

## Phase 5 - Codebase Maintainability (Still Deployable as Static)
Objective: reduce future regression risk.

Tasks:
- Split logic into clear modules (engine/parser/ui/i18n) in source form.
- Keep production output as simple static files.
- Add linting and formatting standards.

Acceptance criteria:
- Feature changes are isolated and testable.
- Public deployment remains static and zero-backend.

## Phase 6 - Quality Gate and Release Process
Objective: ship reliable updates safely.

Tasks:
- Add test cases for core operations and edge cases.
- Add sanity checks:
  - known determinants
  - inverse verification (`A * inv(A) ˜ I`)
  - parser invalid inputs
- Add pre-release checklist and semantic versioning.

Acceptance criteria:
- All checks pass before production deployment.
- Release notes capture user-impacting changes.

## Suggested Timeline
- Week 1: Phase 1 + Phase 2
- Week 2: Phase 3
- Week 3: Phase 4
- Week 4: Phase 5 + Phase 6

## Vercel Deployment Workflow

## Initial setup
1. Connect GitHub repo to Vercel.
2. Deploy default branch.
3. Set production branch (usually `main`).

## Ongoing releases
1. Create feature branch.
2. Implement + test changes.
3. Open pull request (preview deployment auto-generated by Vercel).
4. Validate preview on desktop and mobile.
5. Merge to production branch.
6. Vercel auto-deploys production.

## Operational Notes
- No server costs for backend, since app is static.
- Analytics can be added later (privacy-respecting, optional).
- Custom domain can be attached via Vercel project settings.

## Risks and Mitigations
1. Performance degradation from very detailed steps on larger matrices.
- Mitigation: brief mode + rendering chunking + matrix size limits.

2. User confusion when strict mode rejects old workflows.
- Mitigation: compatibility toggle with clear explanatory text.

3. Regression risk from refactor.
- Mitigation: phase-gated rollout plus baseline tests before refactor.

## Definition of Done (Public v1.1)
- Strict math mode enabled by default.
- Clear, complete step-by-step traces for all supported operations.
- Mobile-friendly UI and keyboard accessibility baseline.
- Vercel production deployment active with documented release flow.
