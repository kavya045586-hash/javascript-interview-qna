## Q: Arrow Functions vs Normal Functions

**Answer:**

Arrow functions (`=>`) are a shorter syntax for writing functions, but they
also behave differently from normal functions in a few important ways —
these differences come up constantly in interviews.

**1. Syntax**

```js
// Normal function
function add(a, b) {
  return a + b;
}

// Arrow function
const add = (a, b) => a + b;   // implicit return, no {} needed for single expressions
```

---

**2. `this` binding — the biggest difference**

- Normal functions get their own `this`, determined by **how they're called**
- Arrow functions do NOT have their own `this` — they inherit `this` from
  the surrounding (lexical) scope where they were defined

**❌ Problem:**

```js
const obj = {
  name: "Kavya",
  normalFn: function () {
    console.log(this.name); // "Kavya" — this = obj, because obj.normalFn() called it
  },
  arrowFn: () => {
    console.log(this.name); // undefined — this = surrounding scope (not obj)
  }
};

obj.normalFn(); // "Kavya"
obj.arrowFn();   // undefined
```

**✅ Solution:** Don't use arrow functions for object methods that need
`this`. Use a normal function/method shorthand for the method itself, and
only use an arrow function INSIDE it (for callbacks) if you want it to
inherit that correct `this`:

```js
const obj = {
  name: "Kavya",
  greet() {                          // normal function — has correct `this`
    setTimeout(() => {
      console.log(this.name);        // ✅ "Kavya" — arrow inherits `this` from greet()
    }, 1000);
  }
};
obj.greet();
```

---

**3. No `arguments` object**

**❌ Problem:**

```js
function normalFn() {
  console.log(arguments); // works — lists all arguments passed
}

const arrowFn = () => {
  console.log(arguments); // ❌ ReferenceError — arrow functions don't have their own `arguments`
};
```

**✅ Solution:** Use rest parameters (`...args`) instead — this works in
both function types and gives you a real array (unlike `arguments`, which
is only array-*like*):

```js
const arrowFn = (...args) => {
  console.log(args); // ✅ [1, 2, 3] — a real array
};
arrowFn(1, 2, 3);

// Bonus: rest params are better even in normal functions
function normalFn(...args) {
  args.map(x => x * 2); // ✅ works — real array methods available
}
```

---

**4. Cannot be used as constructors**

**❌ Problem:**

```js
function Person(name) {
  this.name = name;
}
const p1 = new Person("Kavya"); // ✅ works

const PersonArrow = (name) => {
  this.name = name;
};
const p2 = new PersonArrow("Kavya"); // ❌ TypeError: PersonArrow is not a constructor
```

**✅ Solution:** Simply don't use arrow functions as constructors — always
use a regular `function` (or an ES6 `class`) when you need to create
objects with `new`:

```js
function Person(name) {
  this.name = name;
}
const p1 = new Person("Kavya"); // ✅ works fine
```

---

**Quick comparison table:**

| | Normal Function | Arrow Function |
|---|---|---|
| Own `this`? | Yes | No — inherits from surrounding scope |
| Has `arguments`? | Yes | No — use `...args` instead |
| Can be a constructor (`new`)? | Yes | No |
| Good for | Object methods, constructors | Callbacks, short one-liners, preserving outer `this` |

**Follow-up questions interviewers ask:**

- Why do arrow functions not have their own `this`?
- When would you deliberately choose an arrow function over a normal one?
