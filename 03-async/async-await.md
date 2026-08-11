## Q: What is Async/Await?

**Answer:**

`async/await` is syntactic sugar built on top of Promises — it lets you
write asynchronous code that *looks* synchronous, making it much easier
to read than `.then()` chains.

**Basic syntax:**

```js
async function getData() {
  return "hello";
}
getData().then(val => console.log(val)); // "hello" — proves async functions still return a Promise
```

An `async` function ALWAYS returns a Promise — even if you just `return`
a plain value, JavaScript automatically wraps it in a resolved Promise.

**Using `await`:**

`await` pauses execution of the async function until the Promise settles,
then unwraps the resolved value.

```js
async function loadUser() {
  const user = await fetchUser();   // pauses here until fetchUser() resolves
  console.log(user);                 // runs only after the Promise resolves
}
```

**Rewriting a Promise chain with async/await:**

```js
// Promise chain version
function loadData() {
  return fetchUser()
    .then(user => fetchPosts(user.id))
    .then(posts => console.log(posts));
}

// async/await version — much more readable
async function loadData() {
  const user = await fetchUser();
  const posts = await fetchPosts(user.id);
  console.log(posts);
}
```

**Important: `await` only pauses the CURRENT async function, not the whole program**

```js
console.log("Start");

async function loadData() {
  console.log("Before await");
  await new Promise(resolve => setTimeout(resolve, 1000));
  console.log("After await");
}

loadData();
console.log("End");

// Output order:
// Start
// Before await
// End              ← rest of the program keeps running while loadData() waits
// After await        (1 second later)
```

**Follow-up questions interviewers ask:**

- Is async/await a completely different mechanism from Promises? (No — it's built on top of Promises, just cleaner syntax)
- Does `await` block the entire JavaScript engine? (No — only pauses within that specific async function)
