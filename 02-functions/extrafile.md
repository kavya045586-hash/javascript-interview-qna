# Extra Topics & Deep-Dive Quiz — Functions

Every question here explains the underlying concept. Cover the answer with
your hand and try to explain it in your own words first.

---

## Section 1: Closures

**Q1. What makes a closure different from a normal function?**
<details><summary>Answer</summary>
A normal function only has access to its own local variables while it's
running. A closure is a function that keeps access to variables from its
OUTER (enclosing) scope even AFTER that outer function has already
finished executing — because of lexical scope, JS keeps those variables
alive in memory as long as the inner function still references them.
</details>

---

**Q2. What does this print, and why?**
```js
function outer() {
  let count = 0;
  return function () {
    count++;
    return count;
  };
}
const counter1 = outer();
const counter2 = outer();
console.log(counter1()); // ?
console.log(counter1()); // ?
console.log(counter2()); // ?
```
<details><summary>Answer</summary>
1, 2, 1 — each call to outer() creates a BRAND NEW, independent `count`
variable. counter1 and counter2 are separate closures with their own
private `count`, so they don't share or affect each other.
</details>

---

**Q3. Why are closures useful for data privacy?**
<details><summary>Answer</summary>
Variables inside a closure can't be accessed directly from outside — only
through functions you explicitly expose. This mimics "private" variables,
since JavaScript doesn't have true private variables at the language level
(outside of newer `#privateField` class syntax).
</details>

---

## Section 2: IIFE

**Q4. Why does an IIFE need to be wrapped in parentheses?**
<details><summary>Answer</summary>
Without parentheses, `function(){...}()` is parsed as a function
DECLARATION, and function declarations can't be immediately invoked that
way — it causes a syntax error. Wrapping it in `(function(){...})` forces
JavaScript to treat it as an EXPRESSION instead, which can then be called
immediately with `()`.
</details>

---

**Q5. Are IIFEs still necessary in modern JavaScript? Why or why not?**
<details><summary>Answer</summary>
Less necessary than before — `let`/`const` already provide block scope, and
ES6 modules provide file-level scope isolation automatically. IIFEs were
mainly a workaround for `var`'s lack of block scope, so their main
historical use case is largely solved by modern syntax now.
</details>

---

## Section 3: Function Declaration vs Expression

**Q6. Why can you call a function declaration before it appears in the code, but not a function expression?**
<details><summary>Answer</summary>
Function declarations are hoisted COMPLETELY — both the name and the full
function body — so they exist and are callable from the very top of the
scope. Function expressions only hoist the VARIABLE (as undefined, or in
TDZ for let/const) — the actual function body isn't attached until that
line of code runs.
</details>

---

**Q7. What's the benefit of a NAMED function expression over an anonymous one?**
<details><summary>Answer</summary>
A named function expression can safely reference itself internally (using
its inner name) for recursion, without depending on the outer variable
staying unchanged. It also shows a meaningful name in stack traces during
debugging, instead of just "anonymous".
</details>

---

## Section 4: Arrow vs Normal Functions

**Q8. Why can't arrow functions be used as constructors?**
<details><summary>Answer</summary>
Constructors rely on `this` being newly created and bound to the object
being constructed via the `new` keyword. Arrow functions don't have their
own `this` at all — they inherit it from the surrounding scope — so there's
no `this` for `new` to bind to, making them incompatible as constructors.
</details>

---

**Q9. What does this print?**
```js
const obj = {
  value: 10,
  getValue: () => this.value
};
console.log(obj.getValue());
```
<details><summary>Answer</summary>
undefined — the arrow function doesn't get its own `this` bound to `obj`.
It inherits `this` from the outer (global/module) scope instead, where
`value` doesn't exist.
</details>

---

**Q10. When is an arrow function the WRONG choice?**
<details><summary>Answer</summary>
As an object method that needs `this` to refer to the object, or as a
constructor function, or anywhere you specifically need access to the
`arguments` object.
</details>

---

## Section 5: this Keyword

**Q11. Does `this` depend on where a function is defined or how it's called?**
<details><summary>Answer</summary>
How it's called (for regular functions) — this is called "dynamic
binding". Arrow functions are the one exception — they use where they're
DEFINED (lexical binding) instead.
</details>

---

