## Q: What is the `this` Keyword?

**Answer:**

`this` refers to the object that is currently **calling/executing** the
function. Its value is NOT fixed — it depends on **how** a function is
called, not where it's defined (except for arrow functions, which are
lexically bound).

**1. In a regular function call — `this` is `undefined` (strict mode) or the global object**

```js
function show() {
  console.log(this);
}
show(); // undefined (strict mode) or window/global (non-strict)
```

**2. As an object method — `this` is the object before the dot**

```js
const user = {
  name: "Kavya",
  greet() {
    console.log(this.name); // "Kavya" — this = user
  }
};
user.greet();
```

**3. Inside a constructor function — `this` is the new object being created**

```js
function Person(name) {
  this.name = name;
}
const p = new Person("Kavya");
console.log(p.name); // "Kavya"
```

**4. In an arrow function — `this` comes from the surrounding scope, not the caller**

```js
const user = {
  name: "Kavya",
  greet: () => {
    console.log(this.name); // undefined — this = outer scope, not user
  }
};
user.greet();
```

**5. A common trap — losing `this` in a callback**

```js
const user = {
  name: "Kavya",
  greet() {
    setTimeout(function () {
      console.log(this.name); // undefined — this = global, NOT user (regular function loses binding)
    }, 1000);
  }
};
user.greet();
```

**Fix using arrow function (inherits `this` from `greet`):**

```js
const user = {
  name: "Kavya",
  greet() {
    setTimeout(() => {
      console.log(this.name); // "Kavya" — arrow function keeps this from greet()
    }, 1000);
  }
};
user.greet();
```

**Follow-up questions interviewers ask:**

- Why does a normal function callback inside a method lose `this`?
- How does an arrow function solve that problem?
