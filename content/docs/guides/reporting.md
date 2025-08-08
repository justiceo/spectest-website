---
title: "Reporting"
weight: 4
---

The CLI prints a concise summary after every run. With the `--verbose` flag you also get server logs grouped under each case.

## Standard Output

Each suite is listed with the individual results:

```text
📊 Test Summary:
 [✅] Login (45ms)
 [❌] Fetch profile (140ms)
```

Latency statistics and the overall pass count appear at the end. A non‑zero exit code indicates failures which CI systems can detect.

## Verbose Mode

Enable verbose output when debugging to see request identifiers and server log lines:

```bash
npx spectest --verbose
```

Any `console` output from your server is collected and printed beneath the related test when verbose mode is on or when the test fails.

## Snapshot Reports

When using `--snapshot` the report file contains the same pass/fail status and latency numbers. This makes it possible to compare historical runs or feed the data into external dashboards.

## Continuous Integration

Add Spectest to your pipeline by executing the CLI as part of your test job. Example using GitHub Actions:

```yaml
- name: Run API tests
  run: npx spectest --base-url=$URL --snapshot=reports/snapshot.json
```

Upload the snapshot or parse the console output to visualize results over time.
