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

Suites may use CommonJS, ES modules or JSON. Each file becomes its own suite and the file name (without extension) is used when no name is provided.

Related: [Test Case](/docs/api-references/test-case/) for the available case options.
