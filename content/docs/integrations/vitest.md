---
title: "Vitest"
weight: 2
---

Vitest provides a fast, Vite‑powered alternative to Jest. Spectest can run alongside Vitest to cover your API while Vitest handles component and unit tests.

## Setup

Install the packages:

```bash
npm install --save-dev vitest spectest
```

Create `spectest.config.js`:

```js
// spectest.config.js
export default {
  baseUrl: 'http://localhost:3000',
  testDir: './test/api',
  filePattern: '\\.spectest\\.',
};
```

Vitest's configuration goes in `vitest.config.js` or inside `vite.config.js` depending on your project.

## Example project layout

```text
my-project/
├─ src/
├─ test/
│  ├─ api/          # Spectest suites
│  │   └─ users.spectest.js
│  └─ unit/         # Vitest tests
│      └─ users.test.ts
├─ vitest.config.ts
└─ spectest.config.js
```

## Running both

Define npm scripts similar to:

```json
{
  "scripts": {
    "test:unit": "vitest run",
    "test:api": "spectest",
    "test": "npm run test:unit && npm run test:api"
  }
}
```

Run `npm test` locally or in CI to execute everything in sequence.

### Calling Spectest from Vitest

You can also start Spectest inside a Vitest test using `execa` or `child_process`:

```ts
// api.test.ts
import { execaSync } from 'execa';
import { expect, test } from 'vitest';

test('API contract', () => {
  const { exitCode } = execaSync('npx', ['spectest'], { stdio: 'inherit' });
  expect(exitCode).toBe(0);
});
```

### Sharing utilities

Place common helpers in a shared folder and import them from both Spectest and Vitest files. This avoids duplication of payload builders or authentication helpers.

## CI/CD example

```yaml
- uses: actions/checkout@v3
- run: npm ci
- run: npm test
```

## Migration tips

Move any HTTP‑level tests from Vitest into `.spectest.js` suites. Vitest remains ideal for unit tests and component behavior while Spectest focuses on API contracts.

## Troubleshooting

- **Process exits** – Vitest stops on the first failing test, whereas Spectest runs all cases. When combining them ensure both exit codes propagate.
- **Watch mode** – Run Spectest in a separate terminal when using `vitest --watch`.

