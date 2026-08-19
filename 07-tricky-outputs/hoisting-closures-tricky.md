## Tricky Outputs: Hoisting + Closures Combined

**Q1.** What does this print?
```js
var funcs = [];
for (var i = 0; i < 3; i++) {
  funcs.push(function () { return i; });
}
console.log(funcs[0](), funcs[1](), funcs[2]());
```
<details><summary>Answer</summary>
3 3 3 — var is function-scoped, so all three closures share the SAME i.
By the time any function actually runs, the loop has finished and i is 3.
</details>

---

**Q2.** What does this print?
```js
function outer() {
  console.log(x);
  var x = 10;
  function inner() {
    console.log(x);
  }
  inner();
}
outer();
```
<details><summary>Answer</summary>
undefined then 10 — var x is hoisted inside outer(), so the first log sees
the hoisted-but-unassigned x (undefined). By the time inner() runs, x has
been assigned 10, and inner's closure sees the updated value.
</details>

---

**Q3.** What does this print?
```js
function outer() {
  let count = 0;
  return function () {
    count++;
    return count;
  };
}
const c1 = outer();
console.log(c1(), c1(), c1());
```
<details><summary>Answer</summary>
1 2 3 — each call to c1() shares the SAME closure over count, since c1
was only created once via outer(). count persists and increments across
calls, unlike creating a new outer() each time.
</details>
