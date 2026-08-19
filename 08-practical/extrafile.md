# Extra Topics & Deep-Dive Quiz — Practical JS

Every question explains the underlying concept. Cover the answer with your
hand and try to explain it in your own words first.

---

## Section 1: Currying & Function Composition

**Q1. What does this print?**
```js
const curry = (fn) => (...args) =>
  args.length >= fn.length ? fn(...args) : (...more) => curry(fn)(...args, ...more);

const add3 = curry((a, b, c) => a + b + c);
console.log(add3(1)(2)(3));
console.log(add3(1, 2)(3));
console.log(add3(1, 2, 3));
```
<details><summary>Answer</summary>
6, 6, 6 — this is a GENERIC curry helper that works flexibly, accepting
arguments one at a time OR in groups, as long as the total count eventually
reaches the original function's expected argument count (fn.length).
</details>

---

**Q2. Why is composition often written right-to-left (using reduceRight) instead of left-to-right?**
<details><summary>Answer</summary>
Convention borrowed from mathematical function composition, where f(g(x))
is read as "f of g of x" — g runs first, then f wraps around it. Right-to-
left composition matches this mathematical notation intuition, though
some libraries (like pipe()) intentionally go left-to-right for more
intuitive "step by step" readability instead.
</details>

---

## Section 2: Memoization

**Q3. What's a real risk of memoization if not implemented carefully?**
<details><summary>Answer</summary>
Unbounded cache growth — if a function is called with many different
unique arguments over time, the cache can grow indefinitely, causing a
memory leak. Production memoization often uses an LRU (Least Recently
Used) cache with a size limit instead of an infinitely growing object.
</details>

---

**Q4. Does memoization help with functions that have side effects (like console.log or API calls)?**
<details><summary>Answer</summary>
Not reliably — memoization only caches the RETURN VALUE. If a function's
side effects matter (like logging every call, or triggering a network
request), memoizing it means those side effects won't happen on cached
calls, which may or may not be desired behavior.
</details>

---

## Section 3: Error Handling & Custom Errors

**Q5. What does `error.stack` contain, and why is it useful for debugging?**
<details><summary>Answer</summary>
A string showing the call stack at the moment the error was created —
which functions were called, in what order, leading up to the error. This
helps trace exactly WHERE and HOW an error occurred, especially in deeply
nested function calls.
</details>

---

**Q6. Can you catch errors from an asynchronous callback (like inside setTimeout) using a regular try/catch around it?**
<details><summary>Answer</summary>
No — try/catch only catches SYNCHRONOUS errors within its own execution.
An error thrown inside a setTimeout callback happens LATER, in a
completely separate execution context, so the surrounding try/catch has
already finished by the time it runs. You'd need the try/catch INSIDE the
callback itself.
</details>

```js
try {
  setTimeout(() => { throw new Error("fail"); }, 100);
} catch (e) {
  console.log("caught"); // never runs — error escapes uncaught
}
```

---

## Section 4: Strict Mode

**Q7. Does strict mode affect performance?**
<details><summary>Answer</summary>
It can slightly IMPROVE performance in some engines, since strict mode
eliminates certain ambiguous behaviors, allowing JS engines to apply more
aggressive optimizations. It's not a major factor, but it's not a
performance cost either — a common misconception.
</details>

---

**Q8. Does `"use strict"` apply retroactively to code written above it in the same file?**
<details><summary>Answer</summary>
No — it must be the very FIRST statement in a file or function to take
effect (only comments/other directives can precede it). If placed
anywhere else, it's just treated as a harmless string literal with no
effect.
</details>

---

## Section 5: Polyfills

**Q9. Why does the map() polyfill need to use `this` and a `for` loop instead of something simpler?**
<details><summary>Answer</summary>
Because when you write arr.myMap(...), inside myMap, `this` automatically
refers to the array it was called on (arr) — that's how prototype methods
work. The for loop is necessary because you're implementing the mechanism
FROM SCRATCH — you can't use the real map() to build your own map()
polyfill, that would be circular.
</details>

