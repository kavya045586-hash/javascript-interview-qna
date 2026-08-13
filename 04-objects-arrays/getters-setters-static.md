## Q: Getters, Setters, and Static Methods

**Answer:**

**Getters and Setters** — special methods that let you define custom
logic for reading (`get`) or writing (`set`) a property, while still using
normal property-access syntax (no parentheses needed).

```js
class Person {
  constructor(name) {
    this._name = name; // convention: underscore prefix for the "real" internal value
  }

  get name() {
    return this._name.toUpperCase();  // custom logic runs when reading .name
  }

  set name(newName) {
    if (newName.length < 2) {
      console.log("Name too short!");
      return;
    }
    this._name = newName;              // custom logic runs when writing .name
  }
}

const p1 = new Person("Kavya");
console.log(p1.name);   // "KAVYA" — getter runs automatically
p1.name = "Nagar";        // setter runs automatically
console.log(p1.name);    // "NAGAR"
p1.name = "K";              // "Name too short!" — setter validation blocks it
```

**Static Methods** — belong to the CLASS itself, not to individual
instances. Called directly on the class, not on an object created from it.

```js
class MathHelper {
  static square(x) {
    return x * x;
  }
}

console.log(MathHelper.square(5)); // 25 — called on the class directly

const m = new MathHelper();
m.square(5); // ❌ TypeError: m.square is not a function — static methods aren't on instances
```

**Real use case for static methods — utility/helper functions related to the class, but not tied to a specific instance:**

```js
class User {
  constructor(name) {
    this.name = name;
  }

  static createGuest() {
    return new User("Guest");  // factory method — doesn't need an existing instance
  }
}

const guest = User.createGuest();
console.log(guest.name); // "Guest"
```

**Follow-up questions interviewers ask:**

- Why would you use a getter/setter instead of a regular property?
- Why can't you call a static method on an instance of the class?
