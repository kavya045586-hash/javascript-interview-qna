## Q: Event Bubbling vs Capturing

**Answer:**

When an event fires on a nested element, it doesn't just affect that
element — it travels through the DOM tree in a specific order. There are
two phases:

- **Capturing (trickle down)** — event travels from the outermost ancestor
  DOWN to the target element
- **Bubbling (bubble up)** — event travels from the target element UP
  through its ancestors, back to the outermost element

By default, event listeners run during the BUBBLING phase.

**Example:**

```html
<div id="outer">
  <div id="inner">
    <button id="btn">Click me</button>
  </div>
</div>
```

```js
document.getElementById("outer").addEventListener("click", () => {
  console.log("Outer clicked");
});
document.getElementById("inner").addEventListener("click", () => {
  console.log("Inner clicked");
});
document.getElementById("btn").addEventListener("click", () => {
  console.log("Button clicked");
});
```

Clicking the button logs, in order:
Button clicked
Inner clicked
Outer clicked

This is bubbling — event starts at the target (button) and bubbles UP.

**Using capturing instead (third argument = true):**

```js
document.getElementById("outer").addEventListener("click", () => {
  console.log("Outer clicked (capturing)");
}, true); // true = capturing phase
```

If ALL listeners used capturing, the order would reverse: Outer → Inner → Button.

**Stopping propagation:**

```js
document.getElementById("btn").addEventListener("click", (e) => {
  e.stopPropagation(); // prevents the event from bubbling further up
  console.log("Button clicked, propagation stopped");
});
```

**Follow-up questions interviewers ask:**

- What's the default phase for event listeners? (Bubbling)
- What does `stopPropagation()` do, and when would you use it?
