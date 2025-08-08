---
title: "Helpers"
weight: 5
---

The `spectest/helpers` module contains utilities for modifying entire test suites. These functions mutate the cases you pass in and return the updated array.

```js
import { focus, delay } from 'spectest/helpers';

const suite = [
  { name: 'Get todo list', endpoint: '/todos' },
  { name: 'Fetch TODO 1', endpoint: '/todos/1' },
];
export default focus(delay(suite, 500));
```

### Available helpers

- `composeBeforeSend(...fns)` – combine multiple `beforeSend` functions
- `composePostTest(...fns)` – combine multiple `postTest` functions
- `delay(tests, ms)` – add a delay before each case runs
- `focus(tests)` – mark tests as focused
- `repeat(tests, count)` – run tests sequentially multiple times
- `bombard(tests, count)` – launch multiple concurrent runs
- `skip(tests)` – skip the provided cases

{{< hint info >}}
Helpers are optional but help reduce repetition in large suites.
{{< /hint >}}
