## Q: Promise.all, Promise.race, and Promise.allSettled

**Answer:**

These are static methods for handling **multiple Promises at once**, but
each behaves differently.

**1. `Promise.all()` — waits for ALL to succeed, fails fast on any rejection**

```js
Promise.all([
  fetchUser(),
  fetchPosts(),
  fetchComments()
])
  .then(([user, posts, comments]) => {
    console.log(user, posts, comments); // all succeeded
  })
  .catch(error => {
    console.log(error); // if even ONE fails, this runs immediately, other results are lost
  });
```

**2. `Promise.race()` — resolves/rejects with whichever settles FIRST**

```js
Promise.race([
  fetch("/api/data"),
  new Promise((_, reject) => setTimeout(() => reject("Timeout!"), 5000))
])
  .then(result => console.log(result))
  .catch(error => console.log(error)); // useful for implementing timeouts
```

**3. `Promise.allSettled()` — waits for ALL to finish, regardless of success/failure**

```js
Promise.allSettled([
  Promise.resolve(1),
  Promise.reject("fail"),
  Promise.resolve(3)
]).then(results => {
  console.log(results);
  // [
  //   { status: "fulfilled", value: 1 },
  //   { status: "rejected", reason: "fail" },
  //   { status: "fulfilled", value: 3 }
  // ]
});
```

**Quick comparison table:**

| Method | Waits for | Fails fast? | Use case |
|---|---|---|---|
| `Promise.all()` | All to succeed | Yes — rejects on first failure | You need ALL results, and any failure means the whole thing fails |
| `Promise.race()` | Whichever finishes first | N/A — first settled wins | Implementing timeouts, "fastest response wins" scenarios |
| `Promise.allSettled()` | All to finish (success or fail) | No — never rejects | You want results from everything, even if some fail |

**Follow-up questions interviewers ask:**

- When would you choose `allSettled` over `all`?
- How would you implement an API timeout using `Promise.race`?
