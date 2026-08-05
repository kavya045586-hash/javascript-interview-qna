## Q: What are the Data Types in JavaScript?

**Answer:**

JavaScript has 8 data types, split into two categories:

**Primitive types (7):**
- `String` — text: `"hello"`
- `Number` — integers and decimals: `42`, `3.14`
- `Boolean` — `true` / `false`
- `undefined` — a variable declared but not assigned a value
- `null` — intentional absence of value
- `BigInt` — for numbers larger than `Number` can safely handle
- `Symbol` — unique, immutable identifier (often used as object keys)

**Non-primitive type (1):**
- `Object` — includes objects, arrays, functions, dates, etc.

**Example:**

```js
let name = "Kavya";        // String
let age = 21;               // Number
let isStudent = true;       // Boolean
let x;                      // undefined
let y = null;                // null
let big = 123456789123456789n; // BigInt
let sym = Symbol("id");     // Symbol
let user = { name: "Kavya" }; // Object
```

---

### 🔹 Why is Symbol used?

Object keys are normally strings, but string keys can **accidentally clash**
— if two different scripts both add a property called `id` to the same
object, one overwrites the other.

`Symbol` fixes this by creating a **guaranteed unique value**, even if two
symbols share the same description:

```js
let id1 = Symbol("id");
let id2 = Symbol("id");

console.log(id1 === id2); // false — always unique, even with the same label
```

This means Symbol keys can never accidentally collide with another key:

```js
let user = {};
user[id1] = "from library A";
user[id2] = "from library B";

console.log(user[id1]); // "from library A" — safe, no overwrite
```

**Real-world use cases:**
- Avoiding property name collisions in large codebases or third-party libraries
- "Hidden" properties — Symbol keys don't show up in `for...in` or `Object.keys()`
- Built-in JS features use symbols internally (e.g. `Symbol.iterator`, which makes an object loopable with `for...of`)

**Interview note:** Symbol is usually asked as a "do you know what it's for"
question, not something you'll use daily as a beginner — this level of
understanding is enough.
