## Q: Object and Array Destructuring

**Answer:**

Destructuring lets you unpack values from objects/arrays into individual
variables in a single, clean line — instead of accessing each property
manually.

**Object destructuring:**

```js
const user = { name: "Kavya", age: 21, city: "Bangalore" };

// Old way
const name = user.name;
const age = user.age;

// Destructuring
const { name, age } = user;
console.log(name, age); // "Kavya" 21
```

**Renaming while destructuring:**

```js
const { name: userName } = user;
console.log(userName); // "Kavya" — note: `name` variable does NOT exist, only `userName`
```

**Default values (if property doesn't exist):**

```js
const { country = "India" } = user;
console.log(country); // "India" — user.country doesn't exist, so default is used
```

**Array destructuring:**

```js
const colors = ["red", "green", "blue"];

// Old way
const first = colors[0];
const second = colors[1];

// Destructuring
const [first, second] = colors;
console.log(first, second); // "red" "green"
```

**Skipping elements in array destructuring:**

```js
const [, , third] = colors;  // skip first two, grab third
console.log(third); // "blue"
```

**Destructuring in function parameters (very common in real code):**

```js
function greet({ name, age }) {
  console.log(`Hello ${name}, age ${age}`);
}
greet(user); // "Hello Kavya, age 21"
```

**Nested destructuring:**

```js
const person = { name: "Kavya", address: { city: "Bangalore" } };
const { address: { city } } = person;
console.log(city); // "Bangalore"
```

**Follow-up questions interviewers ask:**

- How do you set a default value while destructuring?
- Can you destructure directly in a function's parameters? (Yes — very common for cleanly accessing object properties passed into a function)
