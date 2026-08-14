# Q: What is a Closure?

## Answer

A **closure** is a function that **remembers the variables from its outer (lexical) scope**, even after the outer function has finished executing.

This is possible because of **lexical scope** — the inner function keeps a **reference to its surrounding variables**, not a copy of their values.

### Example

```js
function outer() {
  let count = 0;

  function inner() {
    count++;
    return count;
  }

  return inner;
}

const counter = outer();

console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```

Even though `outer()` has already finished running, `inner()` (returned and stored in `counter`) still has access to `count` and remembers its value between calls.

This is a **closure in action**.

---

# Why Do Closures Matter?

Closures are commonly used for:

## 1. Data Privacy

Variables inside a function cannot be accessed or modified directly from outside.

```js
function outer() {
  let count = 0;

  return {
    increment: () => ++count,
    getCount: () => count
  };
}

const c = outer();

c.increment();

console.log(c.getCount()); // 1
console.log(c.count);      // undefined
```

Here, `count` is private.

The outside code can interact with `count` only through the functions returned by `outer()`.

---

## 2. Function Factories

Closures allow us to create functions that remember specific values.

```js
function multiplier(x) {
  return function (y) {
    return x * y;
  };
}

const double = multiplier(2);

console.log(double(5)); // 10
```

Here, `double` remembers the value `x = 2`.

So when we call:

```js
double(5);
```

it effectively performs:

```js
2 * 5;
```

---

# Closure with Local and Global Variables

Consider this example:

```js
let age = 0;

function outer() {
  let count = 0;

  age++; // increments only when outer() is called

  function inner() {
    count++;

    return {
      count,
      age
    };
  }

  return inner;
}

const counter1 = outer();

console.log(counter1()); // { count: 1, age: 1 }
console.log(counter1()); // { count: 2, age: 1 }

const counter2 = outer();

console.log(counter2()); // { count: 1, age: 2 }
console.log(counter2()); // { count: 2, age: 2 }
```

## Explanation

### `count` is local to each `outer()` call

Every time `outer()` is called, a **new `count` variable** is created.

Therefore:

```js
const counter1 = outer();
const counter2 = outer();
```

creates two independent `count` variables.

Conceptually:

```text
counter1 → count = 0
counter2 → count = 0
```

When we call:

```js
counter1();
```

only `counter1`'s `count` changes.

When we call:

```js
counter2();
```

only `counter2`'s `count` changes.

That's why they maintain **independent counters**.

---

### `age` is global

`age` exists outside `outer()`:

```js
let age = 0;
```

It is incremented here:

```js
age++;
```

This statement is inside `outer()`, so `age` increments **when `outer()` runs**, not when `inner()` runs.

Therefore:

```js
const counter1 = outer();
```

makes:

```text
age = 1
```

And:

```js
const counter2 = outer();
```

makes:

```text
age = 2
```

So:

* `age` tracks **how many times `outer()` was called**.
* `count` tracks **how many times a particular returned function was called**.

---

# Important Interview Point

A closure does **not** store a copy of the variable's value.

It maintains access to the **variable itself** through its lexical environment.

For example:

```js
function outer() {
  let count = 0;

  return function inner() {
    count++;
    return count;
  };
}
```

The returned `inner` function keeps access to the `count` variable even after `outer()` has finished executing.

So:

```js
const counter = outer();

counter(); // 1
counter(); // 2
counter(); // 3
```

The same `count` variable is being updated each time.

---

# Simple Definition for Interviews

> **A closure is a function that retains access to variables from its lexical scope even after the outer function has finished executing.**

### Easy way to remember

**Function + remembered lexical environment = Closure**

---

# Follow-up Questions Interviewers Ask

Interviewers commonly ask:

1. **What is lexical scope?**
2. **How does a closure work internally?**
3. **Does a closure store a copy or a reference to variables?**
4. **What are practical use cases of closures?**
5. **How are closures used for data privacy?**
6. **What is a function factory?**
7. **What is the difference between closure and scope?**
8. **Can closures cause memory leaks?**
9. **How do closures work inside loops?**
10. **What is the difference between `var`, `let`, and `const` in closures?**
11. **How do closures work with asynchronous code such as `setTimeout()`?**
12. **What happens when multiple closures access the same variable?**
