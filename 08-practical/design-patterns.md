## Q: Design Patterns in JavaScript

**Answer:**

Design patterns are reusable solutions to common coding problems. A few
commonly asked about in JS interviews:

**1. Module Pattern — encapsulation using closures (data privacy)**

```js
const CounterModule = (function () {
  let count = 0; // private — not accessible from outside
  return {
    increment() { count++; return count; },
    reset() { count = 0; }
  };
})();

console.log(CounterModule.increment()); // 1
console.log(CounterModule.count);        // undefined — truly private
```

**2. Singleton Pattern — ensures only ONE instance of something exists**

```js
class Database {
  constructor() {
    if (Database.instance) {
      return Database.instance; // return the EXISTING instance instead of creating a new one
    }
    Database.instance = this;
  }
}

const db1 = new Database();
const db2 = new Database();
console.log(db1 === db2); // true — same instance
```

**3. Observer Pattern — objects "subscribe" to be notified of events**

```js
class EventEmitter {
  constructor() {
    this.listeners = {};
  }
  on(event, callback) {
    if (!this.listeners[event]) this.listeners[event] = [];
    this.listeners[event].push(callback);
  }
  emit(event, data) {
    (this.listeners[event] || []).forEach(cb => cb(data));
  }
}

const emitter = new EventEmitter();
emitter.on("message", (data) => console.log("Received:", data));
emitter.emit("message", "Hello!"); // "Received: Hello!"
```

**Follow-up questions interviewers ask:**

- What real JavaScript feature is built on the Observer pattern? (addEventListener — the DOM's entire event system is essentially the Observer pattern)
- Why is the Module pattern useful for data privacy in older JS (pre-classes)?
