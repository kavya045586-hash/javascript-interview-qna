## Q: What is the Call Stack, and what is the Event Loop?

**Answer:**

**Call Stack:** A structure that tracks which function is currently
executing. When a function is called, it's "pushed" onto the stack; when
it returns/finishes, it's "popped" off. JavaScript can only run whatever
is on TOP of the stack — this is why JS is single-threaded (one stack, one
thing at a time).

```js
function a() {
  b();
}
function b() {
  console.log("in b");
}
a();

// Call stack over time:
// [a]
// [a, b]
// [a]         (b finishes, pops off)
// []           (a finishes, pops off)
```

**The problem this creates:** if JS can only do one thing at a time, how
does it handle slow operations (API calls, timers) without freezing?

**Event Loop:** The mechanism that solves this. Slow operations (like
`setTimeout`, `fetch`) are handed off to the browser's Web APIs, which run
OUTSIDE the main JS thread. Once they finish, their callbacks go into a
queue. The Event Loop's job: continuously check — *"Is the call stack
empty? If yes, take the next callback from the queue and push it onto the
stack to run."*

**Visualizing the flow:**

```js
console.log("1");

setTimeout(() => console.log("2"), 0);

console.log("3");

// Output: 1, 3, 2
```

**Step by step:**
1. `console.log("1")` runs immediately — call stack: `[log]` → pops → `[]`
2. `setTimeout(...)` is handed off to the Web API — NOT run immediately, even with 0ms delay
3. `console.log("3")` runs immediately — call stack: `[log]` → pops → `[]`
4. Only now, with the call stack empty, does the Event Loop pick up the setTimeout's callback from the queue and run it
5. Output: `1`, `3`, `2`

**Follow-up questions interviewers ask:**

- Why is JavaScript called "single-threaded" if it can handle async operations?
- Does `setTimeout(fn, 0)` run immediately? (No — it still has to wait for the call stack to be empty first, covered further in microtask vs macrotask)
