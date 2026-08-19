## Q: Memoization

**Answer:**

Memoization is a performance optimization technique that caches the
results of expensive function calls, so repeated calls with the SAME
arguments return the cached result instantly instead of recomputing.

**Basic implementation:**

```js
function memoize(fn) {
  const cache = {};
  return function (...args) {
    const key = JSON.stringify(args);
    if (cache[key]) {
      console.log("From cache");
      return cache[key];
    }
    const result = fn(...args);
    cache[key] = result;
    return result;
  };
}

function slowSquare(n) {
  for (let i = 0; i < 1e9; i++) {} // simulate slow computation
  return n * n;
}

const fastSquare = memoize(slowSquare);
console.log(fastSquare(5)); // slow the first time
console.log(fastSquare(5)); // instant — "From cache"
```

**Real use case — memoized Fibonacci (dramatically faster than plain recursion):**

```js
function fib(n, memo = {}) {
  if (n in memo) return memo[n];
  if (n <= 1) return n;
  memo[n] = fib(n - 1, memo) + fib(n - 2, memo);
  return memo[n];
}
console.log(fib(40)); // fast — without memoization, this would take a very long time
```

**Trade-off:** memoization uses more MEMORY to save on computation TIME —
a classic space-vs-time trade-off, and it only helps for **pure
functions** (same input always gives same output).

**Follow-up questions interviewers ask:**

- Why does memoization only work for pure functions? (If a function's output depends on external/changing state, caching by arguments alone would return stale/incorrect results)
- What's the trade-off memoization makes? (More memory usage in exchange for faster repeated calls)
