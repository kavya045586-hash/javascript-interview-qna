## Q: Primitive vs Reference Types

**Answer:**

- **Primitive types** (String, Number, Boolean, undefined, null, BigInt, Symbol)
  are stored **by value** — each variable holds its own independent copy.
- **Reference types** (Objects, Arrays, Functions) are stored **by reference**
  — the variable holds a pointer to the value's location in memory, not the
  value itself.

This affects how copying and comparison behave.

**Example — Primitive (copied by value):**

```js
let a = 10;
let b = a;   // b gets a COPY of a's value
b = 20;

console.log(a); // 10 — unaffected
console.log(b); // 20
```

**Example — Reference (copied by reference):**

```js
let obj1 = { name: "Kavya" };
let obj2 = obj1;   // obj2 points to the SAME object in memory
obj2.name = "Nagar";

console.log(obj1.name); // "Nagar" — changed too!
console.log(obj2.name); // "Nagar"
```
### Shallow Copy — nested objects still shared

```js
let obj1 = { name: "Kavya", address: { city: "Bangalore" } };
let obj2 = { ...obj1 };  // shallow copy

obj2.name = "Nagar";            // ✅ only affects obj2, top-level is copied
obj2.address.city = "Mysore";   // ⚠️ affects obj1 TOO! nested object is shared

console.log(obj1.name);          // "Kavya" — safe
console.log(obj1.address.city);  // "Mysore" — oops, changed unintentionally
```

### Deep Copy — nested objects are fully independent

```js
let obj1 = { name: "Kavya", address: { city: "Bangalore" } };
let obj2 = structuredClone(obj1);  // deep copy

obj2.address.city = "Mysore";

console.log(obj1.address.city); // "Bangalore" — untouched ✅
console.log(obj2.address.city); // "Mysore"
```
