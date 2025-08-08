---
title: "Testing HTTP APIs"
weight: 3
---

This guide explains how Spectest interacts with your API and how responses are asserted.

## Requests

Every test sends an HTTP request using Axios under the hood. The `request` property mirrors the [`fetch` Request](https://developer.mozilla.org/en-US/docs/Web/API/Request) options. Common fields include `method`, `headers` and `body`.

```js
{
  endpoint: '/login',
  request: {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: { username: 'admin', password: 'secret' },
  }
}
```

## Responses

By default Spectest expects a `200` status code. You can assert specific values or validate against a Zod schema.

```js
import { z } from 'zod';

{
  name: 'Create a post',
  endpoint: '/posts',
  request: { method: 'POST', body: { title: 'foo', body: 'bar', userId: 1 } },
  response: {
    status: 201,
    schema: z.object({
      id: z.number(),
      title: z.string(),
      body: z.literal('foo'),
      userId: z.number().min(1)
    })
  }
}
```

{{< hint tip >}}
Combine `json` and `schema` assertions to verify both shape and exact values.
{{< /hint >}}

## Timeouts and retries

Use the `timeout` property to limit how long a request may run. Tests exceeding the limit fail with a ⏰ indicator.
