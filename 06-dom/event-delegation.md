## Q: Event Delegation

**Answer:**

Event delegation is a pattern that takes advantage of bubbling: instead of
attaching an event listener to EVERY individual child element, you attach
ONE listener to a common parent, and use `event.target` to figure out
which child was actually clicked.

**Without delegation (inefficient for many items):**

```js
document.querySelectorAll("li").forEach(item => {
  item.addEventListener("click", () => console.log("Clicked:", item.textContent));
});
// Problem: if new <li> items are added later, they won't have this listener attached
```

**With delegation (efficient, and works for dynamically added elements too):**

```js
document.getElementById("list").addEventListener("click", (e) => {
  if (e.target.tagName === "LI") {
    console.log("Clicked:", e.target.textContent);
  }
});
// Works even for <li> elements added to the list AFTER this listener was set up
```

**Why this matters:**
1. **Performance** — one listener instead of hundreds, especially for long lists
2. **Works with dynamic content** — new elements added later are automatically covered, since the listener is on the stable parent, not the (potentially newly-created) children

**Follow-up questions interviewers ask:**

- Why is event delegation more efficient than attaching individual listeners?
- What DOM property helps you identify exactly which child element was clicked? (`event.target`)
