## Q: Function Declaration vs Function Expression

**Answer:**

**Function Declaration** — defined with the `function` keyword and a name,
standing on its own as a statement. Fully hoisted (name + body), so it can
be called before its written position.

```js
greet(); // ✅ works — "Hello"

function greet() {
  console.log("Hello");
}
```

**Function Expression** — a function assigned to a variable. Only the
variable is hoisted (as `undefined` for `var`, or left in TDZ for
`let`/`const`), not the function body. Calling it before the assignment
line fails.

```js
greet(); // ❌ TypeError: greet is not a function

var greet = function () {
  console.log("Hello");
};
```

**Named vs Anonymous Function Expressions:**

```js
// Anonymous — most common
const greet = function () {
  console.log("Hello");
};

// Named — the name is only usable INSIDE the function itself (useful for recursion, stack traces)
const factorial = function fact(n) {
  return n <= 1 ? 1 : n * fact(n - 1);
};
```

**Quick comparison table:**

| | Function Declaration | Function Expression |
|---|---|---|
| Hoisted fully? | Yes | No (only variable, not body) |
| Can call before definition? | Yes | No |
| Needs a name? | Yes | Optional |
| Common use | Reusable named functions | Callbacks, IIFEs, conditional assignment |

**Follow-up questions interviewers ask:**

- Why would you choose a function expression over a declaration?
- What's a named function expression useful for? (Recursion and easier debugging via stack traces, since the name shows in error logs even though it's not accessible outside)

---

## 🔹 Deep Dive: Why Named Function Expressions Are Safer for Recursion

### Step-by-step trace of the named version

```js
const factorial = function fact(n) {
  return n <= 1 ? 1 : n * fact(n - 1);
};

console.log(factorial(5));
```

### Two different names, two different jobs

- **`factorial`** — the outer variable, used to call the function from
  outside: `factorial(5)`
- **`fact`** — the inner name, only visible inside the function body, used
  for the recursive call: `fact(n - 1)`

---

### ⚠️ Why relying on the outer variable is risky (anonymous version)

```js
const factorial = function (n) {
  return n <= 1 ? 1 : n * factorial(n - 1); // relies on OUTER variable
};

console.log(factorial(5)); // 120 — works fine, for now
```

This "works" only because `factorial` currently points to this function.
But it can break if the outer variable gets reassigned:

```js
const factorial = function (n) {
  return n <= 1 ? 1 : n * factorial(n - 1);
};

const calc = factorial;   // copy the function to another variable
factorial = null;          // reassign the original

console.log(calc(5)); // ❌ TypeError: factorial is not a function
```

**Why it breaks:** `calc` still holds the function, but the function's
recursive call `factorial(n - 1)` now points to `null` instead of itself,
since `factorial` was reassigned.

---

### ✅ Why the named version never breaks this way

```js
const factorial = function fact(n) {
  return n <= 1 ? 1 : n * fact(n - 1); // relies on INNER name — untouchable from outside
};

const calc = factorial;
factorial = null;

console.log(calc(5)); // ✅ 120 — still works! fact was never affected
```

`fact` isn't a variable living outside the function — it exists only
inside the function's own private scope, so nothing external can ever
reassign or delete it.

---

### 📊 Summary Table

| | Anonymous Function Expression | Named Function Expression |
|---|---|---|
| Recursion possible? | Yes | Yes |
| Depends on | Outer variable staying unchanged | Internal name (can't be touched from outside) |
| Safe if outer variable is reassigned? | ❌ Breaks | ✅ Still works |
| Typical use | One-time callbacks, simple logic | Recursive functions |

**Key takeaway:** Recursion is possible either way — having an internal
name doesn't *enable* recursion, it makes recursion **safe from accidental
reassignment**, which matters more in large codebases where variable names
can get overwritten elsewhere.
