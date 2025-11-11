### Delete User (POST)

BaseUrl

- Prod: `https://egb17p3twf.execute-api.eu-west-1.amazonaws.com/dev`
- Test: `https://pawwf1i0m7.execute-api.eu-west-1.amazonaws.com/test`


**Endpoint**: `POST /users/{email}/delete`

#### Path Parameters
| Parameter | Type | Required | Validation |
| --- | --- | --- | --- |
| `email` | string | ✔ | Trimmed. Missing triggers 400. |

#### Validation Flow
1. Path parameter `email` must be present; missing → 400 `email is required`.
2. Email value is trimmed before processing.

#### Processing
- Extracts email from path parameters and trims whitespace.
- Calls `removeUser` service with the email.
- Service handles user deletion logic; failure returns propagated error with appropriate status code.
- *Note: This endpoint provides the same functionality as `DELETE /users/{email}` but uses POST method for compatibility with clients that don't support DELETE.*

#### Responses
- Success: Status code from service (typically `200 OK` or `204 No Content`)
  ```json
  {}
  ```
  Empty object returned on successful deletion.

- Errors:
  - `400 Bad Request`: Missing email path parameter
  - Other status codes: Propagated from service layer (e.g., `404 Not Found` if user doesn't exist, `500 Internal Server Error` for deletion failures)
