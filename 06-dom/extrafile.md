# Extra Topics & Deep-Dive Quiz — DOM

Every question explains the underlying concept. Cover the answer with your
hand and try to explain it in your own words first.

---

## Section 1: Event Bubbling vs Capturing

**Q1. What does `event.currentTarget` mean, and how is it different from `event.target`?**
<details><summary>Answer</summary>
event.target is the element that ACTUALLY triggered the event (e.g., the
specific button clicked). event.currentTarget is the element the listener
is ATTACHED to — during bubbling, these can differ, since a listener on a
parent still fires even though the click originated on a child.
</details>

---

**Q2. Can you have both bubbling and capturing listeners on the same element for the same event?**
<details><summary>Answer</summary>
Yes — addEventListener's third argument (true/false, or {capture: true})
independently controls which phase that specific listener fires in. You
could attach one listener for capturing and another for bubbling on the
exact same element.
</details>

---

## Section 2: Event Delegation

**Q3. Does event delegation work if the child element itself has NO event listener directly attached?**
<details><summary>Answer</summary>
Yes — that's the entire point. The child doesn't need its own listener at
all; the click event still bubbles up to the parent (which DOES have a
listener), and event.target tells the parent exactly which child was
clicked.
</details>

---

**Q4. What's a real limitation of event delegation?**
<details><summary>Answer</summary>
Some events don't bubble at all (like `focus` and `blur` in their basic
form), so delegation won't work for them directly — you'd need the
capturing-phase equivalents (`focusin`/`focusout`) instead, which DO bubble.
</details>

---

## Section 3: DOM Manipulation Basics

**Q5. Why is directly manipulating the DOM in a loop considered a performance concern?**
<details><summary>Answer</summary>
Each DOM change can trigger a "reflow" (recalculating layout) and "repaint"
(redrawing pixels), which is expensive. Doing this repeatedly inside a
loop (e.g., appendChild one item at a time) forces the browser to redo
this work many times. Batching changes (e.g., building a DocumentFragment
first, then appending once) is much faster.
</details>

```js
const fragment = document.createDocumentFragment();
for (let i = 0; i < 100; i++) {
  const li = document.createElement("li");
  li.textContent = "Item " + i;
  fragment.appendChild(li); // adds to fragment, NOT the real DOM yet
}
document.getElementById("list").appendChild(fragment); // ONE real DOM update
```

---

**Q6. What's the difference between `innerHTML` and `insertAdjacentHTML`?**
<details><summary>Answer</summary>
innerHTML replaces the ENTIRE content of an element (destroying and
rebuilding everything inside it). insertAdjacentHTML lets you insert HTML
at a specific position (beforebegin, afterbegin, beforeend, afterend)
WITHOUT destroying the existing content — more efficient for adding to
existing content.
</details>

---

## Section 4: addEventListener / removeEventListener

**Q7. What does the `{ passive: true }` option do in addEventListener, and why does it matter for performance?**
<details><summary>Answer</summary>
It tells the browser the listener will NEVER call preventDefault(),
allowing the browser to start scrolling/handling the event immediately
without waiting to see if your handler blocks it — commonly used for
scroll/touch listeners to improve responsiveness.
</details>

---

**Q8. Can you attach the same function as a listener twice on the same element/event?**
<details><summary>Answer</summary>
No — if you addEventListener with the exact SAME function reference and
same options twice, the browser recognizes it's a duplicate and only
registers it once. This only applies to identical function references,
not different arrow functions that happen to do the same thing.
</details>

---

## Section 5: Debounce vs Throttle

**Q9. Can debounce ever fire on the FIRST event instead of waiting?**
<details><summary>Answer</summary>
Yes — this is called "leading edge" debounce, an optional variant. Normal
debounce waits until events STOP (trailing edge). A leading-edge version
fires immediately on the first call, then ignores subsequent calls until
the pause period passes — useful for things like preventing double-clicks
on a submit button.
</details>

---

