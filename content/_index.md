---
description: "Spectest is a lightning-fast, declarative HTTP API testing framework. Write tests in JS, JSON, or YAML, run them from the CLI or CI, test OpenAPI specs natively, and record & replay outbound calls — no boilerplate."
toc: false
---

{{< hero
  badge="Open Source · MIT Licensed · Battle-tested"
  title="API testing without the pain"
  tagline="Spectest runs declarative HTTP tests straight from the CLI. Describe the request and the response you expect — no clients, no boilerplate, no ceremony. Ship confident APIs faster."
  cta_text="Get Started"
  cta_url="/docs/introduction/getting-started/"
  cta_secondary_text="View on GitHub"
  cta_secondary_url="https://github.com/justiceo/spectest"
>}}
```js
export default [
  {
    name: "Create User",
    endpoint: "/api/users",
    request: {
      method: "POST",
      body: { name: "John", email: "john@example.com" },
    },
    response: {
      status: 201,
      json: { name: "John", email: "john@example.com" },
    },
  },
];
```
{{< /hero >}}

{{< logos title="Trusted by fast-moving engineering teams" >}}

{{< stats >}}

{{< section-head
  eyebrow="Why Spectest"
  title="Everything you need to test APIs, none of the boilerplate"
  subtitle="A single, focused tool for HTTP API testing — declarative by design, fast by default, and built to live in your CI pipeline." >}}

{{< cards >}}
{{< card title="Blazing Fast Execution" icon="lightning-bolt" subtitle="Run hundreds of assertions in seconds with parallel execution and near-zero startup cost." >}}
{{< card title="Truly Declarative" icon="document-text" subtitle="Describe requests and expected responses in JS, JSON, or YAML. No clients, no glue code." >}}
{{< card title="OpenAPI-Native" icon="code" link="/docs/guides/openapi-testing/" subtitle="Execute OpenAPI 3.0/3.1 operations directly — your spec becomes your test suite." >}}
{{< card title="Record & Replay" icon="save" link="/docs/guides/http-recording/" subtitle="Capture outbound HTTP calls and replay them deterministically. Kill flaky tests for good." >}}
{{< card title="CI/CD Ready" icon="terminal" subtitle="A first-class CLI with rich reporting that drops straight into GitHub Actions, GitLab, or any pipeline." >}}
{{< card title="Schema Assertions" icon="shield-check" link="/docs/integrations/zod/" subtitle="Validate responses against Zod schemas, regex, snapshots, and deep JSON matchers." >}}
{{< /cards >}}

{{< section-head
  eyebrow="See it in action"
  title="Radically less code than traditional API tests"
  subtitle="The same two test cases — a create and a duplicate-rejection — written the old way versus the Spectest way." >}}

<div class="st-compare-grid">

<div class="st-compare-col">

<span class="st-compare-label">Traditional Testing</span>

<div class="st-codecard">
<div class="st-codecard-tabs"><span class="st-codecard-tab">user.test.js</span></div>

```javascript
describe('User API', () => {
  it('creates a user', async () => {
    const res = await fetch('/api/users', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ name: 'John', email: 'john@example.com' })
    });

    expect(res.status).toBe(201);
    expect(res.headers.get('content-type')).toContain('application/json');

    const user = await res.json();
    expect(user).toHaveProperty('id');
    expect(user.name).toBe('John');
    expect(user.email).toBe('john@example.com');
  });

  it('rejects a duplicate email', async () => {
    const res = await fetch('/api/users', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ name: 'Jane', email: 'john@example.com' })
    });

    expect(res.status).toBe(409);
    const body = await res.json();
    expect(body.error).toBe('email already exists');
  });
});
```

</div>

</div>

<div class="st-compare-col">

<span class="st-compare-label st-compare-label--highlight">Spectest</span>

{{< tabs items="JS,JSON,YAML" >}}

{{< tab >}}
```js
export default [
  {
    name: "Create User",
    endpoint: "/api/users",
    request: {
      method: "POST",
      body: { name: "John", email: "john@example.com" },
    },
    response: {
      status: 201,
      headers: { "content-type": /application\/json/ },
      json: { name: "John", email: "john@example.com" },
    },
  },
  {
    name: "Reject Duplicate Email",
    endpoint: "/api/users",
    request: {
      method: "POST",
      body: { name: "Jane", email: "john@example.com" },
    },
    response: {
      status: 409,
      json: { error: "email already exists" },
    },
  },
];
```
{{< /tab >}}

{{< tab >}}
```json
[
  {
    "name": "Create User",
    "endpoint": "/api/users",
    "request": {
      "method": "POST",
      "body": { "name": "John", "email": "john@example.com" }
    },
    "response": {
      "status": 201,
      "headers": { "content-type": "application/json" },
      "json": { "name": "John", "email": "john@example.com" }
    }
  },
  {
    "name": "Reject Duplicate Email",
    "endpoint": "/api/users",
    "request": {
      "method": "POST",
      "body": { "name": "Jane", "email": "john@example.com" }
    },
    "response": {
      "status": 409,
      "json": { "error": "email already exists" }
    }
  }
]
```
{{< /tab >}}

{{< tab >}}
```yaml
- name: Create User
  endpoint: /api/users
  request:
    method: POST
    body:
      name: John
      email: john@example.com
  response:
    status: 201
    headers:
      content-type: application/json
    json:
      name: John
      email: john@example.com

- name: Reject Duplicate Email
  endpoint: /api/users
  request:
    method: POST
    body:
      name: Jane
      email: john@example.com
  response:
    status: 409
    json:
      error: email already exists
```
{{< /tab >}}

