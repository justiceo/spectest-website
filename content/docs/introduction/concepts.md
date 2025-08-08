---
title: "Core Concepts"
weight: 2
---

Spectest revolves around a few simple ideas:

## Test Case

A test case describes a single HTTP operation. At minimum it needs a `name` and an `endpoint`. Options such as `request` and `response` mirror the browser [Request] and [Response] objects.

| Key | Purpose |
| --- | --- |
| `name` | Human readable name |
| `endpoint` | Path relative to `baseUrl` |
| `operationId` | Unique identifier; defaults to the name |
| `dependsOn` | List of operationIds that must pass first |

Other fields like `beforeSend`, `postTest`, `tags`, `delay` and `timeout` allow advanced control.

## Test Suite

Suites are files exporting an array of test cases or an object `{ name, tests }`. The CLI loads every file matching `filePattern` inside `testDir` and runs the cases concurrently.

## Configuration

Settings can come from `spectest.config.js`, a custom config file or CLI flags. The defaults are:

```js
export default {
  startCmd: 'npm run start',
  baseUrl: 'https://localhost:8080',
  testDir: './test',
  filePattern: '\\.spectest\\.',
  rps: Infinity,
  timeout: 30000,
  randomize: false,
  happy: false,
  filter: '',
  runningServer: 'reuse',
  userAgent: 'chrome_windows',
};
```

{{< hint warning >}}
If multiple suites define the same `operationId`, Spectest will exit with an error.
{{< /hint >}}

## Running Tests

The CLI takes care of starting your server (via `startCmd`), limiting the request rate and printing detailed results.
