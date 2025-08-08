---
title: "CLI"
weight: 1
---

The `spectest` command runs suites and manages the test environment. All options correspond to configuration fields and may be supplied via the config file or the command line.

```bash
npx spectest [suiteFile] [options]
```

### Options

| Flag | Description | Default |
| ---- | ----------- | ------- |
| `--config=<file>` | Load additional configuration from file | none |
| `--base-url=<url>` | Base URL for all requests | `https://localhost:8080` |
| `--test-dir=<dir>` | Directory containing test suites | `./test` |
| `--file-pattern=<regex>` | Pattern used to locate suite files | `\.spectest\.` |
| `--start-cmd=<cmd>` | Command used to start the server | `npm run start` |
| `--running-server=<reuse|fail|kill>` | How to handle an already running server | `reuse` |
| `--tags=<list>` | Comma separated tags for filtering | none |
| `--rps=<number>` | Requests per second limit | `Infinity` |
| `--timeout=<ms>` | Default per‑test timeout | `30000` |
| `--snapshot=<file>` | Write snapshot JSON to file | none |
| `--randomize` | Shuffle execution order | `false` |
| `--happy` | Run only tests expecting a 2xx status | `false` |
| `--filter=<pattern>` | Regex or built‑in filter (`happy`, `failures`) | none |
| `--verbose` | Print detailed logs | `false` |
| `--user-agent=<name>` | Override the User‑Agent header | `chrome_windows` |
| `--ua=<name>` | Alias of `--user-agent` | |

Provide a file path without a flag to run just that suite.

See [Test Suite](./suite/) and [Test Case](./test-case/) for details on the file format.

### Examples

```bash
# Run every suite discovered in testDir
npx spectest

# Only run a specific file
npx spectest auth.spectest.js

# Run with mobile user agent and record snapshots
npx spectest --user-agent=chrome_android --snapshot=snap.json
```
