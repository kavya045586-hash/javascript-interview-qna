# Extra Topics & Deep-Dive Quiz — Async JavaScript

Every question here explains the underlying concept, not just tests
memory. Cover the answer with your hand and try to explain it in your own
words before checking.

---

## Section 1: Callbacks & Callback Hell

**Q1. What is a callback, and why does JavaScript need them?**
<details><summary>Answer</summary>
A callback is a function passed as an argument to run later. JavaScript is
single-threaded — it can only do one thing at a time. Callbacks let it
start a slow task (API call, timer, file read) and move on immediately,
running the callback only once that task finishes, instead of freezing
everything while waiting.
</details>

---

**Q2. What makes callback hell specifically a "hell"?**
<details><summary>Answer</summary>
Not the nesting alone — it's that each callback depends on the previous
one's result, so they must nest deeper and deeper. This makes code hard to
read (pyramid shape), hard to add error handling to at every level, and
hard to modify later without breaking the chain.
</details>

---

**Q3. Can callback hell happen even without deep nesting?**
<details><summary>Answer</summary>
Yes — "hell" really means loss of control over error handling and
execution order, not just visual nesting. Poorly structured async code
(inconsistent error handling, mixed sync/async logic) can be just as
problematic even if flattened.
</details>

---

## Section 2: Promises

**Q4. What problem do Promises solve that callbacks don't?**
<details><summary>Answer</summary>
Promises give async code a standard, chainable structure with built-in
error handling (`.catch()`), instead of manually checking for errors at
every nested level. They also avoid the "pyramid of doom" by allowing
`.then()` chains instead of nested functions.
</details>

---

**Q5. What are the three states of a Promise, and can a Promise change state more than once?**
<details><summary>Answer</summary>
Pending (initial state), Fulfilled (succeeded), Rejected (failed). Once a
Promise moves to Fulfilled or Rejected, it's permanently "settled" — it can
never change state again, even if you try to resolve/reject it a second
time.
</details>

---

**Q6. What does `.then()` actually return?**
<details><summary>Answer</summary>
`.then()` always returns a NEW Promise — this is what makes chaining
possible. Whatever value you `return` inside `.then()` becomes the
resolved value of that new Promise, available to the next `.then()` in the
chain.
</details>

```js
Promise.resolve(1)
  .then(val => val + 1)   // returns a new Promise resolving to 2
  .then(val => console.log(val)); // 2
```

---

**Q7. What happens if you forget a `.catch()` on a Promise chain?**
<details><summary>Answer</summary>
If any Promise in the chain rejects and there's no `.catch()`, you get an
"Unhandled Promise Rejection" — the error is silently swallowed or logged
as a warning depending on the environment, and your program continues in
an inconsistent state without knowing something failed.
</details>

---

**Q8. Can you chain `.then()` after a `.catch()`? What happens?**
<details><summary>Answer</summary>
Yes. Once a `.catch()` handles an error, the chain is considered "recovered"
— execution continues normally into the next `.then()`, unless the catch
block itself throws a new error.
</details>

```js
Promise.reject("error")
  .catch(err => {
    console.log("Caught:", err);
    return "recovered";
  })
  .then(val => console.log(val)); // "Caught: error" then "recovered"
```

---

## Section 3: Promise.all / race / allSettled

**Q9. What's the core behavioral difference between `Promise.all` and `Promise.allSettled`?**
<details><summary>Answer</summary>
`Promise.all` fails FAST — if even one promise rejects, the whole thing
immediately rejects, and you lose the results of the other promises (even
successful ones). `Promise.allSettled` waits for ALL promises to finish
regardless of success/failure, then gives you an array describing each
one's outcome individually.
</details>

```js
Promise.all([Promise.resolve(1), Promise.reject("fail"), Promise.resolve(3)])
  .catch(err => console.log(err)); // "fail" — stops immediately, other results lost

Promise.allSettled([Promise.resolve(1), Promise.reject("fail"), Promise.resolve(3)])
  .then(results => console.log(results));
  // [{status:"fulfilled", value:1}, {status:"rejected", reason:"fail"}, {status:"fulfilled", value:3}]
```

---

**Q10. When would you use `Promise.race` in a real scenario?**
<details><summary>Answer</summary>
Implementing a timeout for a slow API call — race the actual API call
against a Promise that rejects after N seconds. Whichever settles first
"wins," so if the API takes too long, the timeout Promise rejects first
and you can show an error instead of waiting forever.
</details>

```js
Promise.race([
  fetch("/api/data"),
  new Promise((_, reject) => setTimeout(() => reject("Timeout!"), 5000))
]);
```

---

**Q11. What does `Promise.any` do, and how is it different from `Promise.race`?**
<details><summary>Answer</summary>
`Promise.any` resolves as soon as ANY promise succeeds (ignoring
rejections, unless ALL reject). `Promise.race` resolves/rejects on
whichever settles FIRST, whether that's a success or a failure. So `race`
cares about speed only; `any` cares about getting a success specifically.
</details>

---

## Section 4: Async/Await

**Q12. Is `async/await` a completely different mechanism from Promises?**
<details><summary>Answer</summary>
No — async/await is "syntactic sugar" built on top of Promises. An `async`
function always returns a Promise, and `await` simply pauses execution
until that Promise settles, making asynchronous code visually look
synchronous.
</details>

