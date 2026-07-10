---
title: "OpenAPI Testing"
weight: 5
---

Spectest can load an OpenAPI 3.0 or 3.1 document directly and run its operations as tests — no hand-written suite required.

```bash
npx spectest --openapi ./openapi.yaml --base-url=https://api.example.com
```

Generated tests stay in memory; Spectest does not write `.spectest` files for this workflow (see [Scaffolding editable test files](#scaffolding-editable-test-files) if you want a written starting point instead). Spectest resolves required path/query/header/cookie parameters and required JSON request bodies from examples or schema defaults. An operation that needs a value Spectest can't resolve — a missing example, unsupported media type, unsupported parameter serialization, unresolved external `$ref`, unsupported schema construct, or a missing hook/auth lookup — is generated as a **skipped** test with a `skipReason`, rather than failing the loader.

When both `--openapi` and `testDir` are configured, Spectest loads OpenAPI-generated tests and hand-written suites into the same run and dependency graph. A hand-written suite can `dependsOn` a spec-generated `operationId`, and duplicate `operationId`s across the two sources fail fast via the same uniqueness check described in [Core Concepts](/docs/introduction/concepts/).

## Multiple examples per operation

If a request body, parameter, or response defines an `examples` map (rather than a single `example`), Spectest generates one test per entry. Each generated test's `operationId` becomes `${operationId}+${exampleKey}` and its name gets a ` — ${exampleKey}` suffix.

The expected response for a given request example is resolved in order:

1. `x-spectest.status` declared on that example.
2. A response example under a documented non-2xx status sharing the same key as the request example.
3. The lowest documented `2xx` status.

An operation with no `examples` map behaves exactly as before: a single generated test with an unsuffixed `operationId`.

## `x-spectest` vendor extension

A single vendor extension, allowed on an operation and on an individual entry inside an `examples` map — an example-level value overrides the operation-level one:

```yaml
x-spectest:
  status: 400
  tags: [slow, real-backend]
  skip: true
  skipReason: "hits real registrar, run manually"
  phase: setup | main | teardown
  dependsOn: [operationId, "otherOperationId+exampleKey"]
  beforeSend: hookName        # looked up in cfg.openapiHooks
  postTest: hookName
  security: none | variantName
  generate:                   # dynamic values, see below
    orderId: uuid
    "product.domainName": shortId
```

## Named hook registry

Add `openapiHooks` to `spectest.config.js` to give `x-spectest.beforeSend`/`postTest` something to resolve against:

```js
export default {
  openapiHooks: {
    extractOneTimeToken: {
      postTest: async (res, state, ctx) => { /* scrape ctx.logs, stash in state */ },
    },
  },
};
```

A name referenced by `x-spectest` that isn't in `openapiHooks` generates a skipped test with a reason — the same way a missing `openapiAuth` hook does. It never crashes the loader.

## Dynamic example values

Use `{{uuid}}`, `{{timestamp}}`, or `{{shortId}}` as a literal example string to get a fresh value resolved once per generated test (stable across `repeat`/`bombard` reruns of that same test). For object bodies, `x-spectest.generate` is the path-keyed alternative — list which fields need freshness per run instead of rewriting the example. Both are opt-in; Spectest never synthesizes data for a property that has no example.

## Explicit non-default auth cases

`openapiAuth` entries can be either a single hook or a named-variant map, letting you deliberately generate a test with missing or expired credentials:

```js
export default {
  openapiAuth: {
    session: {
      valid: async (ctx) => ({ headers: { Cookie: 'session=...' } }),
      expired: async (ctx) => ({ headers: { Cookie: 'session=expired' } }),
      missing: async () => ({}),
    },
  },
};
```

Set `x-spectest.security: none` to bypass security application entirely for an example, or `x-spectest.security: expired` (etc.) to pick a variant. With no override, a variant map defaults to its `valid` entry.

## Native `links` for chaining

Standard OpenAPI `responses.<status>.links` are honored for simple data-passing chains — e.g. register something, then fetch it using the `orderId` from the first response. When operation B has an unresolved parameter or request body and an earlier operation A declares a link targeting B by `operationId`, Spectest auto-generates `dependsOn: [A]` plus a `beforeSend` that reads the linked value from `state.completedCases[A]` via the link's runtime expression (`$response.body#/pointer` or `$response.header.<name>`). This only applies when A itself resolves to exactly one generated test; if A was split by multiple examples, use `x-spectest.dependsOn` plus `beforeSend`/hooks instead.

## Contract coverage reporting

```bash
npx spectest --openapi ./openapi.yaml --coverage-report
npx spectest --openapi ./openapi.yaml --coverage-report --coverage-report-file ./coverage.txt
```

After the run, Spectest prints one line per spec operation: `generated & passed`, `generated & failed`, `generated & skipped (<reason>)`, `covered by hand-written test <operationId>` (when a hand-written suite's request matches that operation's method/path but no generated test ran for it), or `uncovered`.

## Scaffolding editable test files

```bash
npx spectest generate openapi-tests --openapi ./openapi.yaml --output ./test
```

Writes a `.spectest.js` file with the same generated tests Spectest would run in-memory, as an editable starting point for suites that will grow real `beforeSend`/`postTest` logic by hand. Anything expressible this way is already expressible via direct `--openapi` loading — this is a convenience/onboarding command, not a coverage unlock.

## Selecting a server

If the document declares multiple `servers` entries, pick one with `openapiServer` (a URL or an index) in `spectest.config.js`, or `--openapi-server` on the command line. Otherwise pass `--base-url` as usual to override the server entirely.

Related: [CLI](/docs/api-references/cli/) for the full flag reference, [Test Case](/docs/api-references/test-case/) for the fields `x-spectest` maps onto.
