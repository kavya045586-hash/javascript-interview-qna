## Q: DOM Manipulation Basics

**Answer:**

The DOM (Document Object Model) is a tree structure representing your
HTML page in memory. JavaScript can read and modify it to make pages
interactive.

**Selecting elements:**

```js
document.getElementById("myId");
document.querySelector(".myClass");       // first match
document.querySelectorAll(".myClass");     // ALL matches (NodeList)
```

**Creating and adding elements:**

```js
const newDiv = document.createElement("div");
newDiv.textContent = "Hello!";
document.body.appendChild(newDiv);
```

**Modifying content:**

```js
const el = document.getElementById("myId");
el.textContent = "New text";      // sets plain text (safe from XSS)
el.innerHTML = "<b>Bold text</b>"; // sets HTML (can execute scripts if used carelessly)
```

**Modifying styles and classes:**

```js
el.style.color = "blue";
el.classList.add("active");
el.classList.remove("hidden");
el.classList.toggle("open");
```

**Removing elements:**

```js
el.remove(); // modern way
// or
el.parentNode.removeChild(el); // older way, still works
```

**Follow-up questions interviewers ask:**

- What's the difference between `textContent` and `innerHTML`? (textContent is safer — treats content as plain text, no HTML parsing; innerHTML parses HTML, which can be a security risk if inserting untrusted user input)
- Why is `querySelectorAll` returning a NodeList, not an array, sometimes tricky? (NodeList has limited methods — no `.map()` directly, though `.forEach()` works; convert with `Array.from()` if you need full array methods)
