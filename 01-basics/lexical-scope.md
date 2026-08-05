## Q: What is Lexical Scope?

**Answer:**

Lexical scope means a function's access to variables is determined by
**where the function is physically written in the code** — not by where or
how it's called. Inner functions can access variables from their outer
(parent) functions, based on this "nesting" structure.

This is the foundation that makes **closures** possible.

**Example:**

```js
function outer() {
  let outerVar = "I'm from outer";

  function inner() {
    console.log(outerVar); // ✅ inner can access outer's variable
  }

  inner();
}

outer(); // logs: "I'm from outer"
```

Here, `inner()` can see `outerVar` because it's **lexically nested inside**
`outer()` — the relationship is fixed by how the code is written, not by
when or where `inner()` gets called.

**What lexical scope does NOT allow (important contrast):**

```js
function outer() {
  let outerVar = "I'm from outer";
}

function inner() {
  console.log(outerVar); // ❌ ReferenceError — inner is NOT nested inside outer
}

outer();
inner(); // fails — inner has no lexical access to outer's variables
```

**Follow-up questions interviewers ask:**

- What's the difference between lexical scope and closures? (Lexical scope is the *rule* that determines what a function can access; a closure is what happens when a function "remembers" that access even after the outer function has finished running)
- Does lexical scope depend on where a function is called or where it's written? (Written — always determined at the time the code is defined, not when it executes)
