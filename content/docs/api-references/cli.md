---
title: "CLI"
weight: 1
---

The `spectest` command runs suites and manages the test environment. All options correspond to configuration fields and may be supplied via the config file or the command line. CLI flags take precedence over `spectest.config.js`, which takes precedence over the built-in defaults.

```bash
npx spectest [suiteFile] [options]
```

### Options

| Flag | Description | Default |
| ---- | ----------- | ------- |
| `--config=<file>` | Path to an extra config file | none |
| `--base-url=<url>` | Base URL of the API | `https://localhost:8080` |
| `--test-dir=<dir>` | Directory containing test suites | `./test` |
| `--file-pattern=<regex>` | Regex for suite filenames | `\.spectest\.` |
| `--start-cmd=<cmd>` | Command to start the test server | `npm run start` |
| `--build-cmd=<cmd>` | Command to build the test server | none |
| `--running-server=<reuse\|fail\|kill>` | Handling for an already running server | `reuse` |
| `--tags=<tag1,tag2>` | Comma-separated tags used for filtering tests | none |
| `--rps=<number>` | Requests per second rate limit | `Infinity` |
| `--timeout=<ms>` | Default request timeout in milliseconds | `60000` |
| `--snapshot=<file>` | Path to write a snapshot file | none |
| `--randomize` | Shuffle test ordering before execution | `false` |
| `--happy` | Run only tests expecting a 2xx status | `false` |
| `--filter=<pattern>` | Regex or smart filter to select tests (`happy`, `failures`) | none |
| `--test-output=<summary\|errors>` | Executed test result detail; `errors` includes failed-test server logs and failure reasons | `summary` |
| `--verbose` | Verbose spectest runner/program output (separate from `--test-output`) | `false` |
| `--user-agent=<name>`, `--ua=<name>` | Browser User-Agent string or one of the predefined [user-agents](https://github.com/justiceo/spectest/blob/main/src/user-agents.ts) | `chrome_windows` |
| `--proxy=<url>` | Proxy URL to route requests through | none |
| `--recording=<off\|replay\|record>` | HTTP recording mode for outbound Node SUT requests | `off` |
| `--recording-file=<path>` | JSON cassette path for HTTP recordings | `.spectest/cassette.json` |
| `--missing-recording-behavior=<fail\|record\|bypass>` | Behavior when replay can't find a cassette entry | `fail` |
| `--openapi=<path>` | Path to an OpenAPI 3.0/3.1 document to load directly | none |
| `--openapi-server=<url\|index>` | Server URL or index to select from an OpenAPI document with multiple `servers` entries | none |
| `--coverage-report` | Print a per-operation OpenAPI contract coverage report after the run | `false` |
| `--coverage-report-file=<path>` | Write the coverage report to a file instead of stdout | none |
| `--dir=<path>` | Root directory of the project; anchors relative paths like `testDir`, `openapi`, `recordingFile` | current working directory |
| `-h`, `--help` | Show the help message and exit | |

Provide a file path without a flag to run just that suite. Options that accept a list of enum-like values (`recording`, `runningServer`, `testOutput`, `missingRecordingBehavior`) are validated whether they come from the CLI or from `spectest.config.js`.

See [Test Suite](../suite/) and [Test Case](../test-case/) for details on the file format, and [OpenAPI Testing](../../guides/openapi-testing/) and [HTTP Recording](../../guides/http-recording/) for the larger features above.

### Examples

```bash
# Run every suite discovered in testDir
npx spectest

# Only run a specific file
npx spectest auth.spectest.js

# Run with mobile user agent and write a snapshot
npx spectest --user-agent=chrome_android --snapshot=snap.json

# Load and run tests directly from an OpenAPI document
npx spectest --openapi ./openapi.yaml --base-url=https://api.example.com

# Print contract coverage after running an OpenAPI-backed suite
npx spectest --openapi ./openapi.yaml --coverage-report
```

### `generate openapi-tests`

Scaffolds an editable `.spectest.js` file containing the same tests Spectest would generate in-memory from an OpenAPI document:

```bash
npx spectest generate openapi-tests --openapi <path> --output <dir> [--dir <projectRoot>] [--config <file>]
```

This is a convenience for suites that will grow real `beforeSend`/`postTest` logic by hand — everything it writes is already expressible via direct `--openapi` loading.
