---
title: "Outbound Throttling"
weight: 7
---

Spectest can rate-limit the **outbound** HTTP requests made by a Node-based server under test (SUT) — calls your API makes to a payments provider, a third-party API, etc. This is separate from `--rps`, which limits how fast Spectest itself calls your API; `outboundThrottle` limits calls going the other direction, from your API out to the services it depends on.

Like [HTTP recording](/docs/guides/http-recording/), this works by injecting instrumentation into the SUT process at startup, so **outbound throttling is incompatible with `runningServer: 'reuse'`** — Spectest must be the one that spawns the server.

## Configuration

`outboundThrottle` is config-file only; there is no CLI flag.

```js
// spectest.config.js
export default {
  startCmd: 'npm run start',
  runningServer: 'kill',
  outboundThrottle: [
    { match: 'https://api.stripe.com', rps: 5, name: 'stripe' },
    { match: /^https:\/\/.*\.internal\.example\.com/, rps: 20 },
  ],
};
```

Each rule:

| Key | Description |
| --- | ----------- |
| `match` | A string (canonical URL prefix match) or `RegExp` (tested against the canonical URL) |
| `rps` | Requests per second allowed for outbound calls matching this rule |
| `name` | Optional label used in throttling-related log output |

Rules are evaluated in order and the first match wins; outbound calls that match no rule are unthrottled.

Related: [HTTP Recording](/docs/guides/http-recording/) for the other SUT-process instrumentation feature, and [CLI](/docs/api-references/cli/) for `--server-startup-timeout` / `--server-health-check-interval`, which control how long Spectest waits for the (now-instrumented) server to become ready.
