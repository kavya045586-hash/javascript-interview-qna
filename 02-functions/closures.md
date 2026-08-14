## Q: What is a Closure?

**Answer:**

A closure is a function that **remembers the variables from its outer
(lexical) scope**, even after the outer function has finished executing.
This is possible because of lexical scope — the inner function keeps a
reference to its surrounding variables, not a copy of their values.

**Example:**

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

Even though `outer()` has already finished running, `inner()` (returned and
stored in `counter`) still has access to `count` — and remembers its value
between calls. This is a closure in action.

**Why closures matter (real use cases):**

```js
// 1. Data privacy — count can't be accessed or modified directly from outside
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
console.log(c.count);       // undefined — no direct access

// 2. Function factories
function multiplier(x) {
  return function (y) {
    return x * y;
  };
}
const double = multiplier(2);
console.log(double(5)); // 10
```

```js
let age = 0;

function outer() {
  let count = 0;
  age++; // increments only when outer() is called

  function inner() {
    count++;
    return { count, age };
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

✅ Explanation

-count is local to each outer() call.

-Every inner() remembers its own count because of closure.

-That’s why counter1 and counter2 have independent counters.

-age is global.

-It increments only when outer() runs, not when inner() runs.

-So age tracks how many times you created a new counter, while count tracks how many times you used that counter.

**Follow-up questions interviewers ask:**

- Why do closures matter for data privacy?
- What's the classic "loop + closure" bug with `var`? (covered in 07-tricky-outputs)
