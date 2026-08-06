## Q: What is an IIFE (Immediately Invoked Function Expression)?

**Answer:**

An IIFE is a function that runs **immediately** as soon as it's defined —
you don't call it separately later. It's wrapped in parentheses to tell
JavaScript to treat it as an expression, then invoked right away with `()`.

**Syntax:**

```js
(function () {
  console.log("I run immediately!");
})();

// Arrow function version
(() => {
  console.log("I also run immediately!");
})();
```

**Why use an IIFE?**

The main reason: **avoid polluting the global scope**. Any variables
declared inside run and stay contained — they never leak out.

```js
(function () {
  var secret = "hidden";
  console.log(secret); // "hidden"
})();

console.log(typeof secret); // "undefined" — secret never leaked outside
```

**Historical context:** IIFEs were extremely common before `let`/`const`
and ES6 modules existed, since `var` had no block scope and would leak into
the global scope. They're less necessary today but still appear in some
codebases and libraries.

**Follow-up questions interviewers ask:**

- Why were IIFEs more common before ES6? (No block scope with `var`, so IIFEs were the main way to create an isolated scope)
- Can an IIFE take parameters? 
```js
(function (name) {
  console.log("Hello, " + name);
})("Kavya");
```
