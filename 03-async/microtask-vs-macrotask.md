## Q: Microtask Queue vs Macrotask Queue

**Answer:**

Once an async operation finishes, its callback doesn't go straight onto
the call stack — it goes into one of two queues, and the Event Loop
decides the order they're processed in.

**Macrotask Queue** (also called the "task queue"):
- `setTimeout`, `setInterval`
- UI rendering
- I/O operations

**Microtask Queue:**
- Promise callbacks (`.then()`, `.catch()`, `.finally()`)
- `async/await` continuations
- `queueMicrotask()`

**The critical rule:** After every SINGLE macrotask finishes, the Event
Loop fully drains the ENTIRE microtask queue before picking up the next
macrotask — even if a macrotask was scheduled first.

**Example:**

```js
console.log("1");

setTimeout(() => console.log("2"), 0);  // macrotask

Promise.resolve().then(() => console.log("3"));  // microtask

console.log("4");
```

**Step by step:**
1. `console.log("1")` — synchronous, runs immediately → `1`
2. `setTimeout(...)` — scheduled, goes to the macrotask queue (waits)
3. `Promise.resolve().then(...)` — scheduled, goes to the microtask queue (waits)
4. `console.log("4")` — synchronous, runs immediately → `4`
5. Call stack is now empty — Event Loop checks the MICROTASK queue first → runs `3`
6. Microtask queue is now empty — Event Loop finally checks the macrotask queue → runs `2`

**Output:** `1`, `4`, `3`, `2`

**A trickier example — chained microtasks:**

```js
setTimeout(() => console.log("timeout"), 0);

Promise.resolve()
  .then(() => console.log("promise 1"))
  .then(() => console.log("promise 2"));

console.log("sync");
```

**Output:** `sync`, `promise 1`, `promise 2`, `timeout`

Why: BOTH `.then()` calls are microtasks, and the entire microtask queue
drains completely (including any new microtasks added during draining)
before the event loop ever looks at the macrotask queue.

**Follow-up questions interviewers ask:**

- Why do microtasks always run before macrotasks, even if the macrotask was scheduled first?
- Does `setTimeout(fn, 0)` ever run before a Promise's `.then()`? (No — never, regardless of delay value, because microtasks always fully drain first)
