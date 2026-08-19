## Q: Performance — Reflow and Repaint

**Answer:**

When the browser renders a page, changes to the DOM can trigger two
expensive processes:

- **Reflow (Layout)** — recalculating the POSITION and SIZE of elements
  (triggered by things like changing width, adding/removing elements,
  changing font size)
- **Repaint** — redrawing PIXELS on screen (triggered by things like
  changing color, visibility — WITHOUT affecting layout/position)

Reflow is more expensive than repaint, since it can cascade — a layout
change in one element can force recalculating the layout of its
neighbors/children/ancestors too.

**What triggers reflow (expensive):**

```js
element.style.width = "200px";     // changes layout → reflow
element.style.display = "none";     // removes from layout → reflow
element.offsetHeight;                 // reading this FORCES a reflow to get an accurate answer
```

**What triggers only repaint (cheaper):**

```js
element.style.color = "red";       // just visual — repaint only
element.style.backgroundColor = "blue"; // just visual — repaint only
```

**How to minimize reflows — batch DOM changes:**

```js
// ❌ Bad — triggers multiple reflows, one per change
element.style.width = "100px";
element.style.height = "100px";
element.style.margin = "10px";

// ✅ Better — batch into one class change, ONE reflow
element.classList.add("new-size-class");
```

**Avoiding forced synchronous layout (a subtle performance trap):**

```js
// ❌ Bad — reading layout property forces reflow, then writing again forces ANOTHER
for (let i = 0; i < items.length; i++) {
  items[i].style.width = items[i].offsetWidth + 10 + "px"; // read then write, repeatedly
}

// ✅ Better — batch all reads first, then all writes
const widths = items.map(item => item.offsetWidth); // all reads together
items.forEach((item, i) => item.style.width = widths[i] + 10 + "px"); // all writes together
```

**Follow-up questions interviewers ask:**

- Why is reflow more expensive than repaint? (Reflow can cascade — recalculating layout for related elements too, while repaint just redraws pixels without recalculating positions)
- What's a "forced synchronous layout" and how do you avoid it? (Reading a layout property right after a write forces the browser to reflow immediately instead of batching; fix by separating all reads from all writes)
