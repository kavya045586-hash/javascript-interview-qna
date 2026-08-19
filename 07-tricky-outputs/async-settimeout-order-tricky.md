## Tricky Outputs: Async/Await + setTimeout Ordering

**Q1.** What does this print?
```js
console.log("A");
setTimeout(() => console.log("B"), 0);
Promise.resolve().then(() => console.log("C"));
console.log("D");
```
<details><summary>Answer</summary>
A, D, C, B — sync code runs first (A, D). Then the microtask queue drains
(C, from the Promise). Only then does the macrotask queue run (B, from
setTimeout), even with a 0ms delay.
</details>

---

**Q2.** What does this print?
```js
async function test() {
  console.log("1");
  await null;
  console.log("2");
}
console.log("start");
test();
console.log("end");
```
<details><summary>Answer</summary>
start, 1, end, 2 — test() runs synchronously up to await (logging "1"),
then pauses and yields control back to the main program, which logs
"end". Only after the main synchronous code finishes does "2" run, since
await always defers to the microtask queue, even on `null`.
</details>

---

**Q3.** What does this print?
```js
setTimeout(() => console.log("timeout"), 0);
Promise.resolve()
  .then(() => console.log("p1"))
  .then(() => console.log("p2"));
console.log("sync");
```
<details><summary>Answer</summary>
sync, p1, p2, timeout — ALL microtasks (both .then() calls) drain
completely before the event loop even looks at the macrotask queue.
</details>
