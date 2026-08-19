## Tricky Outputs: Loop + Closure (var vs let)

**Q1.** What does this print?
```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
```
<details><summary>Answer</summary>
3, 3, 3 — var is function-scoped, so all three callbacks share ONE i.
By the time any callback runs (after the loop finishes), i is already 3.
</details>

---

**Q2.** What does this print?
```js
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
```
<details><summary>Answer</summary>
0, 1, 2 — let is block-scoped, so EACH loop iteration gets its own fresh
i. Every callback closes over its own separate copy.
</details>

---

**Q3.** What does this print?
```js
for (var i = 0; i < 3; i++) {
  (function (j) {
    setTimeout(() => console.log(j), 100);
  })(i);
}
```
<details><summary>Answer</summary>
0, 1, 2 — the IIFE captures the CURRENT value of i as its own parameter j
on each iteration, creating a new scope per iteration — this was the
classic pre-ES6 fix for the var/closure loop problem, before let existed.
</details>
