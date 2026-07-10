---
title: "HTTP Recording"
weight: 6
---

Spectest can record and replay the **outbound** HTTP requests made by a Node-based server under test (SUT). Spectest still sends real requests to your local API — recording only intercepts the calls your API makes to *other* services (a payments provider, a third-party API, etc.), so tests stay fast and deterministic without hand-rolled mocks.

This works by starting the SUT process with a recording preload so its `fetch`, `http`, `https`, and common Node HTTP library calls can be captured. Because instrumentation requires controlling process startup, **recording is incompatible with `runningServer: 'reuse'`** — Spectest must be the one that spawns the server.

## Configuration

```js
// spectest.config.js

export default {
  baseUrl: 'http://localhost:3000',
  startCmd: 'npm run start',
  runningServer: 'kill',
  recording: 'replay',
  recordingFile: '.spectest/cassette.json',
  missingRecordingBehavior: 'fail',
  recordingExcludeUrls: [
    'https://telemetry.example.com/',
    /^https:\/\/metadata\.google\.internal\//,
    (url) => url.hostname.endsWith('.internal.example.com'),
  ],
};
```

## Recording and replaying

Create or update a cassette:

```bash
npx spectest --recording=record
```

Replay from the cassette:

```bash
npx spectest --recording=replay
```

## Handling replay misses

`missingRecordingBehavior` controls what happens when replay can't find a matching cassette entry:

| Value | Behavior |
| ----- | -------- |
| `fail` | Fail the outbound request with a clear unmatched-recording error |
| `record` | Allow the real outbound request and save it to the cassette |
| `bypass` | Allow the real outbound request without saving it |

`recordingExcludeUrls` always bypasses cassette handling entirely, regardless of mode. String entries are canonical URL prefix matches, `RegExp` entries are tested against the canonical URL string, and function entries receive `(url, request)` and return a boolean.

## Per-test overrides

A test case's `recording` field overrides only the mode for that test; when unset it inherits the run-level `recording` config.

```js
export default [
  {
    name: 'Health check bypasses cassette',
    endpoint: '/health',
    recording: 'off',
  },
];
```

The `recording(tests, mode)` [helper](/docs/introduction/helpers/) applies this to an entire suite at once.

## Using it outside Spectest suites

The framework-agnostic `useHttpRecordings` helper lets you drive the same cassette from plain Node test runners (Jest, Vitest, `node --test`), not just `.spectest.*` suites:

```js
import { useHttpRecordings } from 'spectest/recordings';

let recordings;

beforeAll(async () => {
  recordings = await useHttpRecordings({
    file: '.spectest/cassette.json',
    mode: 'replay',
    missingRecordingBehavior: 'fail',
    recordingExcludeUrls: ['https://telemetry.example.com/'],
  });
});

afterAll(async () => {
  await recordings.dispose();
});
```

Related: [Jest](/docs/integrations/jest/) and [Vitest](/docs/integrations/vitest/) for running Spectest alongside those frameworks.
