## Q: What is Callback Hell? (Deep Dive)

**Answer:**

Callback Hell happens when multiple asynchronous operations depend on each
other, so their callbacks get nested deeper and deeper — creating code
that's hard to read, debug, and maintain. It's often called the
"Pyramid of Doom" because of its triangular shape.

**Example:**

```js
getUser(1, function (user) {
  getPosts(user.id, function (posts) {
    getComments(posts[0].id, function (comments) {
      getLikes(comments[0].id, function (likes) {
        console.log(likes); // 4 levels deep — hard to follow
      });
    });
  });
});
```

**Why this is a real problem, not just an ugly-code issue:**

1. **Error handling gets messy** — you'd need a separate error check at
   every single level, and errors from inner callbacks don't automatically
   bubble up to outer ones
2. **Hard to reason about execution order** — especially if operations
   happen in parallel rather than sequence
3. **Difficult to reuse or modify** — changing one step often means
   restructuring the whole nested block

**How it's typically solved:**

```js
// Using Promises — flattens the pyramid
getUser(1)
  .then(user => getPosts(user.id))
  .then(posts => getComments(posts[0].id))
  .then(comments => getLikes(comments[0].id))
  .then(likes => console.log(likes))
  .catch(error => console.log("Error:", error)); // ONE error handler for the whole chain

// Using async/await — reads almost like synchronous code
async function loadData() {
  try {
    const user = await getUser(1);
    const posts = await getPosts(user.id);
    const comments = await getComments(posts[0].id);
    const likes = await getLikes(comments[0].id);
    console.log(likes);
  } catch (error) {
    console.log("Error:", error);
  }
}
```

**Follow-up questions interviewers ask:**

- Why does async/await make code more readable than Promise chains?
- Does using Promises automatically prevent callback hell? (Not automatically — you still have to structure it as a chain, not nested `.then()` calls)