---

**Q10. Would a polyfill for `Array.prototype.includes()` need to handle NaN specially? Why?**
<details><summary>Answer</summary>
Yes — a naive polyfill using === would fail to detect NaN in an array
(since NaN === NaN is false), unlike the REAL includes(), which correctly
finds NaN using the SameValueZero algorithm internally. A correct
polyfill needs special-case logic to handle this.
</details>

---

## Section 6: Design Patterns

**Q11. What problem does the Singleton pattern actually solve, in practical terms?**
<details><summary>Answer</summary>
Ensures expensive or stateful resources (like a database connection, a
configuration object, or a logging service) are only created ONCE and
shared everywhere, rather than accidentally creating multiple conflicting
instances that waste resources or cause inconsistent state.
</details>

---

**Q12. How is the Module pattern different from just using ES6 modules (import/export)?**
<details><summary>Answer</summary>
The Module pattern (using an IIFE + closures) was the PRE-ES6 way to
achieve encapsulation/privacy within a single script. ES6 modules achieve
similar encapsulation at the FILE level automatically — every module has
its own scope by default, without needing the IIFE trick.
</details>

---

## Section 7: Performance (Reflow/Repaint)

**Q13. Does reading `element.offsetHeight` always force a reflow?**
<details><summary>Answer</summary>
Only if there are PENDING style changes that haven't been applied yet —
the browser must recalculate layout to give you an accurate answer. If no
styles changed since the last reflow, reading it can return a cached
value without forcing extra work.
</details>

---

**Q14. Why is `transform` and `opacity` often recommended for animations instead of changing `width`/`top`/`left`?**
<details><summary>Answer</summary>
transform and opacity can often be handled entirely by the GPU compositor
layer, skipping BOTH reflow and repaint — they only trigger a cheaper
"composite" step. Changing width/top/left triggers full reflow, which is
much more expensive, especially during animations running many times per
second.
</details>

---

## Bonus: Scenario / Trace Questions

**Q15.** What does this print?
```js
function memoize(fn) {
  const cache = new Map();
  return (...args) => {
    const key = args.join(",");
    if (cache.has(key)) return cache.get(key);
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
}

const slowAdd = memoize((a, b) => a + b);
console.log(slowAdd(1, 2));
console.log(slowAdd(1, 2));
console.log(slowAdd(2, 1));
```
<details><summary>Answer</summary>
3, 3, 3 — the first two calls have identical args (1,2), so the second
uses the cache. The third call (2,1) has a DIFFERENT key string ("2,1"
vs "1,2"), so it recomputes separately, even though the mathematical
result happens to be the same.
</details>

---

**Q16.** What does this print?
```js
class Logger {
  constructor() {
    if (Logger.instance) return Logger.instance;
    this.logs = [];
    Logger.instance = this;
  }
  log(msg) {
    this.logs.push(msg);
  }
}

const l1 = new Logger();
l1.log("first");
const l2 = new Logger();
console.log(l2.logs);
```
<details><summary>Answer</summary>
["first"] — Singleton pattern: l2 = new Logger() returns the SAME
instance as l1 (since Logger.instance already exists), so l2.logs is
actually l1's logs array, already containing "first".
</details>

---

**Q17.** What does this print?
```js
"use strict";

function Vehicle(type) {
  this.type = type;
}

const car = Vehicle("Car"); // called WITHOUT new
console.log(car);
```
<details><summary>Answer</summary>
undefined — but importantly, in strict mode, `this` inside Vehicle() is
undefined (not the global object), so `this.type = type` actually throws
a TypeError: Cannot set properties of undefined. Without strict mode, this
would silently succeed by polluting the global object instead — strict
mode surfaces this bug loudly instead of hiding it.
</details>
