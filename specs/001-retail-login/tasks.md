# Tasks: Retail Login

**Input**: Design documents from `/specs/001-retail-login/`

**Prerequisites**: plan.md, spec.md, research.md, data-model.md, contracts/login-ui.md, quickstart.md

**Tests**: Include focused pure unit tests for email and DOB validation as specified
in the plan. Do not add integration or end-to-end tests.

**Organization**: Tasks are grouped by user story and ordered for incremental,
independently verifiable delivery.

## Phase 1: Setup

**Purpose**: Add the planned feature/test dependencies and generate the requested UI primitives.

- [ ] T001 Add `date-fns` and Vitest, configure the `test:unit` npm script, and update `package.json` and `package-lock.json`.
- [ ] T002 Configure Vitest for TypeScript unit tests in `vitest.config.ts`.
- [ ] T003 Initialize shadcn/ui for this Next.js/Tailwind v4 project in `components.json` and generate only the needed primitives in `lib/utils.ts`, `components/ui/button.tsx`, `components/ui/input.tsx`, `components/ui/checkbox.tsx`, `components/ui/label.tsx`, `components/ui/card.tsx`, and `components/ui/alert.tsx`.

---

## Phase 2: Foundational

**Purpose**: Establish shared theme tokens and app metadata used by all stories.

- [ ] T004 Define semantic retail color tokens using Tailwind CSS v4 `@theme inline` in `app/globals.css`, preserving the existing Tailwind import and mapping colors to generated `--color-*` utilities.
- [ ] T005 Replace starter metadata with retail demo title and description in `app/layout.tsx`.

**Checkpoint**: Shared dependencies, UI primitives, theme, and app shell are ready.

---

## Phase 3: User Story 1 - Sign In to the Retail Demo (Priority: P1) 🎯 MVP

**Goal**: Deliver the responsive login form with required email, password, and DOB; validation; simulated sign-in; password visibility; and email-only Remember Me.

**Independent Test**: Use the displayed synthetic demo credentials and a valid DOB to reach the clearly labeled signed-in demo state. Verify missing fields, malformed email, invalid/future DOB, and invalid credentials are rejected with clear feedback. Verify password visibility preserves the value and Remember Me restores only the email on a later visit.

### Tests for User Story 1

- [ ] T006 [P] [US1] Add Vitest unit cases for required and malformed email, whitespace trimming, required password, empty/malformed/future DOB, and valid DOB boundaries in `tests/unit/login-validation.test.ts`.

### Implementation for User Story 1

- [ ] T007 [US1] Implement pure email, password, and DOB validation plus the documented synthetic credential comparison in `lib/login-validation.ts`; DOB must be canonical `yyyy-MM-dd`, a valid calendar date, and no later than today's local date.
- [ ] T008 [P] [US1] Implement versioned, namespaced localStorage read/write/remove helpers for the email only in `lib/remembered-email.ts`; report storage failures explicitly and never persist password, DOB, credentials, or sign-in state.
- [ ] T009 [US1] Build the accessible login form and simulated success/failure states in `components/login/login-form.tsx`, using shadcn/ui primitives and a native date input with a client-computed local-today `max`; include visible demo credentials, field-associated errors, keyboard-operable show/hide, and Remember Me behavior.
- [ ] T010 [US1] Render the login form at the root route in `app/page.tsx` and provide Forgot Password? and Sign Up / Register links to `/forgot-password` and `/register`.

**Checkpoint**: Story 1 works independently as the MVP and satisfies its sign-in, validation, accessibility, and Remember Me scenarios.

---

## Phase 4: User Story 2 - Recover Account Access (Priority: P2)

**Goal**: Provide a demo-only recovery screen that clearly states real recovery requests are not processed.

**Independent Test**: From the login page, activate Forgot Password? and verify the recovery notice appears, no account request is submitted, and a keyboard-accessible link returns to login.

- [ ] T011 [P] [US2] Create the demo-only recovery screen and accessible return-to-login link in `app/forgot-password/page.tsx`; state clearly that no real recovery request is processed and do not collect or submit account data.

