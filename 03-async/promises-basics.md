## Q: What is a Promise?

**Answer:**

A Promise is an object representing the eventual completion (or failure)
of an asynchronous operation. It acts as a placeholder for a value that
isn't available yet, but will be at some point in the future.

**The three states of a Promise:**

1. **Pending** — initial state, operation not finished yet
2. **Fulfilled** — operation completed successfully
3. **Rejected** — operation failed

Once a Promise moves to Fulfilled or Rejected, it's permanently "settled"
— it can never change state again.

**Creating a Promise:**

```js
const myPromise = new Promise((resolve, reject) => {
  const success = true;

  if (success) {
    resolve("Operation succeeded!");
  } else {
    reject("Operation failed!");
  }
});
```

**Consuming a Promise:**

```js
myPromise
  .then(result => console.log(result))   // runs if resolved
  .catch(error => console.log(error))     // runs if rejected
  .finally(() => console.log("Done"));     // always runs, regardless of outcome
```

**Chaining Promises:**

```js
fetchUser()
  .then(user => fetchPosts(user.id))     // each .then() returns a NEW promise
  .then(posts => console.log(posts))
  .catch(error => console.log(error));    // catches errors from ANY step above
```

**Follow-up questions interviewers ask:**

- What are the three states of a Promise?
- What does `.then()` actually return? (A new Promise — this is what makes chaining possible)
- What happens if you forget `.catch()` and a Promise rejects? (Unhandled Promise Rejection — the error is silently swallowed or logged as a warning)