**Q12. What does this print?**
```js
const user = {
  name: "Kavya",
  greet: function () {
    console.log(this.name);
  }
};
const greetFn = user.greet;
greetFn();
```
<details><summary>Answer</summary>
undefined (or throws in strict mode) — even though greet was originally a
method on user, once it's assigned to greetFn and called on its own
(greetFn()), it's just a plain function call. `this` is no longer bound to
user because HOW you call a function determines `this`, not where it was
originally defined.
</details>

---

**Q13. How would you fix Q12 so `greetFn()` still logs "Kavya"?**
<details><summary>Answer</summary>
Use `.bind()`: `const greetFn = user.greet.bind(user);` — this permanently
locks `this` to `user`, regardless of how greetFn is later called.
</details>

---

## Section 6: call, apply, bind

**Q14. In one sentence, when would you choose bind() over call() or apply()?**
<details><summary>Answer</summary>
When you want to save a function with `this` permanently fixed, to be
called LATER (e.g., as an event handler or a callback) — call() and
apply() both invoke the function immediately, they don't let you delay
execution.
</details>

---

**Q15. What does this print?**
```js
function sayHi() {
  console.log("Hi, " + this.name);
}
const boundFn = sayHi.bind({ name: "Kavya" });
const anotherBind = boundFn.bind({ name: "Nagar" });
anotherBind();
```
<details><summary>Answer</summary>
"Hi, Kavya" — once a function is bound with bind(), its `this` is
PERMANENTLY locked. Trying to bind it again to a different object has no
effect; the first bind() always wins.
</details>

---

## Section 7: Higher Order Functions

**Q16. Is every function that takes a callback automatically a Higher Order Function?**
<details><summary>Answer</summary>
Yes — by definition, any function that accepts another function as an
argument (or returns one) qualifies as a HOF, regardless of what else it
does internally.
</details>

---

