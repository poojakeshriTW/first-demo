# Login UI Contract

This feature exposes a browser UI only. It defines no API endpoints, network
contracts, or external service integrations.

## Login screen

### Controls

- Labeled Email Address text input.
- Labeled Password input with a keyboard-operable Show Password / Hide Password
  button that preserves the current value.
- Labeled Date of Birth native date input. Its maximum selectable value is today's
  local date; submission validation also rejects a future or invalid date.
- Remember Me checkbox.
- Login submit button.
- Forgot Password? and Sign Up / Register navigation links.
- Visible, actionable validation feedback associated with each invalid field.
- A clearly identified demo-credential notice for shoppers/testers.

### Submit behavior

1. Trim the email value and validate required fields, email format, and DOB.
2. If invalid, show field-specific messages and do not evaluate the simulated login.
3. If the email/password pair does not match the published demo pair, show
   non-sensitive failure feedback; do not imply that a real account was checked.
4. Otherwise show the signed-in demo state. If Remember Me is checked, retain only
   the email; if not checked, remove any saved email.
5. Never store the password, DOB, or signed-in state.

## Recovery and registration

- Activating Forgot Password? opens the demo-only recovery screen and states that no
  real recovery request is processed.
- Activating Sign Up / Register opens the demo-only registration screen and states
  that no real account is created.
- Both screens provide a keyboard-accessible path back to the login screen.

## Accessibility and layout

- All inputs have visible, programmatically associated labels.
- Field errors are associated with their input and announced accessibly.
- All actions and links are keyboard operable with visible focus treatment.
- Content remains usable at 320 px and wider without unintended horizontal
  scrolling or clipped controls/messages.
