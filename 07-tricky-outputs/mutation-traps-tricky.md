## Tricky Outputs: Array/Object Mutation Traps

**Q1.** What does this print?
```js
const arr1 = [1, 2, 3];
const arr2 = arr1;
arr2.push(4);
console.log(arr1);
```
<details><summary>Answer</summary>
[1, 2, 3, 4] — arr2 = arr1 doesn't copy the array, just creates a second
reference to the SAME array. Mutating arr2 mutates arr1 too.
</details>

---

**Q2.** What does this print?
```js
const obj1 = { name: "Kavya", details: { age: 21 } };
const obj2 = { ...obj1 };
obj2.details.age = 25;
console.log(obj1.details.age);
```
<details><summary>Answer</summary>
25 — spread only creates a SHALLOW copy. The nested "details" object is
still the SAME reference in both obj1 and obj2, so mutating it through
obj2 affects obj1 too.
</details>

---

**Q3.** What does this print?
```js
function addItem(arr) {
  arr.push("new item");
}
const list = ["a", "b"];
addItem(list);
console.log(list);
```
<details><summary>Answer</summary>
["a", "b", "new item"] — arrays are reference types, so passing `list`
into the function passes a reference to the SAME array, not a copy.
Mutating it inside the function affects the original outside too.
</details>
