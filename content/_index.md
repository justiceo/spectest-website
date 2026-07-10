---
description: "Lightning fast and declarative API testing"
toc: false
---

{{< hero
  badge="Open Source · MIT Licensed"
  title="API testing without the pain"
  tagline="Run declarative HTTP tests quickly from the CLI. No boilerplate, no ceremony — just describe what you expect."
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

## Why Developers Choose Spectest

{{< cards >}}
{{< card title="Fast Execution" icon="lightning-bolt" subtitle="Execute hundreds of tests in seconds with parallel execution." >}}
{{< card title="Truly Declarative" icon="document-text" subtitle="Write tests in JavaScript or JSON. No boilerplate, just describe what you expect." >}}
{{< card title="OpenAPI-Native" icon="code" link="/docs/guides/openapi-testing/" subtitle="Run OpenAPI 3.0/3.1 operations directly, no hand-written suite required." >}}
{{< card title="Record & Replay" icon="save" link="/docs/guides/http-recording/" subtitle="Capture outbound HTTP calls from your server and replay them deterministically." >}}
{{< /cards >}}

## See It In Action

Here's how simple API testing becomes with Spectest:

{{< tabs items="Traditional Testing,Spectest" >}}

{{< tab >}}
```javascript
describe('User API', () => {
  it('should create user and return 201', async () => {
    const response = await fetch('/api/users', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ name: 'John', email: 'john@example.com' })
    });

    expect(response.status).toBe(201);
    const user = await response.json();
    expect(user).toHaveProperty('id');
    expect(user.name).toBe('John');
    expect(user.email).toBe('john@example.com');
  });
});
```
{{< /tab >}}

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
      json: {
        name: "John",
        email: "john@example.com",
      },
    },
  },
];
```
{{< /tab >}}

{{< /tabs >}}

{{< hint type="tip" >}}
**That's it!** No setup, no boilerplate, no ceremony. Just describe what you want to test and Spectest handles the execution, validation, and reporting.
{{< /hint >}}

## Quick Start Guide

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

## Ready to Transform Your API Testing?

{{< cards >}}
{{< card title="Read the Docs" icon="book-open" link="/docs/introduction/" subtitle="Comprehensive guides, examples, and API references to get you up to speed quickly" >}}
{{< card title="View on GitHub" icon="github" link="https://github.com/justiceo/spectest" subtitle="Explore the source code, contribute, or report issues. We ❤️ community feedback!" >}}
{{< card title="Get Support" icon="chat" link="/docs/more/about/" subtitle="Join our community or reach out for help" >}}
{{< /cards >}}

<div class="st-license">
  <span class="st-license-icon">{{< icon name="badge-check" attributes="width=20 height=20" >}}</span>
  <div>
    <p class="st-license-title">Open Source &amp; MIT Licensed</p>
    <p class="st-license-text">Spectest is free, open-source software released under the MIT license. Use it anywhere, modify it as needed, and contribute back to help make API testing better for everyone.</p>
  </div>
</div>
