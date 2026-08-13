## Q: Array forEach, find, some, and every

**Answer:**

**1. `forEach()` — runs a function on each element, returns `undefined` (no new array)**

```js
const numbers = [1, 2, 3];
numbers.forEach(n => console.log(n * 2));
// 2
// 4
// 6

const result = numbers.forEach(n => n * 2);
console.log(result); // undefined — forEach doesn't return anything useful
```

Used purely for side effects (logging, updating external state) — NOT for
transforming data (use `map()` for that instead).

**2. `find()` — returns the FIRST element that passes a test, or `undefined` if none match**

```js
const users = [
  { id: 1, name: "Kavya" },
  { id: 2, name: "Nagar" }
];

const user = users.find(u => u.id === 2);
console.log(user); // { id: 2, name: "Nagar" }

const missing = users.find(u => u.id === 99);
console.log(missing); // undefined
```

**3. `some()` — returns `true` if AT LEAST ONE element passes the test**

```js
const numbers = [1, 2, 3, 4];
const hasEven = numbers.some(n => n % 2 === 0);
console.log(hasEven); // true — 2 and 4 are even
```

**4. `every()` — returns `true` only if ALL elements pass the test**

```js
const numbers = [2, 4, 6];
const allEven = numbers.every(n => n % 2 === 0);
console.log(allEven); // true

const numbers2 = [2, 3, 6];
const allEven2 = numbers2.every(n => n % 2 === 0);
console.log(allEven2); // false — 3 breaks it
```

**Quick comparison table:**

| Method | Returns | Stops early? |
|---|---|---|
| `forEach()` | `undefined` | No — always runs on every element |
| `find()` | The matching element (or undefined) | Yes — stops at first match |
| `some()` | `true`/`false` | Yes — stops at first `true` |
| `every()` | `true`/`false` | Yes — stops at first `false` |

**Follow-up questions interviewers ask:**

- Why shouldn't you use forEach() to build a new array? (It doesn't return one — use map() instead, which is built exactly for that purpose)
- What's the difference between find() and filter()? (find returns ONE element (or undefined); filter returns an ARRAY of all matches, even if empty)
