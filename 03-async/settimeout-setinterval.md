## Q: setTimeout vs setInterval

**Answer:**

- **`setTimeout(fn, delay)`** — runs `fn` ONCE, after the specified delay
- **`setInterval(fn, delay)`** — runs `fn` REPEATEDLY, every `delay`
  milliseconds, until explicitly stopped

**setTimeout example:**

```js
console.log("Start");
setTimeout(() => {
  console.log("Runs once, after 2 seconds");
}, 2000);
console.log("End");
```

**setInterval example:**

```js
let count = 0;
const intervalId = setInterval(() => {
  count++;
  console.log("Tick", count);
  if (count === 5) {
    clearInterval(intervalId); // MUST manually stop it, or it runs forever
  }
}, 1000);
```

**Why `clearInterval()` matters — the common mistake:**

If you forget to call `clearInterval()`, the interval keeps running
forever in the background, even after it's no longer needed — wasting
memory/CPU, and potentially trying to update something (like a UI element)
that no longer exists. This is a common source of memory leaks in real
applications.

```js
// ❌ Problem — never cleared
setInterval(() => console.log("still running..."), 1000);
// runs forever, even if you don't need it anymore

// ✅ Solution — clear it when done
const id = setInterval(() => console.log("running"), 1000);
setTimeout(() => clearInterval(id), 5000); // stop after 5 seconds
```

**Does `setTimeout(fn, 0)` run immediately?**

No — even with a 0ms delay, it still goes into the macrotask queue and
must wait for the call stack to be empty AND the microtask queue to fully
drain first (covered in the previous topic).

```js
console.log("1");
setTimeout(() => console.log("2"), 0);
console.log("3");
// Output: 1, 3, 2 — NOT 1, 2, 3
```

**Quick comparison table:**

| | setTimeout | setInterval |
|---|---|---|
| Runs | Once | Repeatedly |
| Needs manual stop? | No | Yes — `clearInterval()` |
| Common use | Delays, timeouts, deferred actions | Polling, clocks, repeating animations |

**Follow-up questions interviewers ask:**

- What happens if you never call clearInterval()?
- Can you achieve setInterval-like behavior using setTimeout? (Yes — by having the setTimeout callback call itself recursively; this is actually often preferred since it waits for each execution to finish before scheduling the next one)
