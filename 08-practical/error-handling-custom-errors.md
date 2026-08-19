## Q: Error Handling & Custom Errors

**Answer:**

**Basic try/catch/finally:**

```js
try {
  JSON.parse("invalid json");
} catch (error) {
  console.log("Error caught:", error.message);
} finally {
  console.log("This always runs, error or not");
}
```

**Throwing your own errors:**

```js
function divide(a, b) {
  if (b === 0) {
    throw new Error("Cannot divide by zero");
  }
  return a / b;
}

try {
  divide(10, 0);
} catch (error) {
  console.log(error.message); // "Cannot divide by zero"
}
```

**Custom Error classes — extending the built-in Error class:**

```js
class ValidationError extends Error {
  constructor(message) {
    super(message);       // calls Error's constructor
    this.name = "ValidationError";  // custom error name
  }
}

function validateAge(age) {
  if (age < 0) {
    throw new ValidationError("Age cannot be negative");
  }
  return age;
}

try {
  validateAge(-5);
} catch (error) {
  if (error instanceof ValidationError) {
    console.log("Validation issue:", error.message);
  } else {
    console.log("Unknown error:", error.message);
  }
}
```

**Why custom errors matter — being able to distinguish error TYPES:**

```js
try {
  // ... some code that might throw different kinds of errors
} catch (error) {
  if (error instanceof ValidationError) {
    // handle validation errors specifically
  } else if (error instanceof TypeError) {
    // handle type errors differently
  } else {
    // generic fallback
  }
}
```

**Follow-up questions interviewers ask:**

- Why create custom error classes instead of just throwing strings? (Allows using `instanceof` to distinguish error types, attach extra properties, and handle different failures differently)
- What does `finally` guarantee? (Runs regardless of whether the try block succeeded or the catch block ran — useful for cleanup like closing connections or hiding loading spinners)
