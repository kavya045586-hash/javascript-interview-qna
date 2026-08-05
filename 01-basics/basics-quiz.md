# Basics Quiz — Real Interview Style

Try answering WITHOUT running the code first.

---

**Q1.** What does this print?
```js
console.log(typeof typeof 1);
```
<details><summary>Answer</summary>
"string" — typeof 1 is "number" (a string), then typeof "number" is "string"
</details>

---

**Q2.** What does this print?
```js
console.log(1 + "1");
console.log(1 - "1");
```
<details><summary>Answer</summary>
"11" and 0 — + triggers string concatenation, - forces numeric conversion
</details>

---

**Q3.** What does this print?
```js
var x = 10;
function test() {
  console.log(x);
  var x = 20;
}
test();
```
<details><summary>Answer</summary>
undefined — var x inside test() is hoisted (as undefined), shadowing the outer x
</details>

---

**Q4.** What does this print?
```js
console.log(0.1 + 0.2 === 0.3);
```
<details><summary>Answer</summary>
false — floating point precision issue
</details>

---

**Q5.** What does this print?
```js
let a = [1, 2, 3];
let b = a;
b.push(4);
console.log(a);
```
<details><summary>Answer</summary>
[1, 2, 3, 4] — arrays are reference types, b and a point to the same array
</details>

---

**Q6.** What does this print?
```js
console.log(NaN === NaN);
console.log(Number.isNaN(NaN));
```
<details><summary>Answer</summary>
false and true — NaN never equals itself; Number.isNaN correctly detects it
</details>

---

**Q7.** What does this print?
```js
let str = "hello";
str[0] = "H";
console.log(str);
```
<details><summary>Answer</summary>
"hello" — strings are immutable
</details>

---

**Q8.** What does this print?
```js
console.log("5" == 5);
console.log("5" === 5);
console.log(null == undefined);
console.log(null === undefined);
```
<details><summary>Answer</summary>
true, false, true, false
</details>

---

**Q9.** What does this print?
```js
if ([]) {
  console.log("truthy");
} else {
  console.log("falsy");
}
```
<details><summary>Answer</summary>
"truthy" — empty array is not in the 6 falsy values
</details>

---

**Q10.** What does this print?
```js
console.log(typeof null);
```
<details><summary>Answer</summary>
"object" — a long-standing JS bug kept for backward compatibility
</details>
