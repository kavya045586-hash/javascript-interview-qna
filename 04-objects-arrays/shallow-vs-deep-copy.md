## Q: Shallow Copy vs Deep Copy

**Answer:**

When copying objects/arrays, JavaScript doesn't automatically create a
fully independent copy — how much gets duplicated depends on the method
used.

**The core problem — direct assignment copies NOTHING (just a reference):**

```js
let obj1 = { name: "Kavya" };
let obj2 = obj1;  // obj2 just points to the SAME object

obj2.name = "Nagar";
console.log(obj1.name); // "Nagar" — obj1 changed too!
```

**Shallow Copy — copies top-level properties only:**

```js
let obj1 = { name: "Kavya", address: { city: "Bangalore" } };
let obj2 = { ...obj1 };  // spread operator — shallow copy

obj2.name = "Nagar";            // ✅ only affects obj2 — top-level is truly copied
obj2.address.city = "Mysore";   // ⚠️ affects obj1 TOO — nested object is still shared!

console.log(obj1.name);          // "Kavya" — safe
console.log(obj1.address.city);  // "Mysore" — oops, unintentionally changed
```

**Other ways to shallow copy:**

```js
let obj2 = Object.assign({}, obj1);   // objects
let arr2 = [...arr1];                  // arrays (spread)
let arr2 = arr1.slice();                // arrays (slice)
```

**Deep Copy — copies EVERYTHING, including nested objects/arrays:**

```js
let obj1 = { name: "Kavya", address: { city: "Bangalore" } };
let obj2 = structuredClone(obj1);  // modern deep copy

obj2.address.city = "Mysore";

console.log(obj1.address.city); // "Bangalore" — untouched ✅
console.log(obj2.address.city); // "Mysore"
```

**Older deep copy method (has limitations):**

```js
let obj2 = JSON.parse(JSON.stringify(obj1));
// Limitation: silently drops functions, undefined values,
// and converts Date objects into plain strings
```

**Quick summary table:**

| Method | Copy type | Nested objects safe? |
|---|---|---|
| `obj2 = obj1` | No copy — same reference | ❌ |
| `{...obj1}` / `Object.assign()` | Shallow | ❌ still shared |
| `structuredClone(obj1)` | Deep | ✅ fully independent |
| `JSON.parse(JSON.stringify())` | Deep (older, limited) | ✅ but breaks on functions/Date/undefined |

**Follow-up questions interviewers ask:**

- Why does modifying a nested object break a "shallow copy"?
- Why did `JSON.parse(JSON.stringify())` fail on my object with a function in it?
