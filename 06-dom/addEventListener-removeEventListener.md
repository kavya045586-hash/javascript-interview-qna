## Q: addEventListener / removeEventListener

**Answer:**

`addEventListener` attaches an event handler to an element without
overwriting any existing handlers (unlike the older `onclick = fn` style,
which can only hold ONE handler at a time).

```js
function handleClick() {
  console.log("Clicked!");
}

const btn = document.getElementById("btn");
btn.addEventListener("click", handleClick);
```

**Multiple listeners on the same event — all run:**

```js
btn.addEventListener("click", () => console.log("First handler"));
btn.addEventListener("click", () => console.log("Second handler"));
// Clicking runs BOTH — this wouldn't be possible with onclick = fn
```

**removeEventListener — must reference the EXACT same function:**

```js
btn.removeEventListener("click", handleClick); // ✅ works — same function reference

btn.addEventListener("click", () => console.log("hi"));
btn.removeEventListener("click", () => console.log("hi")); // ❌ does NOT work — different function reference (even though it looks identical)
```

**Why removeEventListener often "fails" — the classic gotcha:** anonymous
functions can never be removed later, because each time you write
`() => {}`, it creates a NEW function reference. You must store the
function in a variable to be able to remove it later.

**Using options — `once` and `capture`:**

```js
btn.addEventListener("click", handleClick, { once: true }); // auto-removes after firing once
```

**Follow-up questions interviewers ask:**

- Why doesn't removeEventListener work with anonymous functions?
- Why is addEventListener preferred over `element.onclick = fn`? (Allows multiple listeners on the same event, and separates concerns better than a single override-able property)
