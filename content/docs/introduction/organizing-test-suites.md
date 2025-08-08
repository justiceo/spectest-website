---
title: "Organizing Test Suites"
weight: 4
---

As your project grows you'll accumulate many test files. Spectest offers several tools to keep them maintainable.

## File layout

Suites live in the directory specified by `testDir` and must match `filePattern`. You can mix JavaScript, ESM, CommonJS and plain JSON files. Typescript should be transpiled ahead of time.

## Tags and filters

Assign tags to cases and run subsets of your suite:

```js
{
  name: 'Fetch TODOs',
  endpoint: '/todos/',
  tags: ['todo', 'collection']
}
```

Run with:

```bash
npx spectest --tags=todo
```

Use `--filter` to match by name or the built-in `happy` and `failures` filters.

## Dependencies

`dependsOn` allows explicit sequencing between operations. Tests with unmet dependencies are skipped until prerequisites pass.

## Rate limiting

Large suites may hit third‑party services hard. Set `rps` in your config or via `--rps` to cap requests per second.
