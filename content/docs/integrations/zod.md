---
title: "Zod"
weight: 4
---

`response.schema` accepts a Zod schema directly — Spectest detects it via `.safeParse` and validates the response body against it, no wrapper object required. If your server already validates requests or responses with Zod, you can reuse those exact schemas in your Spectest suites instead of hand-writing a second set of assertions.

## Setup

```bash
npm install --save-dev spectest zod
```

```js
// users.spectest.js
import { z } from 'zod';

export default [
  {
    name: 'Get user',
    endpoint: '/users/1',
    response: {
      status: 200,
      schema: z.object({
        id: z.string().uuid(),
        email: z.string().email(),
      }),
    },
  },
];
```

A schema mismatch fails the test with Zod's own error formatting (field paths and violated checks), same as a `safeParse` failure anywhere else in your code.

## Sharing schemas with your server

If your API layer already exports Zod schemas — for request validation, or as the source of truth for generating an OpenAPI document — import them into your suite instead of duplicating the shape:

```js
// users.spectest.js
import { UserSchema } from '../src/schemas/user.js';

export default [
  {
    name: 'Get user',
    endpoint: '/users/1',
    response: { status: 200, schema: UserSchema },
  },
];
```

This keeps the contract test and the runtime validator from drifting apart — a schema change that breaks the API automatically breaks the test.

## Zod vs. raw JSON Schema

`response.schema` also accepts a raw JSON Schema or OpenAPI 3.0/3.1 Schema Object directly (validated with a shared Ajv2020 instance); see [Test Case](/docs/api-references/test-case/#validating-with-responseschema) for that form. Use Zod when your project already has Zod schemas to reuse; use a JSON Schema object when you're validating straight against an OpenAPI document's `components.schemas` entries via [OpenAPI Testing](/docs/guides/openapi-testing/).

## Troubleshooting

- **Schema silently not applied** — Spectest treats anything with a `.safeParse` function as a Zod schema. A plain object that happens to define `safeParse` will be misdetected; keep custom validators separate from `response.schema`.
- **OpenAPI-specific keywords in a Zod-generated schema** — if you convert an OpenAPI schema to Zod (e.g. via a codegen tool), the OpenAPI-only-keyword normalization Spectest applies to raw JSON Schema does not apply to Zod schemas, since Zod already fully describes the shape itself.

Related: [Test Case](/docs/api-references/test-case/) for the full `response.schema` reference, [OpenAPI](/docs/integrations/openapi/) for spec-driven testing.
