## Q: Prototypes & Prototypal Inheritance

**Answer:**

Every JavaScript object has an internal link to another object called its
**prototype**. When you access a property/method on an object, JavaScript
first checks the object itself — if not found, it looks up the
**prototype chain** until it finds it (or reaches `null`).

**Basic example:**

```js
const animal = {
  eats: true,
  walk() {
    console.log("Animal walks");
  }
};

const rabbit = Object.create(animal); // rabbit's prototype = animal
rabbit.jumps = true;

console.log(rabbit.eats);   // true — found on the PROTOTYPE (animal), not rabbit itself
console.log(rabbit.jumps);   // true — found directly on rabbit
rabbit.walk();                 // "Animal walks" — inherited method
```

**How this works internally:**

```js
console.log(rabbit.hasOwnProperty("jumps")); // true — jumps is rabbit's OWN property
console.log(rabbit.hasOwnProperty("eats"));   // false — eats belongs to the prototype, not rabbit
```

**Constructor functions and `.prototype`:**

```js
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function () {
  console.log("Hello, " + this.name);
};

const p1 = new Person("Kavya");
p1.greet(); // "Hello, Kavya" — greet() is found on Person.prototype, not on p1 itself
```

**Why put methods on the prototype instead of inside the constructor?**

```js
// ❌ Inefficient — every instance gets its OWN copy of greet
function Person(name) {
  this.name = name;
  this.greet = function () {
    console.log("Hello, " + this.name);
  };
}

// ✅ Efficient — all instances SHARE one greet, via the prototype
function Person(name) {
  this.name = name;
}
Person.prototype.greet = function () {
  console.log("Hello, " + this.name);
};
```

**Follow-up questions interviewers ask:**

- What is the prototype chain, and how far does it go? (Up through each prototype's own prototype, until it reaches `null` at the top — this is `Object.prototype`'s prototype)
- Why is putting methods on the prototype more memory-efficient than defining them in the constructor?
