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
| `phase` | `setup`, `main` (default), or `teardown` — controls which of the three execution waves the test belongs to |
| `dependsOn` | List of operationIds that must pass first |

Other fields like `beforeSend`, `postTest`, `tags`, `delay` and `timeout` allow advanced control.

## Test Suite

Suites are files exporting an array of test cases or an object `{ name, tests, setup?, teardown? }`. The CLI loads every file matching `filePattern` inside `testDir` and runs the cases concurrently, subject to `rps` and `dependsOn`.

An OpenAPI 3.0/3.1 document passed via `--openapi` is loaded the same way, in memory, alongside any hand-written suites — see [OpenAPI Testing](/docs/guides/openapi-testing/).

## Configuration

Settings can come from `spectest.config.js`, a custom config file (`--config`) or CLI flags, in that order of precedence — CLI flags win. The built-in defaults are:

```js
export default {
  startCmd: 'npm run start',
  baseUrl: 'https://localhost:8080',
  testDir: './test',
  filePattern: '\\.spectest\\.',
  rps: Infinity,
  timeout: 60000,
  randomize: false,
  happy: false,
  filter: '',
  testOutput: 'summary',
  runningServer: 'reuse',
  userAgent: 'chrome_windows',
  proxy: '',
  recording: 'off',
  recordingFile: '.spectest/cassette.json',
  missingRecordingBehavior: 'fail',
  recordingExcludeUrls: [],
  openapiAuth: {},
};
```

See the [Config options](/docs/api-references/cli/) table for a description of every field, and [HTTP Recording](/docs/guides/http-recording/) for the `recording*` group.

{{< hint warning >}}
If multiple suites define the same `operationId` — including a hand-written suite and an OpenAPI-generated one — Spectest will exit with an error.
{{< /hint >}}

## Running Tests

The CLI takes care of starting your server (via `startCmd`, built first with `buildCmd` if set), limiting the request rate, and printing detailed results. Internally it runs a small plugin pipeline — loaders, a filter/prepare stage, and a console reporter — but you don't need to know that to use the CLI day to day.
