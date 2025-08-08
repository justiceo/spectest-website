---
title: "Test Filtering"
weight: 1
---

Large projects quickly accumulate dozens of suites. Spectest provides several mechanisms to target a subset of tests when you only need to run a portion of them.

## By File

Use `--suite-file` to run a single suite or `--file-pattern` to limit the discovery step.

```bash
npx spectest auth.spectest.js
npx spectest --file-pattern="auth*"
```

## Tags

Tag your cases then pass a comma separated list via `--tags`.

```js
export default [
  { name: 'Create user', endpoint: '/users', tags: ['users','create'] },
  { name: 'List users', endpoint: '/users', tags: ['users','list'] },
];
```

```bash
npx spectest --tags=users
npx spectest --tags=users,list
```

## Name Patterns and Smart Filters

`--filter` accepts a regular expression or one of the built in aliases:

- `happy` – only cases expecting a 2xx status
- `failures` – rerun tests that failed in the last snapshot

```bash
npx spectest --filter="Login"
```

## Focus and Skip

When investigating a failing case you can mark it with `focus: true` to ignore the rest. Similarly `skip: true` temporarily disables a case. The `focus()` and `skip()` helpers apply these flags to entire suites.

## Randomization and Dependencies

`--randomize` shuffles the remaining tests after filtering. Use `dependsOn` inside a case to declare prerequisites so dependent operations run after their parents succeed.
