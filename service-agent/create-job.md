BaseUrl: `https://egb17p3twf.execute-api.eu-west-1.amazonaws.com/dev`

### Create Job

**Endpoint**: `POST /create-job`

#### Payload Overview
| Field | Type | Required | Validation |
| --- | --- | --- | --- |
| `jobNo` | string | ✔ | Trimmed. Missing triggers 400. |
| `companyId` | string | ✖ | Trimmed if present. |
| `companyName` | string | ✖ | Trimmed if present. |
| `customers` | `Customer[]` | ✔ | Must exist and be non-empty. `phone` normalized; `name` trimmed. |
| `product.id` | string | ✔ | Missing triggers 400. |
| `product.serialNumber` | string | ✖ | Trimmed; defaults to `tempSerialNumber` if absent. Fails on `validateSerialNumber` error. |
| `engineers` | `Engineer[]` | ✖ | Used to decide status. For each engineer, `email` looked up. |
| `serviceRequestDate` | ISO date-time string | ✔ | No explicit parse check, but expected ISO string. |
| `installationDate` | ISO date string | ✖ | Must parse via `Date.parse`; otherwise 400. Warns if absent. |
| `warrantyExpiryDate` | ISO date string | ✖ | Must parse via `Date.parse`; otherwise 400. Warns if absent. |
| `estimatedSymptom` | string | ✖ | Trimmed. |
| `estimatedSymptomDetail` | string | ✖ | Trimmed. |
| `customerComment` | string | ✖ | Trimmed. |

#### Validation Flow
1. JSON body must parse; else 400 `Invalid JSON body`.
2. Checks for missing `jobNo`, `product.id`, `serviceRequestDate`; missing → 400 with combined message.
3. Serial number validated by `validateSerialNumber`; failure → aggregates 400.
4. `customers` array must exist and have entries; else 400.
5. `installationDate`, `warrantyExpiryDate` must be parseable; invalid → 400.
6. On validation failure, request logged and stored via `insertFailedJob`, response 400.

#### Processing
- Builds `Job` object with timestamps (`creationDateTime` ISO string).
- Saves job via `saveJob`; failure returns propagated error.
- For each engineer, fetches user (`getUser`); sends in-app notification on success.
- Sends customer SMS, engineer email, and calendar notification when status is `ACCEPTED`.
- Job status logic:
  - 1 engineer → `ACCEPTED`
  - >1 engineer → `PENDING`
  - 0 engineers → `OPEN`

#### Responses
- Success: `201 Created`, body from `saveJob`.
- Errors: Relevant HTTP status with message from validation or persistence layer.
