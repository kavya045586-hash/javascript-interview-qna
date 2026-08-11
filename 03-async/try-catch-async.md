## Q: Error Handling with try/catch in Async Functions

**Answer:**

Since `await` can "fail" (if the Promise it's waiting on rejects), you
handle errors using `try/catch` — the async/await equivalent of `.catch()`
in Promise chains.

**Basic pattern:**

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

**Handling multiple awaits in one try block:**

```js
async function loadData() {
  try {
    const user = await fetchUser();
    const posts = await fetchPosts(user.id);
    const comments = await fetchComments(posts[0].id);
    console.log(comments);
  } catch (error) {
    // catches an error from ANY of the awaits above — whichever fails first
    console.log("Something failed:", error);
  }
}
```

**What happens if you DON'T use try/catch?**

```js
async function loadUser() {
  const user = await fetchUser(); // if this rejects, no catch here...
  console.log(user);
}

loadUser(); // the returned Promise rejects, but nothing is handling it
// Result: "Uncaught (in promise) Error: ..." — Unhandled Promise Rejection
```

**Fix — catch it at the call site instead:**

```js
loadUser().catch(error => console.log("Caught:", error));
```

**Using `finally` for cleanup (runs regardless of success/failure):**

```js
async function loadUser() {
  try {
    const user = await fetchUser();
    console.log(user);
  } catch (error) {
    console.log("Error:", error);
  } finally {
    console.log("Done loading — hide spinner, etc.");
  }
}
```

**Follow-up questions interviewers ask:**

- What happens if an await inside a try block fails and there's no catch?
- Can you use `.catch()` on an async function call instead of try/catch inside it? (Yes — since async functions return Promises, you can chain `.catch()` on the call itself)
