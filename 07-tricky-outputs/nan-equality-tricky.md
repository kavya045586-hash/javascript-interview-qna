## Tricky Outputs: NaN & Equality Edge Cases

**Q1.** What does this print?
```js
console.log(NaN === NaN);
console.log([NaN].includes(NaN));
console.log([NaN].indexOf(NaN));
```
<details><summary>Answer</summary>
false, true, -1 — NaN never equals itself with ===. Array.includes() uses
a special algorithm (SameValueZero) that correctly detects NaN, but
indexOf() uses strict equality internally, so it fails to find NaN
(always returns -1 for it).
</details>

---

**Q2.** What does this print?
```js
console.log(typeof NaN);
console.log(Number.isNaN("hello"));
console.log(isNaN("hello"));
```
<details><summary>Answer</summary>
"number", false, true — NaN is technically type "number". Number.isNaN
is strict — it only returns true for the ACTUAL NaN value, not for things
that merely coerce to NaN. Global isNaN() coerces "hello" to NaN first,
then checks, giving a misleading true.
</details>

---

**Q3.** What does this print?
```js
console.log(0 == "0");
console.log(0 == "");
console.log(0 == false);
console.log("" == false);
console.log(null == undefined);
console.log(null == 0);
```
<details><summary>Answer</summary>
true, true, true, true, true, false — the first four all involve coercion
to 0/false equivalence. null == undefined is a special case (they equal
each other only). null == 0 is FALSE — null does NOT coerce to 0 in loose
equality, a common trap.
</details>
