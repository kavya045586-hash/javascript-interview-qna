# Extra Topics & Deep-Dive Quiz — Objects & Arrays

Every question explains the underlying concept. Cover the answer with your
hand and try to explain it in your own words first.

---

## Section 1: Object Creation & Shorthand

**Q1. What's the actual benefit of computed property names beyond convenience?**
<details><summary>Answer</summary>
They let you build object keys dynamically at runtime based on variables —
essential for things like building objects from form field names, API
responses, or loop-generated data where you don't know the key names in
advance.
</details>

```js
function buildSettings(key, value) {
  return { [key]: value };
}
console.log(buildSettings("theme", "dark")); // { theme: "dark" }
```

---

**Q2. What does this print?**
```js
const a = 1, b = 2;
const obj = { a, b, sum: a + b };
console.log(obj);
```
<details><summary>Answer</summary>
{ a: 1, b: 2, sum: 3 } — property shorthand works for a and b (matching
variable names), while sum uses a regular key-value pair since it's a
computed expression, not a direct variable reference.
</details>

---

## Section 2: Destructuring

**Q3. What does this print, and why?**
```js
const { a, b, ...rest } = { a: 1, b: 2, c: 3, d: 4 };
console.log(a, b, rest);
```
<details><summary>Answer</summary>
1 2 { c: 3, d: 4 } — the rest operator (...rest) collects all REMAINING
properties not already destructured into a, b, into a new object. This is
commonly used to separate a few known fields from the rest of an object.
</details>

---

**Q4. Can you swap two variables using destructuring, without a temp variable?**
<details><summary>Answer</summary>
Yes — a classic interview trick using array destructuring.
</details>

```js
let x = 1, y = 2;
[x, y] = [y, x];
console.log(x, y); // 2 1
```

---

**Q5. What happens if you try to destructure a property that doesn't exist, with no default value?**
<details><summary>Answer</summary>
You get undefined, not an error — destructuring a missing property behaves
just like accessing a missing property normally (obj.missing → undefined).
</details>

```js
const { missing } = { a: 1 };
console.log(missing); // undefined
```

---

## Section 3: Shallow vs Deep Copy

**Q6. Does `Array.prototype.slice()` create a shallow or deep copy?**
<details><summary>Answer</summary>
Shallow — it copies the top-level array elements, but if those elements
are objects/arrays themselves, they're still shared by reference, same
issue as the spread operator.
</details>

---

**Q7. Why does `JSON.parse(JSON.stringify(obj))` still count as a "deep" copy despite its limitations?**
<details><summary>Answer</summary>
Because it fully re-serializes the ENTIRE object structure into a string
and rebuilds it from scratch — nested objects/arrays become genuinely new,
independent objects, not shared references. Its limitations (dropping
functions, Date becoming a string) are separate from whether it's
"shallow vs deep" — it's deep, just imperfect for certain data types.
</details>

---

## Section 4: Prototypes & Inheritance

**Q8. What does `Object.getPrototypeOf(obj)` do?**
<details><summary>Answer</summary>
Returns the prototype object that `obj` is linked to — the modern,
recommended way to inspect an object's prototype, replacing the older
`obj.__proto__` syntax.
</details>

---

**Q9. What does this print?**
```js
function Animal() {}
Animal.prototype.sound = "generic sound";

const dog = new Animal();
dog.sound = "bark";

console.log(dog.sound);
delete dog.sound;
console.log(dog.sound);
```
<details><summary>Answer</summary>
"bark" then "generic sound" — dog.sound = "bark" creates an OWN property
on dog that shadows the prototype's version. Deleting it removes dog's own
property, so the lookup falls back to the prototype chain, revealing
"generic sound" again.
</details>

---

## Section 5: Classes, Constructor, super

**Q10. Can a class method be an arrow function? What changes if it is?**
<details><summary>Answer</summary>
Yes, using class field syntax. The key difference: a regular class method
is on the PROTOTYPE (shared across instances), while an arrow function
class field is created FRESH on each instance and automatically binds
`this` to that instance — useful for event handlers where `this` binding
often gets lost.
</details>

```js
class Button {
  handleClick = () => {  // arrow function class field
    console.log(this);   // always bound to the instance, even if passed as a callback
  };
}
```

---

**Q11. What happens if a subclass doesn't define its own constructor?**
<details><summary>Answer</summary>
JavaScript automatically provides a default constructor that just calls
`super(...args)`, passing along whatever arguments were given — so the
parent class's constructor still runs normally.
</details>

---

## Section 6: Getters/Setters & Static Methods

