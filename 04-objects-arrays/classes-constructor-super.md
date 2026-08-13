## Q: Classes, Constructor, and super

**Answer:**

ES6 `class` syntax is essentially a cleaner way to write constructor
functions and prototypal inheritance — it doesn't introduce a new
inheritance model, just nicer syntax on top of the existing one.

**Basic class:**

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    console.log(`Hello, I'm ${this.name}`);
  }
}

const p1 = new Person("Kavya", 21);
p1.greet(); // "Hello, I'm Kavya"
```

- `constructor()` — runs automatically when you create a new instance with `new`
- Methods defined in a class (like `greet`) are automatically placed on the prototype — same efficiency as manually doing `Person.prototype.greet = ...`

**Inheritance using `extends` and `super`:**

```js
class Student extends Person {
  constructor(name, age, college) {
    super(name, age);       // calls Person's constructor — MUST be called before using `this`
    this.college = college;
  }

  greet() {
    super.greet();           // calls Person's greet() method
    console.log(`I study at ${this.college}`);
  }
}

const s1 = new Student("Kavya", 21, "New Horizon College");
s1.greet();
// "Hello, I'm Kavya"
// "I study at New Horizon College"
```

**Important rule: `super()` must be called before using `this` in a subclass constructor**

```js
class Student extends Person {
  constructor(name, college) {
    this.college = college;  // ❌ ReferenceError — must call super() first
    super(name);
  }
}
```

**Follow-up questions interviewers ask:**

- What does `super()` do, and why must it be called first in a subclass constructor?
- Are JavaScript classes truly a "new" feature, or just syntax on top of prototypes? (Just syntax — under the hood, it's still using prototypal inheritance)