**Checkpoint**: Story 2 can be verified from the login link without an external service.

---

## Phase 5: User Story 3 - Create a Retail Account (Priority: P3)

**Goal**: Provide a demo-only registration screen that clearly states real accounts are not created.

**Independent Test**: From the login page, activate Sign Up / Register and verify the registration notice appears, no real account is created, and a keyboard-accessible link returns to login.

- [ ] T012 [P] [US3] Create the demo-only registration screen and accessible return-to-login link in `app/register/page.tsx`; state clearly that real accounts are not created and do not collect or submit account data.

**Checkpoint**: Story 3 can be verified from the login link without an external service.

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Validate the assembled UI, requirements, and design guide without adding integration or E2E tests.

- [ ] T013 Review the flows for keyboard operation, field/error associations, responsive usability at 320 px and desktop widths, and future-date prevention in `app/page.tsx`, `components/login/login-form.tsx`, `app/forgot-password/page.tsx`, and `app/register/page.tsx`.
- [ ] T014 Run the documented Vitest unit tests, `npm run lint`, `npm run build`, and manual acceptance scenarios from `specs/001-retail-login/quickstart.md`; update that guide only if commands or expected behavior differ.

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No feature-code prerequisites; complete in task order because dependency/configuration files overlap.
- **Foundational (Phase 2)**: Depends on Setup; provides shared theme and metadata.
- **User Story 1 (Phase 3)**: Depends on Setup and Foundational; delivers the MVP.
- **User Story 2 (Phase 4)**: Depends on the shared shell and the login link delivered by US1 (T010).
- **User Story 3 (Phase 5)**: Depends on the shared shell and the login link delivered by US1 (T010).
- **Polish (Phase 6)**: Depends on all three stories being implemented.

### User Story Dependencies

- **US1 (P1)**: Starts after T001-T005; no dependency on another story.
- **US2 (P2)**: Starts after T001-T005 and T010, so its route is reachable from the finished login page.
- **US3 (P3)**: Starts after T001-T005 and T010, so its route is reachable from the finished login page.
- **US2 and US3**: Independent of each other and can be implemented in parallel.

### Within-Story Dependencies

- T006 is an independent test file and can be authored in parallel with T008 after setup.
- T007 depends on test expectations in T006; T009 depends on T007 and T008.
- T010 depends on T009.
- T011 and T012 each depend on T010 and can then run in parallel.
- T013 and T014 run after the story routes are present.

### Parallel Opportunities

- After T001-T005, T006 and T008 can be developed in parallel.
- After T010, T011 and T012 can be developed in parallel.
- T013 and T014 are final polish checks and should be completed before declaring the feature done.

## Parallel Example: User Story 1

```text
Task: T006 Add pure validation unit cases in tests/unit/login-validation.test.ts
Task: T008 Add email-only browser storage helpers in lib/remembered-email.ts
```

## Parallel Example: User Stories 2 and 3

```text
Task: T011 Add demo recovery page in app/forgot-password/page.tsx
Task: T012 Add demo registration page in app/register/page.tsx
```

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Setup and Foundational tasks (T001-T005).
2. Implement and verify User Story 1 (T006-T010).
3. Stop at the Phase 3 checkpoint: validate successful demo sign-in, all field
   failures, Remember Me storage rules, password visibility, keyboard use, and
   responsive layout.
4. Deliver US1 as the minimum useful increment; add US2 and US3 as follow-on stories.

### Incremental Delivery

1. Add the independent recovery route (US2) and registration route (US3) after US1.
2. Run the quickstart's unit, lint, build, and manual checks across all routes.
3. Do not add integration or end-to-end test suites.

## Notes

- Every task uses the required checkbox, sequential ID, optional parallel marker,
  story label where applicable, and concrete file path(s).
- `[P]` marks tasks that touch separate files and have no unmet prerequisite.
- No integration-test or E2E-test tasks are included.