**Q10. In a throttle implementation, what happens to events that occur DURING the "cooldown" period?**
<details><summary>Answer</summary>
They're simply ignored/dropped — throttle doesn't queue them up to run
later, it just skips them entirely until the next allowed execution
window opens.
</details>

---

## Section 6: LocalStorage vs SessionStorage vs Cookies

**Q11. Is data in localStorage encrypted or secure by default?**
<details><summary>Answer</summary>
No — localStorage data is stored in PLAIN TEXT and is accessible to any
JavaScript running on that page (including malicious injected scripts via
XSS). Never store sensitive data like passwords or raw auth tokens there
without additional protection.
</details>

---

**Q12. What does the `HttpOnly` flag on a cookie do, and why does it matter for security?**
<details><summary>Answer</summary>
It makes the cookie inaccessible to JavaScript entirely (document.cookie
won't show it) — only the browser can send it automatically with
requests. This protects against XSS attacks stealing sensitive cookies
like session tokens, since injected malicious JS simply can't read them.
</details>

---

## Section 7: CORS

**Q13. What's a "preflight request" in CORS, and when does it happen?**
<details><summary>Answer</summary>
For certain requests (like POST with custom headers, or methods like
PUT/DELETE), the browser automatically sends an OPTIONS request FIRST,
asking the server "are you okay with this actual request?" before sending
the real one. This is called a preflight request — it happens
automatically, you don't write it yourself.
</details>

---

**Q14. Does CORS protect the SERVER, or the USER's browser?**
<details><summary>Answer</summary>
The user's browser — CORS is enforced client-side by the browser itself.
The server's data isn't inherently "protected" by CORS from someone using
tools like Postman or curl (which ignore CORS); it specifically stops
OTHER WEBSITES from making unauthorized requests using the victim's
browser and cookies.
</details>

---

## Section 8: Fetch API vs XMLHttpRequest

**Q15. Can you cancel a fetch request once it's started?**
<details><summary>Answer</summary>
Yes — using an AbortController, which XMLHttpRequest also supports via
its own .abort() method, but fetch requires this separate controller
object since fetch itself doesn't return a cancellable object directly.
</details>

```js
const controller = new AbortController();
fetch(url, { signal: controller.signal });
controller.abort(); // cancels the request
```

---

**Q16. Does `fetch()` automatically parse JSON responses?**
<details><summary>Answer</summary>
No — fetch() resolves with a Response object, and you must explicitly
call .json() (or .text(), .blob(), etc.) as a SEPARATE step to actually
extract and parse the body content. This itself returns another Promise.
</details>

---

## Bonus: Trace-the-Output / Scenario Questions

**Q17.** A user clicks a button inside a `<div>` that has both a capturing
and bubbling listener attached, plus the button itself has a listener.
What's the firing order?
<details><summary>Answer</summary>
1. Div's CAPTURING listener (event travels DOWN first)
2. Button's listener (target is reached)
3. Div's BUBBLING listener (event travels back UP)
Capturing always fires before the target's own listener, and bubbling
listeners fire after.
</details>

---

**Q18.** Why might a `debounce`-wrapped search function feel "laggy" to users if the delay is set too high (e.g., 2000ms)?
<details><summary>Answer</summary>
Because debounce waits for the FULL delay after the LAST keystroke before
running — a 2000ms delay means results only appear 2 full seconds after
the user stops typing, which feels unresponsive. Search-as-you-type UIs
typically use much shorter delays (200-500ms) to balance responsiveness
with reducing excessive requests.
</details>

---

**Q19.** You store a JWT auth token in localStorage instead of an HttpOnly
cookie. What's the security trade-off?
<details><summary>Answer</summary>
localStorage is vulnerable to XSS attacks — if any malicious script gets
injected into your page (via a vulnerable dependency, unescaped user
input, etc.), it can directly read localStorage and steal the token. An
HttpOnly cookie can't be read by JavaScript at all, making it safer
against this specific attack vector — though cookies bring their own
trade-offs (CSRF risk, sent with every request).
</details>
