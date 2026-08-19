## Q: Currying & Function Composition

**Answer:**

**Currying** transforms a function taking multiple arguments into a chain
of functions, each taking ONE argument at a time.

```js
function add(a) {
  return function (b) {
    return function (c) {
      return a + b + c;
    };
  };
}
console.log(add(1)(2)(3)); // 6

// Arrow function version — more compact
const addArrow = a => b => c => a + b + c;
console.log(addArrow(1)(2)(3)); // 6
```

**Why curry?** Lets you create specialized/reusable functions by partially
applying some arguments upfront:

```js
const discount = rate => price => price - price * rate;
const tenPercentOff = discount(0.1);  // "locked in" 10% rate
console.log(tenPercentOff(200)); // 180
console.log(tenPercentOff(500)); // 450
```

**Function Composition** — combining multiple small functions into one
larger function, where each function's OUTPUT becomes the next one's INPUT.

```js
const double = x => x * 2;
const addOne = x => x + 1;

// Manual composition
const doubleThenAddOne = x => addOne(double(x));
console.log(doubleThenAddOne(5)); // 11

// Generic compose helper
const compose = (...fns) => x => fns.reduceRight((acc, fn) => fn(acc), x);
const combined = compose(addOne, double);
console.log(combined(5)); // 11 — same result, right-to-left order
```

**Follow-up questions interviewers ask:**

- What's the practical benefit of currying? (Creates reusable, specialized functions from a general one — e.g., discount(0.1) becomes a ready-to-use "10% off" function)
- What's the difference between currying and partial application? (Currying always breaks a function into unary (single-argument) functions; partial application can fix ANY number of arguments upfront, not necessarily one at a time)
