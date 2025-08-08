---
title: "Snapshots"
weight: 2
---

Snapshots capture the exact requests and responses from a test run. They are invaluable when reviewing failures or updating expected data.

## Creating a Snapshot

Pass the `--snapshot` flag with a file path. After the run, Spectest writes a JSON report containing each case with its final request, response and overall status.

```bash
npx spectest --snapshot=.spectest/snap.json
```

The file structure resembles:

```json
{
  "lastUpdate": "2024-05-01 15:04 PDT",
  "cases": [
    {
      "name": "Create user",
      "operationId": "Create user",
      "suite": "auth",
      "request": { "method": "POST", "url": "/users" },
      "response": { "status": 201, "data": { "id": 5 } },
      "status": "pass",
      "latency": 120
    }
  ]
}
```

## Updating Tests

Copy the captured response bodies back into your suite to keep expectations in sync. You can also rerun only failing cases with `--filter=failures` which consults the snapshot file.

## CI Usage

Snapshots make it easy to archive historical runs in your CI pipeline. Commit the JSON artifacts or upload them to your results store for later comparison.
