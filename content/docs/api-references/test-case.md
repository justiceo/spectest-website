---
title: "Test Case"
weight: 3
---

A **test case** describes a single HTTP operation. Only `name` and `endpoint` are required. All other fields are optional and mirror the [`fetch` Request](https://developer.mozilla.org/en-US/docs/Web/API/Request) and [`Response`](https://developer.mozilla.org/en-US/docs/Web/API/Response) objects.

### Properties

| Key | Description | Default |
| --- | ----------- | ------- |
| `name` | Human readable test name | required |
| `operationId` | Unique identifier for the operation | `name` |
| `phase` | Execution phase (`setup`, `main`, or `teardown`) | `main` |
| `dependsOn` | Array of operationId strings that must pass first | none |
| `endpoint` | Request path relative to the base URL | required |
| `request.method` | HTTP method | `GET` |
| `request.headers` | Additional request headers | none |
| `request.body` | Request payload | none |
| `request.*` | Any other valid fetch Request option | none |
| `response.status` | Expected HTTP status | `200` |
| `response.json` | Expected partial JSON body | none |
| `response.schema` | Zod or JSON schema to validate response | none |
| `response.headers` | Expected response headers | none |
| `response.*` | Other fetch Response fields | none |
| `beforeSend(req, state)` | Function to finalize the request | none |
| `postTest(res, state, ctx)` | Function called after the response | none |
| `tags` | Tags used with `--tags` filtering | none |
| `skip` | Skip this test | `false` |
| `focus` | Run only focused tests when present | `false` |
| `repeat` | Extra sequential runs of the test | `0` |
| `bombard` | Additional simultaneous runs of the test | `0` |
| `delay` | Milliseconds to wait before running | none |
| `timeout` | Per‑test timeout override | runtime `timeout` (`60000`ms) |
| `recording` | Per‑test HTTP recording mode override (`off`, `replay`, or `record`) | runtime `recording` |

### Example

```js
{
  name: 'Create a post',
  endpoint: '/posts',
  request: {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: { title: 'foo', body: 'bar', userId: 1 }
  },
  response: {
    status: 201,
    json: { id: 101, title: 'foo', body: 'bar', userId: 1 }
  }
}
```

If the server returns `201` with a matching body the case passes. Any mismatched status or body value results in a failure.

### Phases

Tests run in three waves: `setup` → `main` → `teardown`. `phase` is syntactic sugar for `dependsOn` — every `setup` test implicitly blocks every `main` test, and every `main` test implicitly blocks every `teardown` test, without listing operationIds by hand. Within a phase, tests still run concurrently as soon as their explicit `dependsOn` prerequisites pass.

See [Helpers](/docs/introduction/helpers/) for utilities that modify multiple cases at once.
