# Extra Topics & Quiz — Browsers & JS Intro

---

## Extra Topics

### 1. What is a JavaScript Engine?
A program (built into browsers) that reads and executes JavaScript code.
Each browser has its own engine (Chrome/Edge use V8, Firefox uses
SpiderMonkey, Safari uses JavaScriptCore).

### 2. Client-side vs Server-side JavaScript
- **Client-side** — runs in the browser, handles UI/interactivity
- **Server-side** — runs via Node.js, handles backend logic, databases, APIs

### 3. What is Node.js?
A runtime environment that lets JavaScript run **outside the browser** — on
a server, on your computer, anywhere. It uses the same V8 engine as Chrome.

### 4. Compiled vs Interpreted
JavaScript is technically **JIT (Just-In-Time) compiled** — modern engines
compile code to machine code right before execution for speed, rather than
purely interpreting line-by-line like older engines used to.

### 5. What is the DOM?
The Document Object Model — a tree-like structure representing your HTML
page in memory, which JavaScript can read and modify to make pages
interactive.

---

## Quiz

**Q1.** What are the 3 core technologies of the web, and what does each do?
<details><summary>Answer</summary>
HTML (structure), CSS (styling), JavaScript (behavior/interactivity)
</details>

---

**Q2.** Name the JS engine used by Google Chrome.
<details><summary>Answer</summary>
V8
</details>

---

**Q3.** Can JavaScript run without a browser? How?
<details><summary>Answer</summary>
Yes — using Node.js, which runs JS on a server/machine directly using the V8 engine
</details>

---

**Q4.** True or False: JavaScript and Java are related languages.
<details><summary>Answer</summary>
False — they share a similar name for marketing reasons back in 1995, but are completely unrelated languages
</details>

---

**Q5.** What does the DOM allow JavaScript to do?
<details><summary>Answer</summary>
Read and modify the structure, content, and styling of a webpage dynamically
</details>
