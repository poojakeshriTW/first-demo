# Implementation Plan: Retail Login

**Branch**: `001-retail-login` | **Date**: 2026-10-08 | **Spec**: [spec.md](./spec.md)

**Input**: Feature specification from `/specs/001-retail-login/spec.md`

## Summary

Build the retail demo login flow with required email, password, and date-of-birth
fields; field-level validation; password visibility; Remember Me; and demo-only
recovery, registration, and success states. Use the existing Next.js App Router,
Tailwind CSS v4 `@theme` color tokens, generated shadcn/ui components, and date-fns
for date-only formatting and validation. Remember Me stores only the email address
in browser localStorage. Authentication remains a local simulation with no real
account service.

## Technical Context

**Language/Version**: TypeScript 5, strict mode; React 19.3.0; Next.js 16.4.0

**Primary Dependencies**: Existing Next.js, React, and Tailwind CSS 4; add date-fns
for date operations, shadcn/ui-generated components for consistent accessible UI,
and Vitest for focused unit tests of pure validation helpers. Add only dependencies
required by the selected generated shadcn components.

**Storage**: A versioned, namespaced browser `localStorage` entry for the Remember
Me email preference only.
Passwords, dates of birth, demo credential constants, and signed-in state are never
persisted.

**Testing**: Focused unit tests for pure validation/date helpers with Vitest;
`npm run lint` and `npm run build`; manual responsive, keyboard, and acceptance
checks. Do not add integration or end-to-end tests.

**Target Platform**: Modern desktop and mobile browsers; Next.js App Router.

**Project Type**: Single-project web application.

**Performance Goals**: Keep the form client-side interaction lightweight; no
network requests are required for sign-in or demo navigation.

**Constraints**: Preserve the declared Next.js 16.4.0, React 19.3.0, and Tailwind
CSS `^4` versions. No real authentication, recovery, or registration service.
Native DOB date input must cap selection at the current local date, computed on the
client to avoid server/client timezone mismatch, and independently reject invalid or
future values on submit. Storage access is browser-only and must occur after mount;
any storage failure must be surfaced to the shopper rather than silently treated as
a successful Remember Me operation.

**Scale/Scope**: One login flow with demo-only recovery and registration screens;
no backend, external API, real account, or persistent session.

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

- **I. Clean, Maintainable Code — PASS**: Keep the form, pure validation, and
  remembered-email storage concerns separate; do not add backend abstractions.
- **II. Simple, Accessible User Experience — PASS**: Use labeled semantic controls,
  keyboard-operable shadcn/ui primitives, actionable field errors, and clear
  simulated-flow notices.
- **III. Responsive by Default — PASS**: Use a mobile-first layout and verify at
  narrow mobile and desktop sizes, including the 320 px minimum in the spec.
- **IV. Minimal Dependencies — PASS WITH JUSTIFICATION**: date-fns, shadcn-generated
  component prerequisites, and Vitest are limited to the requested date formatting,
  requested UI components, and focused unit coverage where no test runner currently
  exists. Do not add a calendar library or browser automation framework.
- **V. Approved Framework and Version Alignment — PASS**: Retain the manifest's
  Next.js, React, and Tailwind CSS versions; use Tailwind CSS v4 `@theme`.

**Gate result**: No constitution violation. Requested libraries are scoped to
specific feature needs; no real-auth integration or automated integration/E2E
suite is introduced.

## Project Structure

### Documentation (this feature)

```text
specs/001-retail-login/
├── plan.md
├── research.md
├── data-model.md
├── quickstart.md
└── contracts/
    └── login-ui.md
```

### Source Code (repository root)

```text
app/
├── page.tsx                         # Login route shell
├── forgot-password/page.tsx         # Demo-only recovery notice
├── register/page.tsx                # Demo-only registration notice
├── layout.tsx                       # Application metadata and shared shell
└── globals.css                      # Tailwind v4 @theme color tokens
components/
├── login/
│   └── login-form.tsx                # Client form and simulated sign-in state
└── ui/                               # shadcn/ui-generated primitives in use
lib/
├── login-validation.ts              # Pure field validation
├── remembered-email.ts              # Email-only localStorage operations
└── utils.ts                          # shadcn/ui class-name utility
tests/
└── unit/
    └── login-validation.test.ts      # Pure unit tests only
components.json                       # shadcn/ui configuration
```

**Structure Decision**: Use the existing single Next.js App Router project. Keep
recovery and registration as simple static routes, the interactive login form in a
client component, and browser persistence and validation in focused `lib/` modules.
No API routes or backend project are needed.

## Phase 0: Research

Research choices and compatibility notes are recorded in [research.md](./research.md).
The user-selected technologies are fixed; no unresolved technology choice blocks
design.

## Phase 1: Design

- [Data model](./data-model.md) specifies transient form state, the remembered
  email-only preference, validation rules, and simulated sign-in state.
- [UI contract](./contracts/login-ui.md) defines user-visible interactions and
  their validation/navigation outcomes; there is no external API contract.
- [Quickstart](./quickstart.md) provides setup and focused manual/build validation
  steps. It intentionally defines no integration or end-to-end test suite.

## Post-Design Constitution Check

- **I. Clean, Maintainable Code — PASS**: The design separates validation, browser
  storage, and presentation without unnecessary application layers.
- **II. Simple, Accessible User Experience — PASS**: All form controls are labeled,
  errors are associated with their fields, and the demo nature is explicit.
- **III. Responsive by Default — PASS**: The plan includes viewport-specific
  verification and avoids fixed-width form layouts.
- **IV. Minimal Dependencies — PASS WITH JUSTIFICATION**: The design uses date-fns,
  only the shadcn component prerequisites actually needed, and a unit-only runner;
  it adds no calendar, integration, or browser automation framework.
- **V. Approved Framework and Version Alignment — PASS**: No required framework
  version changes are planned.

**Final gate result**: PASS. No complexity-tracking exception is needed.