**Q12. Can you have a getter without a corresponding setter?**
<details><summary>Answer</summary>
Yes — a getter-only property becomes effectively read-only. Trying to
assign to it either silently fails (non-strict mode) or throws a
TypeError (strict mode), since there's no setter to handle the write.
</details>

---

**Q13. Can static methods access instance properties (like `this.name` on a specific object)?**
<details><summary>Answer</summary>
No — static methods belong to the class itself, not to any instance, so
they have no access to instance-specific data via `this`. Inside a static
method, `this` refers to the class itself, not an object created from it.
</details>

---

## Section 7: Array map/filter/reduce

**Q14. Can `reduce()` be used to implement `map()` or `filter()`? How?**
<details><summary>Answer</summary>
Yes — reduce() is the most flexible of the three, since map and filter can
both be expressed as specific patterns of accumulation.
</details>

```js
const numbers = [1, 2, 3, 4];

// map via reduce
const doubled = numbers.reduce((acc, n) => [...acc, n * 2], []);

// filter via reduce
const evens = numbers.reduce((acc, n) => n % 2 === 0 ? [...acc, n] : acc, []);
```

---

**Q15. What does this print?**
```js
const result = [1, 2, 3].map(n => n * 2).filter(n => n > 2).reduce((a, b) => a + b);
console.log(result);
```
<details><summary>Answer</summary>
14 — map doubles to [2,4,6], filter keeps values >2 → [4,6], reduce sums
them (no initial value given, so it starts from the first element) → 10... 
wait, let's trace carefully: [2,4,6] filtered by >2 gives [4,6], reduce
(a,b)=>a+b with no initial value starts acc=4, then 4+6=10. Correct answer: 10.
</details>

---

## Section 8: Array forEach/find/some/every

**Q16. Can you break out of a forEach() loop early, like you can with a regular for loop?**
<details><summary>Answer</summary>
No — forEach() always runs through every element; return inside its
callback only skips to the next iteration, it doesn't stop the loop. If
you need early exit, use a regular for loop, or use some()/every() (which
DO stop early when their condition is met).
</details>

---

**Q17. What does this print?**
```js
const users = [{ id: 1, active: true }, { id: 2, active: false }];
console.log(users.some(u => u.active));
console.log(users.every(u => u.active));
```
<details><summary>Answer</summary>
true then false — some() finds at least one active user (id:1), so true.
every() requires ALL to be active, but id:2 isn't, so false.
</details>

---

## Section 9: JSON.stringify/parse

**Q18. What does the third argument of JSON.stringify actually control, beyond pretty-printing?**
<details><summary>Answer</summary>
It's the "space" argument for indentation — can be a number (spaces per
level) or a string (like "\t" for tabs). The SECOND argument (replacer) is
what actually controls WHICH properties get included, or transforms
values during stringification.
</details>

```js
const obj = { name: "Kavya", password: "secret" };
console.log(JSON.stringify(obj, ["name"], 2));
// only includes "name", excludes "password" — replacer filters as an array of allowed keys
```

---

**Q19. What does JSON.parse do if given invalid JSON?**
<details><summary>Answer</summary>
Throws a SyntaxError — this is why JSON.parse should always be wrapped in
try/catch when parsing data from an external/untrusted source (like an API
response or user input), since malformed JSON will crash the program
otherwise.
</details>

---

## Bonus: Trace-the-Output Challenge Questions

**Q20.** What does this print?
```js
const arr = [1, [2, 3], [4, [5, 6]]];
const flat = JSON.parse(JSON.stringify(arr));
console.log(flat[2][1][0]);
```
<details><summary>Answer</summary>
5 — JSON.stringify/parse performs a deep copy, so nested arrays are fully
reconstructed. arr[2] is [4,[5,6]], so arr[2][1] is [5,6], and [0] of that
is 5.
</details>

---

**Q21.** What does this print?
```js
class Counter {
  static count = 0;
  constructor() {
    Counter.count++;
  }
}
new Counter();
new Counter();
new Counter();
console.log(Counter.count);
```
<details><summary>Answer</summary>
3 — static properties are shared across ALL instances (belong to the
class itself), so each new Counter() increments the SAME Counter.count,
not a separate copy per instance.
</details>

---

**Q22.** What does this print?
```js
const obj = { a: 1, b: 2 };
const { a, ...rest } = obj;
rest.c = 3;
console.log(obj);
console.log(rest);
```
<details><summary>Answer</summary>
{ a: 1, b: 2 } then { b: 2, c: 3 } — the rest operator creates a genuinely
NEW object, so modifying rest afterward does NOT affect the original obj.
This is different from direct reference assignment.
</details>
