### Create User

BaseUrl

- Prod: `https://egb17p3twf.execute-api.eu-west-1.amazonaws.com/dev`
- Test: `https://pawwf1i0m7.execute-api.eu-west-1.amazonaws.com/test`


**Endpoint**: `POST /users`

#### Payload Overview
| Field | Type | Required | Validation |
| --- | --- | --- | --- |
| `firstname` | string | ✔ | Trimmed. Missing triggers 400. |
| `lastname` | string | ✔ | Trimmed. Missing triggers 400. |
| `email` | string | ✔ | Trimmed and lowercased. Must match email regex pattern. Missing triggers 400. |
| `companyId` | string | ✔ | Trimmed. Missing triggers 400. |
| `companyName` | string | ✔ | Trimmed. Missing triggers 400. |
| `gasSafetyNumber` | string | ✖ | Trimmed if present. Warning logged if both this and `oftecNumber` are missing. |
| `oftecNumber` | string | ✖ | Trimmed if present. Warning logged if both this and `gasSafetyNumber` are missing. |

#### Validation Flow
1. JSON body must parse; else 400 `Invalid JSON body`.
2. Checks for missing required fields (`firstname`, `lastname`, `email`, `companyId`, `companyName`); missing → 400 with combined message.
3. Email must match regex pattern `/^[^\s@]+@[^\s@]+\.[^\s@]+$/`; invalid → 400 `Invalid email address: {email}`.
4. Logs warning if neither `gasSafetyNumber` nor `oftecNumber` is provided (no certificate data).

#### Processing
- Builds `User` object with:
  - Trimmed and normalized values (email lowercased)
  - Status set to `CREATED`
  - Timestamp (`createdAt` ISO string)
- Saves user via `createUserService`; failure returns propagated error with appropriate status code.
- Sends welcome email to user with:
  - CC to configured support email
  - Links to Google Play and App Store for app download
  - Instructions for signing up and accessing the app
  - Email failure logged but doesn't block user creation

#### Responses
- Success: `201 Created`
  ```json
  {
    "companyId": "string",
    "companyName": "string",
    "firstname": "string",
    "lastname": "string",
    "email": "string",
    "gasSafetyNumber": "string (optional)",
    "oftecNumber": "string (optional)",
    "status": "CREATED",
    "createdAt": "ISO date-time string"
  }
  ```
  Body includes message: `User created`

- Errors:
  - `400 Bad Request`: Invalid JSON, missing required fields, or invalid email format
  - `500 Internal Server Error`: User creation failure (or specific error code from service layer)
