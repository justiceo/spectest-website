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

## Failure Detail

Set `--test-output=errors` to include failed-test server logs and failure reasons directly in the console report, instead of just pass/fail. This is independent of `--verbose`, which controls the runner's own logging.

## OpenAPI Coverage Reports

When running against an OpenAPI document with `--openapi`, pass `--coverage-report` to print a per-operation contract coverage summary after the run — which operations were generated and passed, generated and failed, generated and skipped (with a reason), covered only by a hand-written test, or left uncovered entirely. Use `--coverage-report-file=<path>` to write it to a file instead of stdout. See [OpenAPI Testing](/docs/guides/openapi-testing/) for details.

## Continuous Integration

Add Spectest to your pipeline by executing the CLI as part of your test job. Example using GitHub Actions:

```yaml
- name: Run API tests
  run: npx spectest --base-url=$URL --snapshot=reports/snapshot.json
```

Upload the snapshot or parse the console output to visualize results over time.
