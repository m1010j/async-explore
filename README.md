# async-explorations-cli

`async-explorations-cli` is a command-line tool to benchmark asynchronous recursive functions in Node.js. It is a companion to [Explorations in Asynchronicity](https://github.com/m1010j/async-explorations).

Benchmarks are run using experimental [worker threads](https://nodejs.org/api/worker_threads.html). As a result, `async-explorations-cli` requires Node.js v10.5.0 or higher.

### Overview

Users can benchmark the following six functions:

```javascript
function syncFib(n) {
  if (n <= 0) return 0;
  if (n === 1) return 1;

  return syncFib(n - 1) + syncFib(n - 2);
}
```

```javascript
async function asyncFib(n) {
  if (n <= 0) return 0;
  if (n === 1) return 1;

  const prevValues = await Promise.all([asyncFib(n - 1), asyncFib(n - 2)]);

  return prevValues[0] + prevValues[1];
}
```

```javascript
function syncBusyFib(n) {
  if (n <= 0) return 0;
  if (n === 1) return 1;

  const superBig = n ** 9;
  for (let i = 0; i < superBig; i++) {
    i;
  }

  return syncBusyFib(n - 1) + syncBusyFib(n - 2);
}
```

```javascript
async function asyncBusyFib(n) {
  if (n <= 0) return 0;
  if (n === 1) return 1;

  const superBig = n ** 9;
  for (let i = 0; i < superBig; i++) {
    i;
  }

  const prevValues = await Promise.all([
    asyncBusyFib(n - 1),
    asyncBusyFib(n - 2),
  ]);

  return prevValues[0] + prevValues[1];
}
```

```javascript
function syncMemoFib(n, memo = {}) {
  if (n <= 0) return 0;
  if (n === 1) return 1;
  if (memo[n]) return memo[n];

  const first = syncMemoFib(n - 1, memo);
  const second = syncMemoFib(n - 2, memo);
  memo[n] = first + second;

  return memo[n];
}
```

```javascript
async function asyncMemoFib(n, memo = {}) {
  if (n <= 0) return 0;
  if (n === 1) return 1;
  if (memo[n]) return memo[n];

  const prevValues = await Promise.all([
    asyncMemoFib(n - 1, memo),
    asyncMemoFib(n - 2, memo),
  ]);

  memo[n] = prevValues[0] + prevValues[1];
  return memo[n];
}
```

### Installation

```bash
npm install -g async-explorations-cli
```

### How to use

The command-line-tool is called `async-explore`. It requires two arguments:
1. The Fibonacci function to benchmark.
    - This must be one of `syncFib`, `asyncFib`, `syncBusyFib`, `asyncBusyFib`, `syncMemoFib`, and `asyncMemoFib`.
1. A positive integer that is the arguemnt to the Fibonacci function.

To turn off the use of worker threads, run `async-explore` with an optional `--mode=no-worker` or `-nw` flag.

**Example**

![asyncFib 28 preview gif](https://raw.githubusercontent.com/m1010j/async-explorations-cli/master/media/preview.gif)


### [Contributing](./CONTRIBUTING.md)

### [License](./LICENSE)