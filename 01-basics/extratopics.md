# Extra Basics Topics (Not Yet Covered)

---

## 1. Ternary Operator
Shorthand for if-else in a single line.

```js
let age = 20;
let status = age >= 18 ? "Adult" : "Minor";
```

---

## 2. Switch Statement
Alternative to multiple if-else chains, useful for comparing one value
against many fixed options.

```js
switch (day) {
  case "Mon": console.log("Monday"); break;
  case "Tue": console.log("Tuesday"); break;
  default: console.log("Unknown day");
}
```

---

## 3. Number Precision Issue (Famous Interview Trick)

```js
0.1 + 0.2;   // 0.30000000000000004, NOT 0.3
0.1 + 0.2 === 0.3;  // false
```
Why: JavaScript stores numbers in binary floating-point, which can't represent
some decimals exactly. Fix: use `Number((0.1 + 0.2).toFixed(2))` or a tolerance
check like `Math.abs(a - b) < Number.EPSILON`.

---

## 4. parseInt / parseFloat vs Number()

```js
parseInt("42px");    // 42 — extracts leading number, ignores rest
Number("42px");        // NaN — fails entirely if the whole string isn't numeric
parseFloat("3.14abc"); // 3.14
```

---

## 5. String Immutability

Strings can't be changed in place — every "modification" creates a new string.

```js
let str = "hello";
str[0] = "H";       // does nothing — strings are immutable
console.log(str);   // still "hello"

str = "Hello";       // this works — reassigning, not mutating
```

---

## 6. Default Parameters

```js
function greet(name = "Guest") {
  console.log("Hello, " + name);
}
greet();          // "Hello, Guest"
greet("Kavya");    // "Hello, Kavya"
```

---

## 7. NaN — What It Really Is

```js
typeof NaN;          // "number" — NaN is technically a number type!
NaN === NaN;           // false — NaN is never equal to itself
isNaN("hello");         // true — but checks AFTER coercion, unreliable
Number.isNaN("hello");   // false — stricter, doesn't coerce first (preferred)
```

---

## 8. Global Object

```js
// In browsers: window
// In Node.js: global
// Universal (works everywhere): globalThis
console.log(globalThis);
```

---

## 9. Comments

```js
// single line comment

/* multi
   line
   comment */
```

---

## 10. Object.freeze (Basic Immutability)

```js
const user = Object.freeze({ name: "Kavya" });
user.name = "Nagar"; // fails silently (or throws in strict mode)
console.log(user.name); // still "Kavya"
```
