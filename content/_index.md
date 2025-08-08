---
description: "Lightning fast and declarative API testing"
toc: false
---

{{< hero title="API testing without the pain" tagline="Run declarative HTTP tests quickly from the CLI." image="https://raw.githubusercontent.com/justiceo/spectest/refs/heads/main/assets/spectest-logo-transparent.png" cta_text="Get Started" cta_url="/docs/introduction/getting-started/" />}}

## Why Developers Choose Spectest

{{< cards >}}
{{< card title="Fast Execution" icon="lightning-bolt" subtitle="Execute hundreds of tests in seconds with parallel execution." >}}
{{< card title="Truly Declarative" icon="document-text" subtitle="Write tests in JavaScript or JSON. No boilerplate, just describe what you expect." >}}
{{< card title="Focused Workflow" icon="cursor-click" subtitle="Built for API testing. Smart defaults and intuitive syntax keep you focused." >}}
{{< /cards >}}

## See It In Action

Here's how simple API testing becomes with Spectest:

### Before: Traditional API Testing
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

### After: Spectest Declarative Testing
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
        id: "@type:number",
        name: "John",
        email: "john@example.com",
      },
    },
  },
];
```

{{< hint type="tip" >}}
**That's it!** No setup, no boilerplate, no ceremony. Just describe what you want to test and Spectest handles the execution, validation, and reporting.
{{< /hint >}}

## Quick Start Guide

{{< cards >}}
{{< card title="1. Install" icon="download" subtitle="Get up and running with the CLI" >}}
{{< card title="2. Create Config" icon="cog" subtitle="Define baseUrl in spectest.config.js" >}}
{{< card title="3. Write Tests" icon="document-add" subtitle="Author cases in JavaScript or JSON" >}}
{{< card title="4. Run Tests" icon="play" subtitle="Execute suites with clear results" >}}
{{< /cards >}}

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

## Ready to Transform Your API Testing?

{{< cards >}}
{{< card title="Read the Docs" icon="book-open" link="/docs/introduction/" subtitle="Comprehensive guides, examples, and API references to get you up to speed quickly" >}}
{{< card title="View on GitHub" icon="github" link="https://github.com/justiceo/spectest" subtitle="Explore the source code, contribute, or report issues. We ❤️ community feedback!" >}}
{{< card title="Get Support" icon="chat" link="/docs/more/about/" subtitle="Join our community or reach out for help" >}}
{{< /cards >}}

---

{{< hint type="info" >}}
**Open Source & MIT Licensed** • Spectest is free, open-source software released under the MIT license. Use it anywhere, modify it as needed, and contribute back to help make API testing better for everyone.
{{< /hint >}}