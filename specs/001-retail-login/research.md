# Research: Retail Login

## Decisions

### Next.js App Router and client boundary

- **Decision**: Keep route shells as App Router pages and implement the interactive
  login form as a small client component. Access `localStorage` only after mount;
  compute the DOB maximum in the browser from the user's local date.
- **Rationale**: The existing application already uses the App Router. Browser
  storage and form interactivity require client-side code. Client components may
  still be prerendered, so browser-only APIs must not be accessed during render or
  in a state initializer. Computing today's bound in the browser avoids timezone
  differences between server and shopper.
- **Alternatives considered**: Making the full application client-rendered (larger
  client boundary) or adding an API/backend (unneeded for a clearly simulated flow).
- **Evidence**: Existing `app/page.tsx` and `app/layout.tsx`; Next.js App Router
  [Server and Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components);
  React [`useEffect`](https://react.dev/reference/react/useEffect).

### Tailwind CSS v4 theme colors

- **Decision**: Preserve the existing Tailwind import and define semantic colors as
  CSS variables in the existing root/theme selectors, mapping them through
  top-level Tailwind v4 `@theme inline` `--color-*` variables; consume the generated
  utilities in components.
- **Rationale**: Tailwind theme variables generate the corresponding color utilities
  and match the installed Tailwind v4 stack. Keep non-utility browser-specific values
  separate only when necessary.
- **Alternatives considered**: Scattered hard-coded colors or a separate theme
  library; both add duplication or dependencies.
- **Evidence**: Existing `app/globals.css`; [Tailwind CSS theme variables](https://tailwindcss.com/docs/theme).

### shadcn/ui components

- **Decision**: Use the shadcn/ui Next.js setup/CLI as a component-code generator,
  review its changes to `components.json`, CSS, aliases, and dependencies, and
  generate only the components used by this feature. Use a native HTML date input
  styled through the same design system.
- **Rationale**: shadcn/ui provides project-owned accessible components that can be
  styled with the requested Tailwind theme without adding a separate runtime
  component framework. Generated component prerequisites are added only as needed.
  The native date input avoids a calendar dependency and exposes `max` for date
  selection bounds.
- **Alternatives considered**: Hand-building every primitive (more accessibility
  and consistency work) or adding a separate date-picker library (unneeded).
- **Evidence**: [shadcn/ui Next.js installation guide](https://ui.shadcn.com/docs/installation/next),
  [components.json](https://ui.shadcn.com/docs/components-json), and
  [theming](https://ui.shadcn.com/docs/theming).

### Date input and date-fns

- **Decision**: Keep the DOB value as the date input's canonical `yyyy-MM-dd`
  string. Use date-fns `parseISO` and `isValid` for explicit date parsing checks and
  `format` for human-readable display; compute the `max` value as a local calendar
  date in the browser. Independently reject invalid and future values at submit.
- **Rationale**: Native date inputs use a stable date-only value. Separating the
  calendar date from locale display prevents parsing display-formatted strings.
  `isValid` alone does not reject future dates. Avoid `new Date(dateString)` or
  `toISOString()` conversions that can shift a date-only value across timezones;
  date-fns tokens use Unicode forms such as `yyyy-MM-dd`.
- **Alternatives considered**: A custom calendar/date picker (more code and a new
  dependency) or locale-dependent string parsing (ambiguous).
- **Evidence**: [date-fns `parseISO`](https://github.com/date-fns/date-fns/blob/main/pkgs/core/src/parseISO/index.ts),
  [`isValid`](https://github.com/date-fns/date-fns/blob/main/pkgs/core/src/isValid/index.ts),
  [`format`](https://github.com/date-fns/date-fns/blob/main/pkgs/core/src/format/index.ts);
  [MDN date input](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input/date).

### Remember Me persistence

- **Decision**: Use one versioned, namespaced localStorage key for the remembered
  email. Read it after mount; write only after an explicit successful simulated
  sign-in with Remember Me checked; remove it when the preference is cleared. Never
  persist the password, DOB, or signed-in state.
- **Rationale**: This implements FR-003 while minimizing retained personal data.
  `localStorage` persists across browser sessions but is browser-only and can be
  unavailable or throw, so errors must be surfaced explicitly. The email is personal
  data, not a secret.
- **Alternatives considered**: Cookies/server sessions (there is no backend),
  sessionStorage (does not meet the later-visit requirement), or persisting the
  whole form (violates the spec).
- **Evidence**: [MDN: `Window.localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage).

### Testing scope

- **Decision**: Add focused unit tests only for pure validation/date helpers, plus
  the existing lint/build checks and manual UI review. Do not add integration or
  end-to-end tests.
- **Rationale**: The project has no test script or test runner. A small unit-only
  test setup covers high-value DOB and email edge cases without introducing a
  browser or application integration harness.
- **Alternatives considered**: No automated tests (less reliable validation) or
  integration/E2E suites (explicitly out of scope).

### Demo credentials

- **Decision**: Centralize a documented synthetic demo-only email/password pair in
  a local constant and display it to shoppers or testers. Since the specification
  does not prescribe literal values, use `retail-demo@example.test` and
  `demo-only-password`; DOB is validated but is not an authentication credential.
- **Rationale**: The values are deterministic, clearly synthetic, and use the
  reserved `.test` domain. No real account service is contacted.
- **Alternatives considered**: Requiring an external auth service (contradicts the
  clarified scope) or leaving the demo flow without documented valid credentials
  (fails FR-012).
