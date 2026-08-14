## Q: Spread vs Rest Operator

**Answer:**

Both use the same `...` syntax, but they do **opposite** things depending
on context — spread "expands" values, rest "collects" values.

**Spread — expands an array/object into individual elements**

```js
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5];
console.log(arr2); // [1, 2, 3, 4, 5]

const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 };
console.log(obj2); // { a: 1, b: 2, c: 3 }

// Spreading into function arguments
function add(a, b, c) {
  return a + b + c;
}
const nums = [1, 2, 3];
console.log(add(...nums)); // 6 — spreads array into separate arguments
```

**Rest — collects multiple individual values INTO an array**

```js
function sum(...numbers) {  // rest — gathers all arguments into an array
  return numbers.reduce((a, b) => a + b, 0);
}
console.log(sum(1, 2, 3, 4)); // 10

const [first, ...remaining] = [1, 2, 3, 4];
console.log(first, remaining); // 1 [2, 3, 4]
```

**How to tell them apart — check the CONTEXT, not the symbol itself:**

```js
// Spread — used when CREATING a new array/object/argument list
const merged = [...arr1, ...arr2];

// Rest — used when RECEIVING parameters or destructuring
function fn(...args) { }
const { a, ...rest } = obj;
```

**Quick summary table:**

| | Spread | Rest |
|---|---|---|
| Direction | Expands values OUT | Collects values IN |
| Used in | Function calls, array/object literals | Function parameters, destructuring |
| Example | `fn(...arr)` | `function fn(...args)` |

**Follow-up questions interviewers ask:**

- How do you tell spread and rest apart if they look identical? (Context — spread is used where you're building something; rest is used where you're receiving/destructuring something)
- Can rest parameters be used anywhere except the LAST parameter? (No — rest must always be the last parameter in a function
