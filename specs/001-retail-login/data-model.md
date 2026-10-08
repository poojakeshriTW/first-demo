# Data Model: Retail Login

The feature has no server-side records. All models below are in-memory UI state or
the single explicitly permitted browser preference.

## Login Form

| Field | Type | Required | Validation / behavior |
|---|---|---:|---|
| `email` | string | Yes | Trim leading/trailing whitespace before format validation and simulated credential comparison. |
| `password` | string | Yes | Compare exactly with the documented synthetic demo password; never persist it. |
| `dateOfBirth` | `yyyy-MM-dd` string | Yes | Must parse as a valid calendar date and be no later than today's local date. Do not persist it. |
| `rememberEmail` | boolean | No | If true after successful demo sign-in, persist only the normalized email. |
| `showPassword` | boolean | No | Presentation state only; toggling does not change the password value. |

### Validation errors

Each field may have one actionable validation message at a time:

- Email: required; otherwise valid email format after trimming.
- Password: required.
- Date of Birth: required; otherwise valid date; future dates are prohibited.
- Simulated credentials: non-sensitive message when the email/password pair does not
  match the documented demo pair. A valid DOB is required to submit, but is not
  matched against an account.

Errors clear or update when the form is corrected and resubmitted. An invalid form
does not enter the signed-in demo state.

## Remembered Login Preference

| Property | Type | Storage |
|---|---|---|
| `email` | string | One namespaced `localStorage` entry |

Only create/update this preference after successful simulated sign-in with Remember
Me selected. Remove it when the shopper clears Remember Me. Loading the preference
prefills only the email and reflects the remembered choice. Password, DOB, and
signed-in state are never stored.

If browser storage cannot be read or written, show clear feedback that Remember Me
could not be applied; do not report a successful persistence operation.

## Simulated Sign-In State

Transient state values:

- `idle`: form is ready for input.
- `invalid`: one or more field or credential errors are displayed.
- `signed-in-demo`: entered email/password match the published demo pair and the
  required DOB is valid; display a clear simulation notice.

Reloading or opening a later visit always starts outside `signed-in-demo`.

## Demo-Only Screens

Recovery and registration screens contain explanatory notices and navigation back to
login. They collect or submit no real account data and do not create persistent
records.
