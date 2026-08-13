## Q: Object Creation & Shorthand Syntax

**Answer:**

There are several ways to create objects in JavaScript, and ES6 introduced
shorthand syntax that makes common patterns much cleaner.

**1. Object literal (most common)**

```js
const user = {
  name: "Kavya",
  age: 21
};
```

**2. Property shorthand — when variable name matches the key name**

```js
const name = "Kavya";
const age = 21;

// Old way (repetitive)
const user = { name: name, age: age };

// Shorthand (ES6)
const user = { name, age };  // same result — key auto-matches variable name
```

**3. Method shorthand — no need for the `function` keyword**

```js
// Old way
const user = {
  greet: function () {
    console.log("Hello");
  }
};

// Shorthand
const user = {
  greet() {
    console.log("Hello");
  }
};
```

**4. Computed property names — dynamic keys**

```js
const key = "email";
const user = {
  [key]: "kavya@example.com"  // key becomes "email" dynamically
};
console.log(user.email); // "kavya@example.com"
```

**5. `Object.create()` and constructor functions (alternative creation methods)**

```js
// Using new + constructor function
function Person(name) {
  this.name = name;
}
const p1 = new Person("Kavya");

// Using Object.create() — creates an object with a specified prototype
const p2 = Object.create(Person.prototype);
```

**Follow-up questions interviewers ask:**

- What's the benefit of property shorthand beyond just less typing? (Reduces repetition and errors — if the variable name changes, you don't have to update the key separately)
- When would you use computed property names?
