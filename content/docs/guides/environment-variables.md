---
title: "Environment Variables"
weight: 3
---

Because the configuration file is plain JavaScript you can easily incorporate environment variables. This lets you keep secrets and server addresses outside of source control.

## Using Env Vars in Configuration

```js
// spectest.config.js
export default {
  baseUrl: process.env.API_BASE_URL,
  testDir: './test',
  filePattern: '\\.spectest\\.',
};
```

Set `API_BASE_URL` before invoking Spectest and the tests will target that server:

```bash
API_BASE_URL=https://staging.example.com npx spectest
```

## Passing Secrets

Sensitive values such as authentication tokens can be injected in a `beforeSend` hook or read from the environment inside your suite.

```js
export default [
  {
    name: 'Fetch profile',
    endpoint: '/profile',
    beforeSend: req => {
      req.headers = { ...req.headers, Authorization: `Bearer ${process.env.API_TOKEN}` };
    },
    response: { status: 200 }
  }
];
```

## CI/CD Integration

In a pipeline, export the required environment variables as part of the job configuration. Spectest exits with a non-zero status on failures so it fits naturally into `npm test` or dedicated testing steps.
