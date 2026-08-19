## Tricky Outputs: this in Callbacks/Arrow Functions

**Q1.** What does this print?
```js
const obj = {
  name: "Kavya",
  greet: function () {
    setTimeout(function () {
      console.log(this.name);
    }, 100);
  }
};
obj.greet();
```
<details><summary>Answer</summary>
undefined — the regular function passed to setTimeout loses `this`
binding to obj; setTimeout calls it as a plain function, so this defaults
to global/undefined.
</details>

---

**Q2.** What does this print?
```js
const obj = {
  name: "Kavya",
  greet: function () {
    setTimeout(() => {
      console.log(this.name);
    }, 100);
  }
};
obj.greet();
```
<details><summary>Answer</summary>
"Kavya" — the arrow function has no own this, so it inherits this from
greet()'s scope, where this correctly equals obj.
</details>

---

**Q3.** What does this print?
```js
const obj = {
  name: "Kavya",
  greet: () => {
    console.log(this.name);
  }
};
obj.greet();
```
<details><summary>Answer</summary>
undefined — greet itself is an arrow function, so it never gets its own
this bound to obj at all; it inherits this from the outer (module/global)
scope, where name doesn't exist.
</details>