**Q17. Write a simple HOF that returns a function (don't worry about running it — just explain the structure).**
<details><summary>Answer</summary>
Example: a function `power(exponent)` that returns another function taking
a base number and returning `base ** exponent`. The OUTER function
"configures" the behavior, and the RETURNED function does the actual work
using that configuration — this pattern is called a function factory.
</details>

```js
function power(exponent) {
  return function (base) {
    return base ** exponent;
  };
}
const square = power(2);
console.log(square(5)); // 25
```

---

## Section 8: Callbacks

**Q18. Is a callback always asynchronous?**
<details><summary>Answer</summary>
No — callbacks can be synchronous too (like the callback passed to
`.map()` or `.forEach()`, which runs immediately during the loop) or
asynchronous (like a `setTimeout` or API callback, which runs later).
</details>

---

**Q19. Why do developers avoid deeply nested callbacks even if the code technically works?**
<details><summary>Answer</summary>
Nested callbacks are hard to read (the "pyramid of doom"), make error
handling messy (each level needs its own error check), and are difficult
to modify or debug later — even if functionally correct, the code becomes
unmaintainable as it grows.
</details>

---

## Bonus: Trace-the-Output Challenge Questions

**Q20.** What does this print?
```js
function outer() {
  let x = 10;
  function inner() {
    console.log(x);
  }
  x = 20;
  return inner;
}
const fn = outer();
fn();
```
<details><summary>Answer</summary>
20 — closures capture a REFERENCE to the variable, not a snapshot of its
value at creation time. By the time inner() actually runs, x has already
been updated to 20.
</details>

---

**Q21.** What does this print?
```js
const obj = {
  name: "Kavya",
  regular: function () {
    return () => {
      console.log(this.name);
    };
  }
};
const fn = obj.regular();
fn();
```
<details><summary>Answer</summary>
"Kavya" — the arrow function is defined INSIDE regular(), so it inherits
`this` from regular()'s `this`. Since regular() was called as obj.regular(),
its `this` = obj, and the arrow function "sees" that same `this` when it
runs later.
</details>

---

**Q22.** What does this print?
```js
function greet(greeting) {
  console.log(greeting + ", " + this.name);
}
const person = { name: "Kavya" };
const boundGreet = greet.bind(person, "Hello");
boundGreet();
```
<details><summary>Answer</summary>
"Hello, Kavya" — bind() locks BOTH `this` (to person) AND can pre-fill
arguments ("Hello"), so calling boundGreet() with no further arguments
still has everything it needs.
</details>


---

## Section 9: More Interview Favorites (Advanced)

**Q23. What does this print?**
```js
function outer() {
  var x = 1;
  function inner() {
    console.log(x);
    var x = 2;
  }
  inner();
}
outer();
```
<details><summary>Answer</summary>
undefined — inside inner(), var x is hoisted to the top of inner()'s own
scope, shadowing the outer x completely. So console.log(x) refers to
inner's local x (not yet assigned, hence undefined), NOT outer's x = 1.
</details>

---

**Q24. What's the difference between `function.length` and `arguments.length`?**
<details><summary>Answer</summary>
`function.length` (no parentheses — a property, not a call) tells you how
many parameters a function was DEFINED with. `arguments.length` tells you
how many arguments were actually PASSED when calling it. These can differ
if you call a function with more or fewer arguments than it declares.
</details>

```js
function greet(a, b, c) {
  console.log(arguments.length);
}
greet(1, 2);           // arguments.length = 2
console.log(greet.length); // function.length = 3
```

---

**Q25. Can a closure cause a memory leak? How?**
<details><summary>Answer</summary>
Yes — if a closure keeps a reference to a large object/variable that's no
longer needed, JavaScript's garbage collector can't free that memory,
because the closure is still "holding on" to it. This becomes a real
problem in long-running applications (e.g., event listeners with closures
that are never removed).
</details>

---

**Q26. What is currying, and how does it relate to Higher Order Functions?**
<details><summary>Answer</summary>
Currying transforms a function that takes multiple arguments into a chain
of functions that each take ONE argument. It relies entirely on closures
and HOFs — each inner function "remembers" the previous arguments via
closure, and the outer function returns a function (making it a HOF).
</details>

```js
function add(a) {
  return function (b) {
    return function (c) {
      return a + b + c;
    };
  };
}
console.log(add(1)(2)(3)); // 6
```

---

**Q27. What does this print, and why is it a classic interview trap?**
```js
for (var i = 0; i < 3; i++) {
  setTimeout(function () {
    console.log(i);
  }, 100);
}
```
<details><summary>Answer</summary>
3, 3, 3 — NOT 0, 1, 2. Because `var` is function-scoped (not block-scoped),
all three setTimeout callbacks share the SAME `i`. By the time any of them
actually run (after the loop finishes), `i` has already become 3. This is
one of the most commonly asked closure+loop questions in interviews.
</details>

---

**Q28. How do you fix Q27 so it prints 0, 1, 2 instead?**
<details><summary>Answer</summary>
Two ways: (1) Replace `var` with `let` — since let is block-scoped, each
loop iteration gets its OWN separate `i`. (2) Wrap the setTimeout in an
IIFE that captures the current value of i as a parameter, creating a new
scope per iteration.
</details>

```js
// Fix 1: use let
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
} // 0, 1, 2

// Fix 2: IIFE
for (var i = 0; i < 3; i++) {
  (function (j) {
    setTimeout(() => console.log(j), 100);
  })(i);
} // 0, 1, 2
```

---

**Q29. What does `this` refer to inside a `setInterval` callback written as a regular function versus an arrow function, inside an object method?**
<details><summary>Answer</summary>
Same rule as setTimeout — a regular function callback loses `this` (defaults
to global/undefined), while an arrow function inherits `this` from the
enclosing method, keeping it correctly bound to the object.
</details>

---

**Q30. What's the difference between `Function.prototype.call()` and simply invoking a function normally?**
<details><summary>Answer</summary>
A normal call `fn()` lets JavaScript decide `this` automatically (based on
how it's called). `fn.call(obj)` lets YOU explicitly choose what `this`
should be inside that function call, regardless of how it's normally
invoked — useful for "borrowing" methods from one object to use with
another.
</details>

---

**Q31. Can you use `bind()` on an arrow function? What happens?**
<details><summary>Answer</summary>
You technically CAN call `.bind()` on an arrow function, but it has NO
effect on `this` — since arrow functions don't have their own `this` to
begin with, there's nothing for bind() to override. The arrow function's
this remains lexically inherited regardless.
</details>

---

**Q32. What does this print?**
```js
const person = {
  name: "Kavya",
  friends: ["Nagar", "Priya"],
  printFriends: function () {
    this.friends.forEach(function (friend) {
      console.log(this.name + " is friends with " + friend);
    });
  }
};
person.printFriends();
```
<details><summary>Answer</summary>
"undefined is friends with Nagar" and "undefined is friends with Priya" —
the regular function callback inside forEach() loses `this` (same trap as
setTimeout). Fix: use an arrow function for the forEach callback instead,
so it inherits `this` from printFriends().
</details>
