---
title: "Jest"
weight: 1
---

Jest is the de‑facto standard for unit testing JavaScript applications. Spectest complements Jest by exercising your running API just like a browser would. Keep your Jest suites for isolated logic and add Spectest for end‑to‑end HTTP verification.

## Setup

Install both packages in your project:

```bash
npm install --save-dev jest spectest
```

Create a `spectest.config.js` at the project root:

```js
// spectest.config.js
export default {
  baseUrl: 'http://localhost:3000',
  testDir: './test/api',
  filePattern: '\\.spectest\\.',
};
```

Jest's own configuration lives in `jest.config.js` as usual.

## Sample project structure

```text
my-project/
├─ src/
├─ test/
│  ├─ api/          # Spectest suites
│  │   └─ users.spectest.js
│  └─ unit/         # Jest tests
│      └─ users.test.js
├─ jest.config.js
└─ spectest.config.js
```

## Running both toolchains

Add npm scripts to run them separately or together:

```json
{
  "scripts": {
    "test:unit": "jest",
    "test:api": "spectest",
    "test": "npm run test:unit && npm run test:api"
  }
}
```

This keeps output from each tool distinct while letting CI execute a single `npm test` command.

### Invoking Spectest from Jest

If you prefer to launch Spectest inside a Jest test you can spawn the CLI and assert on the exit code:

```js
// api.test.js
import { spawnSync } from 'child_process';

test('API contract', () => {
  const result = spawnSync('npx', ['spectest'], { stdio: 'inherit' });
  expect(result.status).toBe(0);
});
```

### Sharing utilities

Utility functions and fixtures can be imported by both Jest and Spectest suites. Keep them in a common folder and reference them from your `.spectest.js` and `.test.js` files.

## CI/CD example

A GitHub Actions job might look like:

```yaml
- uses: actions/checkout@v3
- run: npm ci
- run: npm test
```

Spectest exits with a non‑zero status on failures so the workflow fails when either Jest or Spectest tests fail.

## Migration tips

To migrate existing API tests written in Jest, move the HTTP calls into `.spectest.js` suites. Spectest's declarative format reduces boilerplate and runs faster. Keep pure logic tests in Jest.

## Troubleshooting

- **TypeScript suites** – compile `.ts` files to JavaScript before running Spectest.
- **Watch mode** – Spectest runs to completion each time; use a separate terminal if you rely on `jest --watch`.
- **Exit codes** – ensure your scripts pass through Spectest's exit code so CI can detect failures.

