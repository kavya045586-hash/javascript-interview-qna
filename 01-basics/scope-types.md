## Q: What are the different types of Scope in JavaScript?

**Answer:**

Scope determines **where** in your code a variable is accessible. JavaScript
has three main types of scope:

**1. Global Scope**
Variables declared outside any function or block. Accessible from anywhere
in the code.

```js
let globalVar = "I'm global";

function show() {
  console.log(globalVar); // accessible here too
}
```

**2. Function Scope**
Variables declared with `var` inside a function are only accessible within
that function — not outside it.

```js
function greet() {
  var message = "Hello";
  console.log(message); // accessible here
}

console.log(message); // ❌ ReferenceError — not accessible outside the function
```

**3. Block Scope**
Variables declared with `let`/`const` inside `{ }` (a block — like an `if`,
`for`, or just standalone braces) are only accessible within that block.

```js
if (true) {
  let blockVar = "I'm block-scoped";
  console.log(blockVar); // accessible here
}

console.log(blockVar); // ❌ ReferenceError — not accessible outside the block
```

**Key point:** `var` does NOT respect block scope — only function scope.
This is one of the main reasons `let`/`const` are preferred today.

```js
if (true) {
  var notBlockScoped = "I leak out!";
}
console.log(notBlockScoped); // ✅ works — var ignores the if-block entirely
```

**Follow-up questions interviewers ask:**

- Why does `var` "leak" out of blocks but `let` doesn't?
- Is a `for` loop's `{ }` a block scope too? (Yes — same rules apply)