{{< /tabs >}}

</div>

</div>

{{< hint type="tip" >}}
**That's it.** No setup, no boilerplate, no ceremony. Describe what you expect and Spectest handles execution, validation, and reporting.
{{< /hint >}}

{{< section-head
  eyebrow="Loved by developers"
  title="Teams ship with confidence on Spectest"
  subtitle="From fintech platforms to developer-tool startups, engineers use Spectest to keep their APIs honest." >}}

{{< testimonials >}}

{{< section-head
  eyebrow="Feature deep dive"
  title="Built to be feature-rich and robust"
  subtitle="The capabilities teams reach for once tests move beyond a single happy-path request." >}}

{{< feature-spotlight
  eyebrow="OpenAPI native"
  title="Your spec is your test suite"
  body="Point Spectest at an OpenAPI 3.0 or 3.1 document and run its operations directly — no hand-written suite required. Catch contract drift the moment your implementation diverges from the spec, right in CI."
  link="/docs/guides/openapi-testing/"
  link_text="OpenAPI testing guide" >}}
```bash
# Run every operation defined in your spec
npx spectest --openapi ./openapi.yaml

  ✓ GET /users          200  (12ms)
  ✓ POST /users         201  (18ms)
  ✓ GET /users/{id}     200  (9ms)
  ✓ DELETE /users/{id}  204  (7ms)

  4 passed · 0 failed · 46ms
```
{{< /feature-spotlight >}}

{{< feature-spotlight
  eyebrow="Deterministic by design"
  title="Record once, replay forever"
  body="Capture the outbound HTTP calls your server makes to third parties, then replay them deterministically on every run. Flaky integration tests become reproducible, offline-friendly, and fast."
  link="/docs/guides/http-recording/"
  link_text="Record & replay guide"
  flip="true" >}}
```js
export default {
  name: "Checkout charges Stripe",
  endpoint: "/api/checkout",
  request: { method: "POST", body: { plan: "pro" } },
  // Outbound call to Stripe is recorded on first
  // run, then replayed on every run after.
  record: "./fixtures/stripe.har",
  response: { status: 200, json: { paid: true } },
};
```
{{< /feature-spotlight >}}

{{< feature-spotlight
  eyebrow="Powerful assertions"
  title="Match exactly what matters"
  body="Assert on status, headers, and bodies with deep JSON matching, regex, partial objects, snapshots, and Zod schemas. Ignore volatile fields like timestamps and IDs without brittle string comparisons."
  link="/docs/integrations/zod/"
  link_text="Schema assertions" >}}
```js
import { z } from "zod";

export default {
  name: "List users",
  endpoint: "/api/users",
  response: {
    status: 200,
    schema: z.array(
      z.object({
        id: z.string().uuid(),
        email: z.string().email(),
      })
    ),
  },
};
```
{{< /feature-spotlight >}}

{{< section-head
  eyebrow="Comparison"
  title="How Spectest stacks up"
  subtitle="An honest look at how Spectest compares to the tools teams reach for today. Every option has its place — Spectest is built for fast, declarative, CI-first API testing." >}}

{{< comparison >}}

{{< section-head
  eyebrow="Quick start"
  title="From zero to your first passing test in minutes"
  subtitle="Install the CLI, point it at your API, and describe what you expect." >}}

{{% steps %}}

### Install Spectest

```bash
npm install -g spectest
```

### Create a Config File

```js
// spectest.config.js
export default {
  baseUrl: "https://api.example.com",
  testDir: "./test",
  filePattern: "\.spectest\.",
};
```

### Create Your First Test

```js
// health.spectest.js
export default [
  { name: "Health Check", endpoint: "/health", response: { status: 200 } },
];
```

### Run Your Tests

```bash
npx spectest
```

{{% /steps %}}

{{< section-head
  eyebrow="Integrations"
  title="Fits the stack you already use"
  subtitle="Spectest plays nicely with your test runner, your schemas, and your API contracts." >}}

{{< cards >}}
{{< card title="Jest" icon="beaker" link="/docs/integrations/jest/" subtitle="Run Spectest suites from within Jest for a unified test report." >}}
{{< card title="Vitest" icon="lightning-bolt" link="/docs/integrations/vitest/" subtitle="First-class Vitest integration for modern TypeScript projects." >}}
{{< card title="Zod" icon="shield-check" link="/docs/integrations/zod/" subtitle="Validate response bodies against your existing Zod schemas." >}}
{{< card title="OpenAPI" icon="code" link="/docs/integrations/openapi/" subtitle="Execute and validate directly against your OpenAPI document." >}}
{{< /cards >}}

{{< final-cta
  title="Ready to transform your API testing?"
  subtitle="Install Spectest today and write your first declarative test in under five minutes. It's free, open source, and MIT licensed."
  cta_text="Read the Docs"
  cta_url="/docs/introduction/getting-started/"
  cta_secondary_text="Star on GitHub"
  cta_secondary_url="https://github.com/justiceo/spectest"
>}}

<div class="st-license">
  <span class="st-license-icon">{{< icon name="badge-check" attributes="width=20 height=20" >}}</span>
  <div>
    <p class="st-license-title">Open Source &amp; MIT Licensed</p>
    <p class="st-license-text">Spectest is free, open-source software released under the MIT license. Use it anywhere, modify it as needed, and contribute back to help make API testing better for everyone.</p>
  </div>
</div>
