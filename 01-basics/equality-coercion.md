## Q: == vs === and Type Coercion

**Answer:**

- `==` (loose equality) — compares values **after converting them to the same type** (type coercion happens automatically)
- `===` (strict equality) — compares both **value AND type**, no conversion happens

Because `==` silently converts types behind the scenes, it can produce
confusing/unexpected results. Most style guides recommend always using `===`
unless you have a specific reason to allow type coercion.

**Example:**

```js
5 == "5";      // true  — string "5" is coerced to number 5, then compared
5 === "5";     // false — different types (number vs string), no coercion

0 == false;     // true  — false is coerced to 0
0 === false;    // false — different types

null == undefined;   // true  — special case, both loosely equal each other
null === undefined;  // false — different types

"" == 0;        // true  — empty string coerced to 0
"" === 0;       // false
```

---

**What is Type Coercion?**

Type coercion is JavaScript automatically converting a value from one type
to another when needed — for example, during comparisons, math operations,
or string concatenation.

```js
"5" + 3;     // "53"  — number 3 is coerced to string, then concatenated
"5" - 3;     // 2     — string "5" is coerced to number, then subtracted
"5" * "2";   // 10    — both strings coerced to numbers
true + 1;    // 2     — true is coerced to 1
```

---

### 🔹 Why does `+` behave differently from `-`, `*`, `/`?

**`+` (plus) — prefers STRING conversion if either side is a string**

```js
"5" + 3;   // "53"
```

Why: When JavaScript sees `+` and **at least one side is a string**, it
converts the *other* side to a string too, then joins them together
(concatenation). So `3` becomes `"3"`, and `"5" + "3"` = `"53"`.

**`-`, `*`, `/` — always prefer NUMBER conversion**

```js
"5" - 3;   // 2
"5" * "2"; // 10
```

Why: These operators **only make sense mathematically** — there's no such
thing as "string subtraction" or "string multiplication." So JavaScript
converts both sides to numbers first, then does the math. `"5"` becomes `5`,
and `5 - 3 = 2`.

**Why is `+` special?**

Because `+` has two jobs in JavaScript:
1. Adding numbers: `2 + 3` → `5`
2. Joining strings: `"a" + "b"` → `"ab"`

JavaScript has to guess which job you mean. Its rule: *"If either side is a
string, assume you want to join them as strings."* The other operators
(`-`, `*`, `/`) only have one job (math), so there's no ambiguity — they
always convert to numbers.

**Quick memory trick for interviews:**

> "+ likes strings, everyone else likes numbers."

If a string is anywhere near a `+`, expect concatenation. Every other math
operator (`-`, `*`, `/`, `%`) will always try to force both sides into
numbers, no exceptions.

---

**Follow-up questions interviewers ask:**

- Why is `===` generally preferred over `==` in production code?
- What's the result of `[] == false`? (true — array coerces to empty string, then to 0, matching false's 0)