```js
async function getData() {
  return "hello";
}
getData().then(val => console.log(val)); // "hello" — proves it's still a Promise
```

---

**Q13. What happens if you use `await` outside of an `async` function?**
<details><summary>Answer</summary>
`SyntaxError: await is only valid in async functions` — `await` can only
be used inside a function explicitly marked `async` (top-level await in
modules is a newer exception).
</details>

---

**Q14. Does `await` block the entire JavaScript engine while waiting?**
<details><summary>Answer</summary>
No — it only pauses execution WITHIN that specific async function. The
rest of the program (other code, event handlers, etc.) continues running
normally elsewhere. This is a common misconception.
</details>

---

## Section 5: try/catch with Async

**Q15. How do you handle errors in async/await code?**
<details><summary>Answer</summary>
Wrap the `await` calls in a `try/catch` block — this is the async/await
equivalent of `.catch()` in Promise chains.
</details>

```js
async function loadUser() {
  try {
    const user = await fetchUser();
    console.log(user);
  } catch (error) {
    console.log("Failed to load user:", error);
  }
}
```

---

**Q16. What happens if you don't wrap `await` in try/catch and the Promise rejects?**
<details><summary>Answer</summary>
The async function itself returns a rejected Promise, and if nothing
catches it (neither try/catch nor a `.catch()` on the function call), you
get an unhandled rejection error, same as with plain Promises.
</details>

---

## Section 6: Call Stack & Event Loop

**Q17. What is the Call Stack?**
<details><summary>Answer</summary>
A structure that tracks which function is currently running. When a
function is called, it's "pushed" onto the stack; when it finishes, it's
"popped" off. JavaScript executes whatever is on top of the stack — this
is what makes JS single-threaded (one call stack, one thing at a time).
</details>

---

**Q18. If JavaScript is single-threaded, how does it handle async operations at all?**
<details><summary>Answer</summary>
The JS engine itself is single-threaded, but the browser (or Node.js)
provides separate systems — Web APIs (like `setTimeout`, `fetch`) — that
run OUTSIDE the main JS thread. When those finish, their callbacks are
queued up and the Event Loop pushes them back onto the call stack once
it's empty.
</details>

---

**Q19. What is the Event Loop's actual job, in one sentence?**
<details><summary>Answer</summary>
It continuously checks: "Is the call stack empty? If yes, take the next
task from the queue and push it onto the stack to run."
</details>

---

## Section 7: Microtask vs Macrotask Queue

**Q20. What's the difference between the microtask queue and the macrotask queue?**
<details><summary>Answer</summary>
Microtasks (Promise callbacks, `.then()`, `queueMicrotask`) always run
BEFORE macrotasks (`setTimeout`, `setInterval`, UI rendering), even if a
macrotask was scheduled first. After every single macrotask finishes, the
event loop fully drains the ENTIRE microtask queue before picking up the
next macrotask.
</details>

---

**Q21. What does this code print, and why?**
```js
console.log("1");
setTimeout(() => console.log("2"), 0);
Promise.resolve().then(() => console.log("3"));
console.log("4");
```
<details><summary>Answer</summary>
1, 4, 3, 2 — synchronous code (1, 4) runs first; then the microtask queue
(Promise's .then → 3) drains completely; only then does the macrotask
queue run (setTimeout → 2), even though its delay was 0ms.
</details>

---

## Section 8: setTimeout vs setInterval

**Q22. What's the core difference between setTimeout and setInterval?**
<details><summary>Answer</summary>
`setTimeout` runs a callback ONCE after a specified delay. `setInterval`
runs a callback REPEATEDLY, every specified delay, until explicitly
stopped with `clearInterval()`.
</details>

---

**Q23. Does `setTimeout(fn, 0)` run immediately?**
<details><summary>Answer</summary>
No — even with a 0ms delay, it still goes into the macrotask queue and has
to wait for the call stack to be empty AND the microtask queue to fully
drain first. So it never truly runs "immediately" if there's other
synchronous or microtask code ahead of it.
</details>

---

**Q24. Why can setInterval cause performance issues if not cleared?**
<details><summary>Answer</summary>
If a component/page is removed but `clearInterval()` was never called, the
interval keeps running forever in the background — wasting memory/CPU and
potentially trying to update something that no longer exists (a common
source of memory leaks in real apps).
</details>

---

## Bonus: Trace-the-Output Challenge Questions

**Q25.** What does this print, in order?
```js
console.log("A");

async function test() {
  console.log("B");
  await null;
  console.log("C");
}

test();
console.log("D");
```
<details><summary>Answer</summary>
A, B, D, C — "A" logs first. test() runs synchronously up to the `await`
(logging "B"), then pauses and returns control back to the main program,
which logs "D". Only after the main synchronous code finishes does the
paused async function resume, logging "C" (since await, even on `null`,
still defers to the microtask queue).
</details>

---

**Q26.** What does this print?
```js
setTimeout(() => console.log("timeout"), 0);
Promise.resolve()
  .then(() => console.log("promise 1"))
  .then(() => console.log("promise 2"));
console.log("sync");
```
<details><summary>Answer</summary>
sync, promise 1, promise 2, timeout — sync code runs first, then the ENTIRE
microtask queue drains (both .then() calls happen before moving on), and
only then does the macrotask (setTimeout) finally run.
</details>
