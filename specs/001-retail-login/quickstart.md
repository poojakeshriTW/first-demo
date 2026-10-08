# Quickstart: Retail Login

## Prerequisites

- Node.js and npm compatible with the existing Next.js project.
- Dependencies installed from the repository root with `npm install`.

## Run the application

```sh
npm run dev
```

Open the local URL printed by Next.js and exercise the login and demo-only screens.
The UI displays synthetic demo credentials; no real account service is contacted.

## Focused validation

Run the available static checks:

```sh
npm run lint
npm run build
```

Run the focused pure validation/date unit tests once added:

```sh
npm run test:unit
```

No integration-test or end-to-end-test command or suite is part of this feature.

## Manual acceptance checks

1. Submit the published synthetic email/password with a valid DOB no later than
   today; confirm the clearly labeled signed-in demo state appears.
2. Omit each mandatory field in turn; confirm a clear, field-specific message
   appears and the demo does not sign in.
3. Enter a malformed email, an invalid date, and a future DOB; confirm each is
   rejected. Confirm the native date control disallows future-date selection.
4. Toggle password visibility before and after typing; confirm the value is
   unchanged.
5. Sign in with Remember Me selected, reload, and confirm only the email is
   prefilled. Confirm password, DOB, and signed-in state are not restored. Clear
   Remember Me and confirm the remembered email is removed.
6. Use Forgot Password? and Sign Up / Register; confirm the screens clearly state
   that real recovery/registration is unavailable and return navigation works.
7. Navigate all controls by keyboard, inspect field error associations, and review
   the layout at 320 px, mobile, and desktop viewport widths.

For entity and validation details, see [data-model.md](./data-model.md); for the
user-visible interaction contract, see [contracts/login-ui.md](./contracts/login-ui.md).
