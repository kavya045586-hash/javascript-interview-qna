## Q: call, apply, and bind

**Answer:**

All three let you explicitly control what `this` refers to inside a
function, and let you "borrow" a function from one object to use with
another.

**1. `call()` — invokes the function immediately, arguments passed individually**

```js
const person1 = { name: "Kavya" };
const person2 = { name: "Nagar" };

function greet(greeting) {
  console.log(greeting + ", " + this.name);
}

greet.call(person1, "Hello"); // "Hello, Kavya"
greet.call(person2, "Hi");     // "Hi, Nagar"
```

**2. `apply()` — same as call, but arguments passed as an array**

```js
greet.apply(person1, ["Hello"]); // "Hello, Kavya"
greet.apply(person2, ["Hi"]);     // "Hi, Nagar"
```

**3. `bind()` — does NOT invoke immediately; returns a NEW function with `this` permanently set**

```js
const greetKavya = greet.bind(person1);
greetKavya("Hello"); // "Hello, Kavya" — can call it later, this is locked in

const greetNagar = greet.bind(person2, "Hi"); // can also pre-fill arguments
greetNagar(); // "Hi, Nagar"
```

**Quick comparison table:**

| | Invokes immediately? | Arguments format | Returns |
|---|---|---|---|
| `call()` | Yes | Individually: `fn.call(obj, a, b)` | Function's return value |
| `apply()` | Yes | As array: `fn.apply(obj, [a, b])` | Function's return value |
| `bind()` | No | Individually | A new function (call it later) |

**Real use case — borrowing an array method for an array-like object:**

```js
function sum() {
  const args = Array.prototype.slice.call(arguments); // borrow slice from Array
  return args.reduce((a, b) => a + b, 0);
}
console.log(sum(1, 2, 3)); // 6
```

**Follow-up questions interviewers ask:**

- What's the main difference between call and apply? (Only how arguments are passed — individually vs as an array)
- Why would you use bind instead of call? (When you want to save the function with `this` fixed, to call later — e.g., event handlers, setTimeout callbacks)
