<!--
Sync Impact Report
Version change: Unversioned scaffold → 1.0.0
Modified principles: None; the scaffold's five principle placeholders are now defined as:
  I. Clean, Maintainable Code
  II. Simple, Accessible User Experience
  III. Responsive by Default
  IV. Minimal Dependencies
  V. Approved Framework and Version Alignment
Added sections: Technical Constraints; Delivery Standards
Removed sections: None
Follow-up TODO: Confirm the original ratification date.
-->

# Login Demo App Constitution

## Core Principles

### I. Clean, Maintainable Code
Code MUST be clear, consistently formatted, and organized around focused components,
functions, and modules. Names and types MUST communicate intent. Repeated logic MUST be
consolidated when doing so makes behavior easier to understand and maintain. Changes MUST
avoid unrelated complexity and preserve existing behavior unless a change is intentional.
These rules reduce defects and keep future changes straightforward.

### II. Simple, Accessible User Experience
User-facing flows MUST make the primary action and system state clear, with concise content
and predictable interactions. Interfaces MUST support keyboard operation and use semantic
markup and accessible labels for interactive controls. Validation and failure states MUST
give users actionable feedback. Prefer the simplest interaction that fully serves the
feature.

### III. Responsive by Default
Every user-facing page and component MUST remain usable across supported mobile, tablet, and
desktop viewport sizes. Layouts MUST adapt without unintended horizontal scrolling, clipped
content, or unusably small interactive controls. Responsive behavior MUST be considered and
verified when UI changes are made.

### IV. Minimal Dependencies
New runtime or development dependencies MUST be avoided when the existing platform and
project stack can meet the requirement clearly and reliably. Each proposed dependency MUST
have a documented purpose and be reviewed for maintenance, compatibility, and size impact
before adoption. This keeps the application smaller, safer, and easier to maintain.

### V. Approved Framework and Version Alignment
The application MUST use Next.js, React, and Tailwind CSS. Their required versions MUST
remain aligned with the declarations in the root `package.json`: Next.js `16.4.0`, React
and React DOM `19.3.0`, and Tailwind CSS `^4`. The package manifest is the source of truth;
version changes MUST be made there through an intentional dependency update, not by adding
conflicting version declarations elsewhere.

## Technical Constraints

- The required application stack is Next.js `16.4.0`, React and React DOM `19.3.0`, and
  Tailwind CSS `^4`, as declared in `package.json`.
- Changes MUST use the existing stack and established project conventions before introducing
  another framework, styling system, or dependency.
- Any intentional change to a required framework version MUST update the package manifest
  and be reviewed for compatibility with the application.

## Delivery Standards

- Feature work MUST include focused verification appropriate to the behavior changed, using
  the project's existing lint, build, and test tooling where applicable.
- Reviews MUST check the change against these principles, including responsive behavior,
  accessibility, dependency impact, and alignment with the package manifest.
- Documentation MUST be updated when a change affects setup, usage, architecture, or
  development expectations.

## Governance

This constitution governs project design and implementation decisions. Amendments MUST be
made in this document and MUST include a version update, an updated last-amended date, and a
clear impact report describing changed principles or sections. A proposed amendment MUST
state its rationale and be reviewed for compatibility with existing project guidance before
adoption.

Constitution versions follow semantic versioning: MAJOR for incompatible governance changes
or removal/redefinition of a principle, MINOR for adding a principle or materially expanding
governance, and PATCH for clarifications and non-semantic wording changes. Every
implementation plan and code review MUST check applicable work against this constitution.
Any exception MUST be explicit, justified, and approved as part of review.

**Version**: 1.0.0 | **Ratified**: TODO(YYYY-MM-DD): original ratification date not established | **Last Amended**: 2026-10-07
