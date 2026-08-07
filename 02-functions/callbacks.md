## Q: What is a Callback Function?

**Answer:**

A callback is simply **a function passed as an argument to another
function**, to be executed later — either immediately (synchronous) or
after some operation completes (asynchronous).

Every callback is passed to a Higher Order Function — so callbacks and
HOFs are closely related concepts.

---

**1. Synchronous callback (runs immediately, in order)**

```js
function processUserInput(callback) {
  const name = "Kavya";
  callback(name);
}

processUserInput(function (name) {
  console.log("Hello, " + name);
});
// "Hello, Kavya"
```

**Step by step:**
1. `processUserInput` is called with a function as its argument
2. Inside, it defines `name = "Kavya"`
3. It calls `callback(name)` — this runs the function you passed in, right away
4. Output: `"Hello, Kavya"` — everything happens in order, no delay

---

**2. Asynchronous callback (runs later, after a delay/operation)**

```js
console.log("Start");

setTimeout(function () {
  console.log("This runs later");
}, 2000);

console.log("End");
```

**Step by step:**
1. `"Start"` logs immediately
2. `setTimeout` is called — it schedules the callback to run after 2
   seconds, but does NOT pause the program to wait
3. JavaScript moves on immediately to the next line
4. `"End"` logs immediately (before the timeout finishes)
5. After 2 seconds pass, `"This runs later"` finally logs

**Output order:**


This is the core reason callbacks matter — they let JavaScript handle
things that take time (API calls, timers, file reads) **without freezing**
the rest of the program while waiting.

---

**3. ❌ Problem: "Callback Hell"**

```js
getUser(1, function (user) {
  getPosts(user.id, function (posts) {
    getComments(posts[0].id, function (comments) {
      console.log(comments); // deeply nested — hard to read/maintain
    });
  });
});
```

**Why this is a problem:**
- Each callback depends on the result of the one before it, so they nest
  deeper and deeper
- Hard to read (the "pyramid of doom" shape)
- Hard to handle errors — you'd need a separate error check at every level
- Hard to maintain or modify later

**✅ Solution (preview):** Promises and `async/await` were introduced
specifically to flatten this nested structure into readable, sequential
code. This is covered next, in `03-async`.

```js
// Same logic, with async/await (covered in 03-async)
async function loadData() {
  const user = await getUser(1);
  const posts = await getPosts(user.id);
  const comments = await getComments(posts[0].id);
  console.log(comments);
}
```

---

**Quick summary table:**

| Type | Runs | Example |
|---|---|---|
| Synchronous callback | Immediately, in order | `processUserInput(callback)` |
| Asynchronous callback | Later, after some operation completes | `setTimeout`, API calls |
| Callback Hell | Nested callbacks, hard to maintain | Fixed by Promises/async-await |

**Follow-up questions interviewers ask:**

- What's the difference between a synchronous and asynchronous callback?
- Why is deeply nested callback code considered a problem?
- What replaced callback hell in modern JavaScript? (Promises and async/await)
