## Tricky Outputs: Type Coercion in Operations

**Q1.** What does this print?
```js
console.log(1 + "2" + 3);
console.log(1 + 2 + "3");
```
<details><summary>Answer</summary>
"123" then "33" — left-to-right evaluation matters. First: 1+"2" = "12"
(string), then "12"+3 = "123". Second: 1+2 = 3 (both numbers, math
happens first), then 3+"3" = "33" (now string concatenation).
</details>

---

**Q2.** What does this print?
```js
console.log([] + []);
console.log([] + {});
console.log([1,2] + [3,4]);
```
<details><summary>Answer</summary>
"" then "[object Object]" then "1,23,4" — arrays/objects get converted to
strings when used with +. Empty arrays become "". {} becomes
"[object Object]". Arrays with elements become comma-joined strings, then
concatenated: "1,2" + "3,4" = "1,23,4".
</details>

---

**Q3.** What does this print?
```js
console.log(+"5");
console.log(+true);
console.log(+"");
console.log(+"abc");
```
<details><summary>Answer</summary>
5, 1, 0, NaN — the unary + operator forces numeric conversion. "5"→5,
true→1, empty string→0, and non-numeric strings→NaN (can't be converted).
</details>
