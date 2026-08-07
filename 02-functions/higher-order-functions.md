## Q: What is a Higher Order Function?

**Answer:**

A Higher Order Function (HOF) is a function that does at least one of the
following:
1. **Takes another function as an argument**, or
2. **Returns a function** as its result

This is possible in JavaScript because functions are **first-class
citizens** — they can be treated like any other value (stored in variables,
passed around, returned).

---

**1. Taking a function as an argument**

```js
function greetUser(name, formatter) {
  return formatter(name);
}

function upperCaseFormat(name) {
  return name.toUpperCase();
}

console.log(greetUser("Kavya", upperCaseFormat)); // "KAVYA"
```

**Step by step:**
1. `upperCaseFormat` is passed into `greetUser` — NOT called yet, just handed over as a value
2. Inside `greetUser`, `formatter` now refers to the `upperCaseFormat` function
3. `formatter(name)` actually calls it: `upperCaseFormat("Kavya")`
4. Returns `"KAVYA"`

---

**2. Returning a function**

```js
function multiplier(x) {
  return function (y) {
    return x * y;
  };
}

const double = multiplier(2);
console.log(double(5)); // 10
```

**Step by step:**
1. `multiplier(2)` runs and returns a NEW function — this inner function
   "remembers" `x = 2` (this is a closure, covered earlier)
2. That returned function is stored in `double`
3. `double(5)` calls the inner function with `y = 5`
4. Returns `2 * 5 = 10`

---

**3. Built-in HOFs you already use daily**

```js
[1, 2, 3].map(n => n * 2);         // map takes a function as argument
[1, 2, 3].filter(n => n > 1);       // filter takes a function as argument
[1, 2, 3].reduce((a, b) => a + b);   // reduce takes a function as argument
setTimeout(() => {}, 1000);           // setTimeout takes a function as argument
```

All of these are HOFs because they accept a function (`n => n * 2`, etc.)
as one of their arguments.

---

**4. ❌ Common confusion: "Is `map` itself a HOF, or is my callback the HOF?"**

```js
[1, 2, 3].map(function (n) {
  return n * 2;
});
```

**✅ Clarification:** `map` is the Higher Order Function — because it's the
one *accepting* a function as an argument. The function you pass in
(`function(n){ return n*2 }`) is just a regular callback, NOT itself a HOF
(unless it also takes/returns a function).

---

**5. Why HOFs matter**

They enable **functional programming** patterns — instead of writing manual
loops every time, you describe *what* you want done, and hand off the
*how* as a small reusable function:

```js
// Without HOF — manual loop
let doubled = [];
for (let i = 0; i < [1,2,3].length; i++) {
  doubled.push([1,2,3][i] * 2);
}

// With HOF — cleaner, more declarative
let doubled = [1, 2, 3].map(n => n * 2);
```

---

**Quick summary table:**

| Function | Is it a HOF? | Why |
|---|---|---|
| `map`, `filter`, `reduce` | ✅ Yes | Accept a function as an argument |
| `setTimeout`, `setInterval` | ✅ Yes | Accept a function as an argument |
| `multiplier(x)` (returns a function) | ✅ Yes | Returns a function |
| `n => n * 2` (a simple callback) | ❌ No | Doesn't take or return a function itself |

**Follow-up questions interviewers ask:**

- Name 3 built-in JavaScript methods that are Higher Order Functions
- Why are HOFs considered more "functional programming" style?
- Is the callback you pass to `.map()` itself a Higher Order Function? (No — unless that callback also takes/returns a function)
