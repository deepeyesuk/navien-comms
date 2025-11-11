### Cancel Job

BaseUrl

- Prod: `https://egb17p3twf.execute-api.eu-west-1.amazonaws.com/dev`
- Test: `https://pawwf1i0m7.execute-api.eu-west-1.amazonaws.com/test`


**Endpoint**: `POST /jobs/{jobNo}/cancel`

#### Path Parameters
| Parameter | Type | Required | Validation |
| --- | --- | --- | --- |
| `jobNo` | string | ✔ | Trimmed. Missing triggers 400. |

#### Validation Flow
1. Path parameter `jobNo` must be present; missing → 400 `jobNo is required`.
2. Job number value is trimmed before processing.

#### Processing
- Extracts job number from path parameters and trims whitespace.
- Calls `cancelJobService` with the job number.
- Service handles job cancellation logic (likely updates job status and related metadata); failure returns propagated error with appropriate status code.

#### Responses
- Success: Status code from service (typically `200 OK`)
  ```json
  {
    // Job data returned from service
  }
  ```
  Returns the updated job data after cancellation.

- Errors:
  - `400 Bad Request`: Missing jobNo path parameter
  - Other status codes: Propagated from service layer (e.g., `404 Not Found` if job doesn't exist, `500 Internal Server Error` for cancellation failures)
