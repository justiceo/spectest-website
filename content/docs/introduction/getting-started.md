---
title: "Getting Started"
weight: 1
---

Spectest ships as an npm package and can be executed with **npx**. The quick steps below mirror the sample in the repository README.

## Installation

```bash
npm install --save-dev spectest
```

You can also run it directly with `npx` when you only need the CLI.

## Configure the test environment

Create a `spectest.config.js` file at the root of your project:

```js
// spectest.config.js
export default {
  baseUrl: 'https://jsonplaceholder.typicode.com',
  testDir: './test',
  filePattern: '\\.spectest\\.',
};
```

For real projects `baseUrl` should point at your API server, for example `http://localhost:3000`.

## Write your first tests

Create `test/jsonpayload.spectest.js`:

```js
const tests = [
  { name: 'Fetch TODO 1', endpoint: '/todos/1' },
  {
    name: 'Create a post',
    endpoint: '/posts',
    request: {
      method: 'POST',
      headers: { 'Content-Type': 'application/json; charset=UTF-8' },
      body: { title: 'foo', body: 'bar', userId: 1 },
    },
    response: {
      status: 201,
      json: { id: 101, title: 'foo', body: 'bar', userId: 1 },
    },
  },
];
export default tests;
```

Run the tests:

```bash
npx spectest
```

The CLI prints a summary similar to:

```text
📊 Test Summary:
 [✅] Fetch TODO 1 (53ms)
 [✅] Create a post (108ms)

✅ 2/2 tests passed!
📋 Server logs captured: 0
⏱️ Latency: min 53ms; avg 80ms; max 108ms
```

{{< hint info >}}
You can run a single suite with `npx spectest jsonpayload.spectest.js` or override `baseUrl` on the command line.
{{< /hint >}}
