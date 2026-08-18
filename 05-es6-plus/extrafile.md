# Extra Topics & Deep-Dive Quiz — ES6+

Every question explains the underlying concept. Cover the answer with your
hand and try to explain it in your own words first.

---

## Section 1: Modules (import/export)

**Q1. What happens if two files both import the same module?**
<details><summary>Answer</summary>
The module only RUNS once — JS caches the module after the first import.
Every subsequent import gets a reference to the SAME already-executed
module, not a fresh re-run. This is why module-level state (like a
counter) stays shared across every file that imports it.
</details>

---

**Q2. Can you have circular imports (A imports B, and B imports A)?**
<details><summary>Answer</summary>
Technically yes, but it's risky — depending on load order, one of the two
modules may get an incomplete/undefined version of what it imports from
the other, since neither has finished executing yet. Best practice is to
avoid circular dependencies through better code structure.
</details>

---

## Section 2: Optional Chaining & Nullish Coalescing

**Q3. What does this print?**
```js
const user = { name: "Kavya", age: 0 };
console.log(user.age || "Unknown");
console.log(user.age ?? "Unknown");
```
<details><summary>Answer</summary>
"Unknown" then 0 — || treats 0 as falsy and incorrectly falls back to the
default. ?? only falls back for null/undefined, so it correctly preserves
the valid value 0.
</details>

---

**Q4. Does optional chaining work with function calls that don't exist?**
<details><summary>Answer</summary>
Yes — obj.someMethod?.() safely does nothing and returns undefined if
someMethod doesn't exist, instead of throwing "someMethod is not a
function".
</details>

---

## Section 3: Map/Set vs Object/Array

**Q5. What does `new Set([1, "1", 1])` contain, and why?**
<details><summary>Answer</summary>
Set(2) {1, "1"} — Set uses strict equality (===) to check duplicates, so
the number 1 and the string "1" are considered DIFFERENT values, even
though they look similar. Only the second `1` (duplicate number) gets
removed.
</details>

---

**Q6. How do you convert a Map back into a plain array of [key, value] pairs?**
<details><summary>Answer</summary>
Spread it directly, since Map is iterable by default — [...map] gives an
array of [key, value] pairs, matching how the Map was constructed in the
first place.
</details>

```js
const map = new Map([["a", 1], ["b", 2]]);
console.log([...map]); // [["a", 1], ["b", 2]]
```

---

## Section 4: Spread vs Rest

**Q7. What does this print?**
```js
function example(a, b, ...rest) {
  console.log(a, b, rest);
}
example(1, 2, 3, 4, 5);
```
<details><summary>Answer</summary>
1 2 [3, 4, 5] — a and b capture the first two arguments individually, and
rest collects everything remaining into an array. Rest must always come
last in the parameter list.
</details>

---

**Q8. Can you spread a string?**
<details><summary>Answer</summary>
Yes — strings are iterable, so spreading one breaks it into an array of
individual characters.
</details>

```js
console.log([..."hello"]); // ["h", "e", "l", "l", "o"]
```

---

## Section 5: String Methods & Template Literals

**Q9. What does this print?**
```js
console.log("5" + 3);
console.log(`${5 + 3}`);
```
<details><summary>Answer</summary>
"53" then "8" — plain string concatenation with + coerces 3 to a string
first. But inside a template literal's ${}, the expression 5+3 is
evaluated as MATH first (both are numbers), and only the final RESULT (8)
gets converted to a string for insertion.
</details>

---

**Q10. Can you call a function inside a template literal?**
<details><summary>Answer</summary>
Yes — any valid JS expression works inside ${}, including function calls.
</details>

```js
function shout(str) { return str.toUpperCase() + "!"; }
console.log(`${shout("hello")}`); // "HELLO!"
```

---

## Section 6: Generators & Iterators

**Q11. What does `gen.next(value)` do when you pass an argument?**
<details><summary>Answer</summary>
The passed value becomes the RESULT of the yield expression that the
generator is currently paused on — allowing two-way communication between
the generator and whoever is calling .next().
</details>

```js
function* echo() {
  const received = yield "first pause";
  console.log(received); // logs whatever is passed to the SECOND .next() call
}
const gen = echo();
gen.next();          // { value: "first pause", done: false }
gen.next("hello");    // logs "hello", generator resumes with received = "hello"
```

---

**Q12. What's the difference between `return` and `yield` inside a generator?**
<details><summary>Answer</summary>
yield pauses execution and can be resumed later with the value still
accessible via .next(); return ends the generator completely — sets
done:true immediately, and no further .next() calls will produce new
values.
</details>

---

## Section 7: WeakMap & WeakSet

**Q13. Can you use a WeakSet to track which objects have been "processed", without causing a memory leak?**
<details><summary>Answer</summary>
Yes — this is a very common real use case. Add objects to a WeakSet as
they're processed; check with .has() before reprocessing. Since it's
"weak", if the original object is later discarded elsewhere in the
program, it's automatically removed from the WeakSet too — no manual
cleanup needed.
</details>

```js
const processed = new WeakSet();
function processUser(user) {
  if (processed.has(user)) return;
  processed.add(user);
  // ... do processing
}
```

---

**Q14. Why can't primitives (strings, numbers) be used as WeakMap keys?**
<details><summary>Answer</summary>
Primitives aren't garbage collected the same way objects are — they don't
have a unique memory reference that can become "unreachable." WeakMap's
entire mechanism relies on tracking object references specifically, so
allowing primitive keys would break its core purpose.
</details>

---

## Bonus: Trace-the-Output Challenge Questions

**Q15.** What does this print?
```js
const obj = { a: 1, b: 2, c: 3 };
const { a, ...rest } = obj;
console.log(JSON.stringify(rest));
```
<details><summary>Answer</summary>
{"b":2,"c":3} — rest (via the rest operator in destructuring) collects
every property except a into a new object.
</details>

---

**Q16.** What does this print?
```js
function* gen() {
  yield 1;
  yield 2;
  return 3;
  yield 4; // unreachable
}
console.log([...gen()]);
```
<details><summary>Answer</summary>
[1, 2] — spreading a generator only collects values until done becomes
true. return 3 ends the generator (setting done:true), so the value 3
itself is NOT included in the spread result, and yield 4 never even runs.
</details>

---

**Q17.** What does this print?
```js
const settings = null;
const theme = settings?.theme ?? "light";
console.log(theme);
```
<details><summary>Answer</summary>
"light" — settings?.theme safely evaluates to undefined (since settings
is null), and ?? then supplies the default "light" because undefined
qualifies as nullish.
</details>
