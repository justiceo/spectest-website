---
title: "Test Suite"
weight: 2
---

A **suite** is a file that exports either an array of test cases or an object with a `name` and `tests` array. Spectest loads every file under `testDir` matching `filePattern` and runs the contained cases.

```js
// basic array
export default [
  { name: 'Fetch TODO 1', endpoint: '/todos/1' },
  { name: 'Create Post', endpoint: '/posts', request: { method: 'POST' } }
];
```

```js
// named suite
export default {
  name: 'Comments Tests',
  tests: [
    { name: 'List', endpoint: '/comments/' },
    { name: 'Get 1', endpoint: '/comments/1' }
  ]
};
```

A suite object may also declare `setup` and `teardown` arrays, run before and after `tests` respectively — equivalent to tagging individual cases with `phase: 'setup'` / `phase: 'teardown'`:

```js
export default {
  name: 'Auth Tests',
  setup: [{ name: 'Ping Server', endpoint: '/ping' }],
  tests: [{ name: 'Login', endpoint: '/login', request: { method: 'POST' } }],
  teardown: [{ name: 'Logout', endpoint: '/logout' }],
};
```

Suites may use CommonJS, ES modules, JSON, or YAML. Each file becomes its own suite and the file name (without extension) is used when no name is provided. Typescript (`.ts`) suite files aren't supported — transpile them to JavaScript first.

An OpenAPI document loaded with `--openapi` is treated as another suite source alongside hand-written files — its generated tests and a hand-written suite share the same `operationId` namespace and dependency graph. See [OpenAPI Testing](/docs/guides/openapi-testing/).

Related: [Test Case](/docs/api-references/test-case/) for the available case options.
