# Extra Topics & Deep-Dive Quiz — Tricky Outputs

More output-prediction questions, going beyond the core 7 files. These mix
multiple concepts together, the way real interviewers often do.

---

**Q1.** What does this print?
```js
console.log(typeof typeof undefined);
```
<details><summary>Answer</summary>
"string" — typeof undefined is "undefined" (a string). typeof "undefined"
is then "string". Any typeof result is always a string, so wrapping
typeof around itself always yields "string".
</details>

---

**Q2.** What does this print?
```js
let a = { value: 1 };
let b = a;
a = { value: 2 };
console.log(b.value);
```
<details><summary>Answer</summary>
1 — reassigning `a` to a brand NEW object doesn't affect `b`, since `b`
still points to the ORIGINAL object. Reassignment ≠ mutation — this is
different from mutating a shared object's property.
</details>

---

**Q3.** What does this print?
```js
function foo() {
  return
  {
    value: 42
  };
}
console.log(foo());
```
<details><summary>Answer</summary>
undefined — Automatic Semicolon Insertion (ASI) silently inserts a
semicolon right after `return` (since it's on its own line), making it
`return;` followed by an unreachable object literal. Classic gotcha: never
put a line break between `return` and the value you're returning.
</details>

---

**Q4.** What does this print?
```js
console.log([1, 2, 3].length = 5);
console.log([1, 2, 3, , ,].length);
```
<details><summary>Answer</summary>
5 then 5 — you can directly SET the length property of an array. Setting
it higher than the current length adds empty "holes" (not actual
undefined values, just missing indices) — arr becomes [1,2,3,<2 empty
items>], and its length is genuinely 5.
</details>

---

**Q5.** What does this print?
```js
const arr = [1, 2, 3];
arr.length = 0;
console.log(arr);
```
<details><summary>Answer</summary>
[] — setting length to 0 is actually a common trick to CLEAR an array in
place (keeping the same reference), unlike arr = [] which creates a
brand new array and breaks any other reference pointing to the old one.
</details>

---

**Q6.** What does this print?
```js
console.log(1 < 2 < 3);
console.log(3 > 2 > 1);
```
<details><summary>Answer</summary>
true then false — operators evaluate LEFT to RIGHT. First: (1<2)=true,
then true<3 → true coerces to 1, and 1<3=true. Second: (3>2)=true, then
true>1 → 1>1=false. Chained comparisons don't work like in math.
</details>

---

**Q7.** What does this print?
```js
console.log([1, 2, 3] == "1,2,3");
```
<details><summary>Answer</summary>
true — arrays get converted to strings when compared with ==. Array's
toString() joins elements with commas, producing "1,2,3", which then
matches the string exactly.
</details>

---

**Q8.** What does this print?
```js
console.log(typeof function () {});
console.log(typeof class Foo {});
console.log(typeof []);
```
<details><summary>Answer</summary>
"function", "function", "object" — functions (including classes, which
are just special functions under the hood) return "function". Arrays,
despite being special, are still fundamentally "object" per typeof.
</details>

---

**Q9.** What does this print?
```js
function Person(name) {
  this.name = name;
}
const p = Person("Kavya"); // note: called WITHOUT `new`
console.log(p);
console.log(typeof window !== 'undefined' ? window.name : global.name);
```
<details><summary>Answer</summary>
undefined, then "Kavya" (in a browser) — without `new`, Person() is
called as a REGULAR function, so `this` refers to the global object
(window in browsers), not a new object. Person() has no explicit return,
so p is undefined, but this.name = name accidentally set window.name
globally — a classic mistake showing why forgetting `new` is dangerous.
</details>

---

**Q10.** What does this print?
```js
console.log(Boolean(""));
console.log(Boolean("0"));
console.log(Boolean(" "));
console.log(Boolean([]));
console.log(Boolean({}));
```
<details><summary>Answer</summary>
false, true, true, true, true — only "" (truly empty string) is falsy.
"0" is a non-empty string, so truthy (a very common trap). " " (a space)
is also non-empty. Empty array/object are always truthy in JS.
</details>

---

**Q11.** What does this print?
```js
async function getData() {
  throw new Error("failed");
}
getData().catch(err => console.log(err.message));
console.log("after call");
```
<details><summary>Answer</summary>
"after call" then "failed" — throwing inside an async function makes the
returned Promise REJECT (it's automatically caught and converted). The
.catch() runs asynchronously (as a microtask), so the synchronous
"after call" log happens first.
</details>

---

**Q12.** What does this print?
```js
const obj = {
  a: 1,
  getA() {
    return this.a;
  }
};
const { getA } = obj;
console.log(getA());
```
<details><summary>Answer</summary>
undefined (or throws in strict mode) — destructuring getA out of obj
loses its connection to obj. When called standalone as getA(), `this` is
no longer bound to obj — same trap as assigning a method to a variable
and calling it separately.
</details>

---

**Q13.** What does this print?
```js
console.log(Object.freeze === Object.freeze);
console.log([1,2,3] === [1,2,3]);
console.log({} === {});
```
<details><summary>Answer</summary>
true, false, false — functions ARE reference-equal to themselves (same
function object). But two SEPARATELY created arrays/objects, even with
identical content, are always different references — never equal with
===, since objects compare by reference, not by structural content.
</details>

---

**Q14.** What does this print?
```js
let x = 10;
function outer() {
  console.log(x);
  let x = 20;
}
outer();
```
<details><summary>Answer</summary>
❌ ReferenceError: Cannot access 'x' before initialization — even though
an OUTER x exists at 10, the LOCAL `let x` inside outer() creates its own
binding for the entire function scope, and that local x is in the TDZ
until its declaration line runs. This shadows the outer x entirely,
unlike the var+hoisting case which would just print undefined.
</details>

---

**Q15.** What does this print?
```js
console.log("5" - "2");
console.log("5" + -"2");
console.log("5" - -"2");
```
<details><summary>Answer</summary>
3, "5-2", 7 — first: both strings coerce to numbers with -, giving 3.
Second: unary minus converts "2" to -2 (a number), but + then sees a
string ("5") and a number, so it coerces -2 back into a string "-2" and
concatenates: "5" + "-2" = "5-2". Third: "5" - -"2" is "5" minus negative
2, and - always forces numeric conversion: 5 - (-2) = 7.
</details>
