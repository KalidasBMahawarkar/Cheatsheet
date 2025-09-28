Here’s your **Concept Mastery Cheat Sheet** for **Error Handling & Debugging in JavaScript** — dense, structured, and interview-ready.

---

# 🐞 Error Handling & Debugging — Cheat Card

**Concept:**
Error handling = anticipating & managing runtime issues.
Debugging = diagnosing why code misbehaves.

* JS provides **sync (`try/catch`)**, **async (`.catch`, `try/await`)**, and **tools (DevTools, Node flags)**.

---

### Error Types

* **SyntaxError** → invalid code structure.
* **ReferenceError** → variable not defined.
* **TypeError** → invalid operation on type.
* **RangeError** → invalid numeric range.
* **EvalError** → legacy eval issues.
* **AggregateError** → multiple errors (e.g., `Promise.any`).
* **CustomError** → via `class MyError extends Error {}`.

---

### Error Handling Patterns

**Sync (try/catch)**

```js
try {
  risky();
} catch (e) {
  console.error("Caught:", e.message);
} finally {
  cleanup();
}
```

**Async with Promises**

```js
doWork()
  .then(res => console.log(res))
  .catch(err => console.error("Fail:", err))
  .finally(() => console.log("done"));
```

**Async/Await**

```js
try {
  const data = await fetchData();
} catch (err) {
  console.error("Error:", err);
}
```

**Global Handlers**

```js
window.onerror = (msg, src, line, col, err) => { ... };
window.onunhandledrejection = e => { ... };

process.on("uncaughtException", console.error);
process.on("unhandledRejection", console.error);
```

---

### Debugging Tools

* **Browser DevTools** → breakpoints, watch vars, call stacks.
* **Node.js** →

  * `node --inspect` → debug with Chrome DevTools.
  * `node --trace-warnings` → log warnings.
  * `console.log`, `console.table`, `console.dir`.
* **Linters** (ESLint) → catch common issues.
* **Stack traces** → analyze error origin.

---

### Visualization

```
Code → Error thrown → try/catch OR Promise.catch → handled
If unhandled → global handler → crash/warning
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Silent failures**

   * Missing `.catch()` → unhandled rejections (Node ≥ v15 crashes).

2. **finally doesn’t change result**

   * Cleanup only; result/error flows through.

3. **Async errors**

   * `throw` inside async fn → rejected Promise.
   * Must `try/catch` or `.catch()`.

4. **Global handlers**

   * Last resort; don’t rely on them for app logic.

5. **Stack traces**

   * Minified code obscures → need source maps.

6. **Error objects**

   * Always throw `Error`, not strings → keeps stack trace.

7. **Debugging async**

   * Stack traces may skip async boundaries.

---

### ✅ Pros of Strong Error Handling

* Prevents crashes.
* Provides better user experience.
* Easier debugging and recovery.

---

### ❌ Cons

* Too much catching may hide real bugs.
* Over-reliance on global handlers = sloppy design.
* Debugging async flows can still be hard.

---

### Best Practices

* ✅ Always end Promise chains with `.catch()`.
* ✅ Wrap awaited code in `try/catch`.
* ✅ Throw `Error` objects, not raw strings.
* ✅ Use `finally` for cleanup.
* ✅ Use logging/monitoring tools (Sentry, Winston).
* ✅ Keep errors descriptive (add context).
* ✅ Debug systematically with breakpoints, not just `console.log`.

---

👉 Mnemonic:
**“Catch, Log, Clean, Don’t Hide.”**

---

⚡ Would you like me to also prepare a **Cheat Card on Custom Error Classes & Advanced Debugging (stack traces, Node flags, source maps)** so you’re covered for deeper interview rounds?


Here’s your **Concept Mastery Cheat Sheet** for **`try…catch…finally` in JavaScript** — detailed, crisp, and interview-ready.

---

# 🛡️ `try…catch…finally` — Cheat Card

**Concept:**
`try…catch…finally` = JavaScript’s **synchronous error handling block**.

* `try` → run risky code.
* `catch` → handle errors.
* `finally` → cleanup (always runs).

---

### Syntax

```js
try {
  // risky code
} catch (err) {
  // handle error
} finally {
  // always runs
}
```

---

### Examples

**Basic Handling**

```js
try {
  JSON.parse("{ bad json }");
} catch (e) {
  console.error("Parsing failed:", e.message);
}
```

**With Finally**

```js
try {
  console.log("Start");
  throw new Error("Oops");
} catch (e) {
  console.log("Error:", e.message);
} finally {
  console.log("Cleanup"); // always runs
}
```

**Return with Finally**

```js
function test() {
  try { return "A"; }
  catch { return "B"; }
  finally { return "C"; }
}
console.log(test()); // "C"
```

---

### Flow

1. Run code in `try`.
2. If no error → skip `catch`, run `finally`.
3. If error → jump to `catch`, then `finally`.
4. `finally` always executes (even after `return`, `throw`, `break`).

---

### Visualization

```
try → success ──────────► finally → exit
   └─ error thrown ──► catch ──► finally → exit
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Only catches runtime errors**

   * Syntax errors before execution crash program.

2. **Async trap**

   ```js
   try {
     setTimeout(() => { throw "error"; }, 0);
   } catch(e) {
     // ❌ won't catch (async executes later)
   }
   ```

   * Must handle with Promises or async `try/catch`.

3. **Return overridden by finally**

   * If `finally` has return, it overrides try/catch return values.

4. **Empty catch parameter (ES2019)**

   ```js
   try { risky(); } catch { console.log("fail"); }
   ```

5. **Performance**

   * Excessive try/catch may hinder optimization.

6. **Nested blocks**

   * Readability drops quickly → refactor instead.

---

### ✅ Pros

* Clean synchronous error handling.
* Ensures cleanup logic always runs.
* Keeps error handling local.

---

### ❌ Cons

* Doesn’t catch async errors directly.
* Finally can override returns unexpectedly.
* Overuse makes code messy.

---

### Best Practices

* ✅ Use `finally` for cleanup (closing files, stopping loaders).
* ✅ Never return from `finally` (surprises flow).
* ✅ Use `try/catch` sparingly — not for normal flow.
* ✅ For async, wrap in `try { await … } catch { … }`.
* ✅ Always throw `Error` objects, not raw values.

---

👉 Mnemonic:
**“Try it, Catch errors, Finally clean up.”**

---

⚡ Do you want me to follow this up with a **Cheat Card on Async Error Handling with try/catch (Promises + async/await)** to contrast how sync vs async differs?

Here’s your **Concept Mastery Cheat Sheet** for **Error Objects in JavaScript** — crisp, complete, and interview-ready.

---

# ⚠️ Error Objects — Cheat Card

**Concept:**
In JavaScript, runtime errors are represented as **Error objects**.

* Provide a **message**, **name**, and **stack trace**.
* Can be built-in (`TypeError`, `ReferenceError`, …) or custom.

---

### Creating Errors

```js
throw new Error("Something went wrong");
throw new TypeError("Invalid type");
throw new RangeError("Out of range");
```

---

### Built-in Error Types

* **Error** → base class.
* **EvalError** → legacy `eval()`.
* **RangeError** → invalid numeric range.
* **ReferenceError** → invalid reference.
* **SyntaxError** → invalid code structure.
* **TypeError** → wrong type/operation.
* **URIError** → invalid URI encoding/decoding.
* **AggregateError** (ES2021) → multiple errors (e.g., `Promise.any`).

---

### Properties

```js
try {
  throw new Error("Oops");
} catch (e) {
  console.log(e.name);    // "Error"
  console.log(e.message); // "Oops"
  console.log(e.stack);   // stack trace
}
```

* **name** → error type (e.g., `"TypeError"`).
* **message** → error description.
* **stack** → call stack (non-standard, widely supported).

---

### Custom Errors

```js
class ValidationError extends Error {
  constructor(msg) {
    super(msg);
    this.name = "ValidationError";
  }
}
throw new ValidationError("Invalid input");
```

* Extend `Error` for domain-specific errors.
* Always set `name`.

---

### Visualization

```
Error → base
   ├─ TypeError
   ├─ ReferenceError
   ├─ RangeError
   ├─ SyntaxError
   ├─ URIError
   └─ AggregateError
```

🧠 **Mental Model:** All errors inherit from `Error`.

---

### Gotchas ⚠️ (Exhaustive)

1. **Throw only Error objects**

   * `throw "string"` works but loses stack trace.

2. **Stack trace differences**

   * Format varies across engines (V8, SpiderMonkey).

3. **Custom error inheritance**

   * Must call `super(msg)` to set `message`.

4. **AggregateError quirks**

   ```js
   try {
     throw new AggregateError([new Error("a"), new Error("b")], "Multiple fails");
   } catch(e) {
     console.log(e.errors); // [Error: a, Error: b]
   }
   ```

5. **Error boundaries in async**

   * Errors in Promises become rejections, not thrown sync errors.

6. **Silent errors**

   * Ignored if no `try/catch` or `.catch()` present.

---

### ✅ Pros

* Standardized error handling across JS.
* Stack trace aids debugging.
* Extendable for custom domains.

---

### ❌ Cons

* Stack traces engine-dependent.
* Async errors need special handling (`catch` or try/await).
* Overuse of generic `Error` reduces clarity.

---

### Best Practices

* ✅ Always throw `Error` objects (not strings/numbers).
* ✅ Create **custom error classes** for clarity.
* ✅ Attach context to error messages (e.g., function name).
* ✅ Don’t swallow errors silently — always log or rethrow.
* ✅ Handle async errors with `.catch()` or try/await.

---

👉 Mnemonic:
**“Error = name + message + stack.”**

---

⚡ Do you want me to next prepare a **Cheat Card on Custom Error Classes** (patterns, hierarchy, best practices), since interviewers often ask you to design domain-specific errors?

Here’s your **Concept Mastery Cheat Sheet** for the **core JavaScript Error Types: `Error`, `SyntaxError`, `TypeError`, `ReferenceError`** — crisp, structured, and interview-ready.

---

# ⚡ JavaScript Core Error Types — Cheat Card

**Concept:**
Errors are objects representing runtime or parse-time issues.
They inherit from the base **`Error`** class and provide **name, message, stack**.

---

### 1. `Error` (Base Class)

* Generic error object.
* Extend for custom errors.

```js
throw new Error("Something failed");
```

---

### 2. `SyntaxError`

* Invalid JavaScript syntax.
* Detected at **parse/compile time** (before execution).

```js
try {
  eval("foo bar"); // invalid syntax
} catch (e) {
  console.log(e instanceof SyntaxError); // true
}
```

---

### 3. `TypeError`

* Invalid operation on a value of wrong type.

```js
null.f();           // ❌ Cannot read property
(123).toUpperCase(); // ❌ not a function
```

---

### 4. `ReferenceError`

* Accessing a variable that doesn’t exist.

```js
console.log(x); // ❌ ReferenceError: x is not defined
```

---

### Visualization

```
Error (base)
 ├─ SyntaxError     (invalid code structure)
 ├─ TypeError       (wrong type/operation)
 └─ ReferenceError  (undeclared variable/ref)
```

🧠 **Mental Model:**

* **SyntaxError** = parser stops.
* **TypeError** = wrong usage.
* **ReferenceError** = missing binding.

---

### Gotchas ⚠️ (Exhaustive)

1. **SyntaxError stops execution**

   * If in top-level code, script won’t run at all.

2. **ReferenceError vs TypeError**

   * `x` undeclared → ReferenceError.
   * `x` exists but invalid op (e.g., `undefined.toString()`) → TypeError.

3. **Error inheritance**

   * All errors inherit from `Error` → share `message` & `stack`.

4. **Catching parse-time errors**

   * Only possible when using `eval()` or `new Function()`.

5. **Strict mode differences**

   * Some silent errors in sloppy mode throw Reference/TypeErrors in strict mode.

---

### ✅ Pros

* Clear categories for debugging.
* Consistent across engines.
* Extendable for custom cases.

---

### ❌ Cons

* Stack traces differ across runtimes.
* SyntaxErrors uncatchable at load time.

---

### Best Practices

* ✅ Always throw `Error` objects, not raw strings.
* ✅ Differentiate error types for precise handling.
* ✅ Use `instanceof` for type checking.
* ✅ Wrap risky dynamic code (`eval`, `JSON.parse`) in `try/catch`.

---

👉 Mnemonic:
**“Syntax = invalid code, Type = wrong type, Reference = missing var.”**

---

⚡ Next step: Want me to prepare a **Cheat Card covering ALL built-in Error Types** (`RangeError`, `URIError`, `EvalError`, `AggregateError`) so you have the full set handy for interviews?


Here’s your **Concept Mastery Cheat Sheet** for **Throwing Custom Errors in JavaScript** — sharp, practical, and interview-ready.

---

# 🎭 Throwing Custom Errors — Cheat Card

**Concept:**
Custom errors = user-defined classes extending the built-in **`Error`**.

* Provide **semantic meaning** (e.g., `ValidationError`, `AuthError`).
* Improve debugging & error handling.

---

### Basic Custom Error

```js
class ValidationError extends Error {
  constructor(message) {
    super(message);          // sets message + stack
    this.name = "ValidationError"; // custom name
  }
}

throw new ValidationError("Invalid email");
```

---

### With Extra Context

```js
class DatabaseError extends Error {
  constructor(message, code) {
    super(message);
    this.name = "DatabaseError";
    this.code = code; // attach custom property
  }
}

try {
  throw new DatabaseError("Connection failed", "ECONN");
} catch (e) {
  console.error(e.name, e.message, e.code);
}
```

---

### Multiple Custom Errors

```js
class AuthError extends Error {}
class PermissionError extends Error {}
class NotFoundError extends Error {}

throw new PermissionError("Access denied");
```

---

### Visualization

```
Error (base)
 ├─ ValidationError
 ├─ DatabaseError
 ├─ AuthError
 └─ ...
```

🧠 **Mental Model:** Custom errors = **domain-specific labels** on standard Error objects.

---

### Gotchas ⚠️ (Exhaustive)

1. **Always call `super(message)`**

   * Without it, `message` and `stack` aren’t set.

2. **Set `this.name`**

   * Otherwise defaults to `"Error"`.

3. **Inheritance issues in older JS**

   * Before ES6, extending `Error` was tricky (needed `Object.setPrototypeOf`).

4. **Custom properties**

   * Attach extra fields (code, status, details) carefully → don’t bloat.

5. **Error serialization**

   * `JSON.stringify(new Error("msg"))` → `{}` (properties not enumerable).
   * Use custom `.toJSON()` if needed.

6. **Error instanceof**

   * Works if class properly extends Error.

7. **Stack traces**

   * May vary across environments (Node vs browser).

---

### ✅ Pros

* Expressive, domain-specific errors.
* Easier to differentiate in `catch` blocks.
* Can attach metadata (codes, context).

---

### ❌ Cons

* Too many custom errors = complexity.
* Stack trace handling not always uniform.
* Need consistent hierarchy to avoid confusion.

---

### Best Practices

* ✅ Use **custom errors for domain-specific cases** (e.g., Auth, DB, Validation).
* ✅ Keep error hierarchy simple & meaningful.
* ✅ Always log `name + message + stack` for debugging.
* ✅ Add contextual fields sparingly (status, code).
* ✅ Re-throw when appropriate → don’t swallow errors.

---

👉 Mnemonic:
**“Extend, Name, Super, Throw.”**

1. Extend `Error`
2. Set `name`
3. Call `super(message)`
4. Throw it

---

⚡ Do you want me to prepare a **Cheat Card on Error Hierarchies** (structuring multiple custom error classes for real-world apps like `AppError → AuthError → TokenExpiredError`)?

Here’s your **Concept Mastery Cheat Sheet** for **Error Stack Traces in JavaScript** — polished, visual, and interview-ready.

---

# 🧵 Error Stack Traces — Cheat Card

**Concept:**
A **stack trace** shows the **chain of function calls** that led to an error.

* Comes from the **`stack`** property of `Error` objects.
* Helps debug where and why the error occurred.

---

### Example

```js
function a() { b(); }
function b() { c(); }
function c() { throw new Error("Boom!"); }

try { a(); }
catch (e) { console.log(e.stack); }
```

**Output (V8 / Node.js style):**

```
Error: Boom!
    at c (<file>:3:9)
    at b (<file>:2:3)
    at a (<file>:1:3)
    at <anonymous>:1:1
```

---

### Structure of a Stack Trace

1. **Error name + message**

   * `TypeError: undefined is not a function`
2. **Call frames** (most recent first)

   * `at functionName (file:line:col)`
3. **Execution path**

   * Shows nested calls leading to error.

---

### Visualization

```
Error
  ↳ c()   → failed
  ↳ b()   → called c
  ↳ a()   → called b
  ↳ main  → called a
```

🧠 **Mental Model:** Stack trace = a **breadcrumb trail** back to the bug.

---

### Gotchas ⚠️ (Exhaustive)

1. **Engine differences**

   * V8 (Node/Chrome), SpiderMonkey (Firefox), JavaScriptCore (Safari) → slightly different formats.

2. **Async gaps**

   * Async/await, Promises, and callbacks may break stack continuity.

   ```js
   async function f() { throw new Error("fail"); }
   f().catch(console.error); // stack doesn’t show original caller
   ```

3. **Minified code**

   * Hard to read stack traces in production → need **source maps**.

4. **Custom error classes**

   * Must call `super(msg)` to preserve stack trace.

5. **Stack depth**

   * Some environments truncate very deep stacks.

6. **Performance**

   * Capturing stack traces can be expensive → avoid in hot paths.

7. **Node.js async hooks**

   * Need extra tooling (`--enable-source-maps`, async hooks) to debug async stack traces properly.

---

### ✅ Pros

* Shows **exact failure path**.
* Essential for debugging.
* Supported across all modern JS engines.

---

### ❌ Cons

* Inconsistent formats across runtimes.
* Breaks with async flows unless handled.
* Requires source maps for production debugging.

---

### Best Practices

* ✅ Always log **name + message + stack** for errors.
* ✅ Use **Error.captureStackTrace** in Node.js for custom error handling.
* ✅ Deploy with **source maps** to map minified code back to original.
* ✅ Use monitoring tools (Sentry, LogRocket, etc.) to capture stack traces in prod.
* ✅ Be cautious with stack traces in sensitive environments → may leak file paths.

---

👉 Mnemonic:
**“Stack = Steps to the bug.”**

---

⚡ Would you like me to prepare a **Cheat Card on Async Stack Traces** (how Promises/async/await break them and how to fix with source maps & Node flags)? That’s a very common interview twist after basic error handling.


Here’s your **Concept Mastery Cheat Sheet** for **Debugging Tools in JavaScript (Browser + Node.js)** — concise, complete, and interview-ready.

---

# 🔧 JavaScript Debugging Tools — Cheat Card

**Concept:**
Debugging = inspecting and fixing program behavior.
JS environments (browser, Node.js) provide **tools, flags, and techniques**.

---

### 1. Console API

```js
console.log("value:", x);
console.error("error");
console.warn("warn");
console.table([{id:1},{id:2}]);
console.dir(obj, { depth: null });
```

* Quick logging.
* `console.assert(cond, msg)` → logs only if false.
* `console.time("id")` / `console.timeEnd("id")` → measure performance.

---

### 2. Browser DevTools (Chrome/Firefox)

* **Breakpoints** (line, conditional, DOM, XHR).
* **Watch expressions** → inspect variable changes.
* **Call stack & scope** → step into/out of functions.
* **Network tab** → API calls, status, payloads.
* **Performance tab** → CPU/memory profiling.
* **Lighthouse** → audits (perf, accessibility).

---

### 3. Node.js Debugging

* **Inspector protocol**

  ```bash
  node --inspect app.js
  node --inspect-brk app.js # break on first line
  ```

  → Connect via Chrome DevTools.

* **REPL** → `node` interactive shell.

* **Node flags**:

  * `--trace-warnings` → show warnings stack.
  * `--trace-deprecation` → deprecation logs.
  * `--enable-source-maps` → better async stack traces.

* **Debugger keyword**

  ```js
  function test() {
    debugger; // pause if debugger attached
  }
  ```

---

### 4. Advanced Debugging

* **Linters** (ESLint) → catch errors before runtime.
* **TypeScript** → type errors at compile time.
* **Source Maps** → map minified → original code.
* **Profilers** → measure memory, CPU, event loop lag.
* **Error Monitoring Tools** → Sentry, LogRocket, Datadog.

---

### Visualization

```
Code → Error → Debugging Tools
   │
   ├─ Console logs
   ├─ DevTools breakpoints
   ├─ Node inspector
   └─ Profilers & monitors
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Overusing console.log** → noisy, hides real issues.
2. **Minified code debugging** → unreadable without source maps.
3. **Async stack traces** → may lose context unless using source maps / `--enable-source-maps`.
4. **Debugger keyword ignored** if no debugger attached.
5. **Performance profiling overhead** → don’t leave in production.
6. **Global error handlers** helpful but last-resort (can swallow errors).

---

### ✅ Pros

* Multiple levels (quick logs → full profiling).
* Built into browsers & Node.
* Rich ecosystem (monitoring SaaS).

---

### ❌ Cons

* Async/await makes debugging harder (broken stack traces).
* Too many logs hurt readability.
* Node inspector can feel heavy for small apps.

---

### Best Practices

* ✅ Start with breakpoints, not blind `console.log`.
* ✅ Use linters & TypeScript to catch bugs early.
* ✅ Deploy with source maps for production debugging.
* ✅ Log errors with context (not just stack).
* ✅ Automate error reporting with monitoring tools.

---

👉 Mnemonic:
**“Log, Break, Inspect, Monitor.”**

---

⚡ Do you want me to also prepare a **Cheat Card on Debugging Async Code (Promises, async/await, Event Loop tracing)** — since that’s a common pain point in interviews?

Here’s your **Concept Mastery Cheat Sheet** for the **`console` API in JavaScript** — dense, visual, and fully interview-ready.

---

# 🖥️ JavaScript `console` API — Cheat Card

**Concept:**
`console` provides debugging, logging, and inspection tools in browsers & Node.js.
Not just `log` — a full set of methods for diagnostics.

---

### Core Logging

```js
console.log("Hello", 42);   // general log
console.info("Info msg");   // info-level
console.warn("Warning!");   // warning
console.error("Error!");    // error
```

---

### Debugging Helpers

```js
console.assert(x > 0, "x must be positive"); // logs if false
console.trace("Here"); // stack trace
console.clear();       // clear console
```

---

### Object Inspection

```js
console.dir(obj);               // inspect object
console.dirxml(node);           // DOM tree (browser)
console.table([{id:1},{id:2}]); // tabular display
```

---

### String Formatting

```js
console.log("Hi %s, you are %d years old", "Kalidas", 25);
// %s = string, %d = number, %o = object, %c = CSS style (browser)
console.log("%cStyled", "color:red; font-size:20px");
```

---

### Grouping

```js
console.group("Start");
console.log("Inside group");
console.groupEnd();
```

* `console.groupCollapsed()` → starts collapsed group.

---

### Timing & Counting

```js
console.time("db");
doQuery();
console.timeEnd("db"); // logs elapsed time

console.count("loop"); // increments label count
console.countReset("loop");
```

---

### Visualization

```
console
 ├─ log/info/warn/error
 ├─ assert / trace / clear
 ├─ dir / table / dirxml
 ├─ group / groupCollapsed / groupEnd
 ├─ time / timeEnd / timeLog
 └─ count / countReset
```

🧠 **Mental Model:** `console` = multi-tool for logging, timing, tracing, and formatting.

---

### Gotchas ⚠️ (Exhaustive)

1. **Environment differences**

   * Browser vs Node → some methods missing (`dirxml` browser-only, `console.debug` in Node = alias for log).

2. **Performance overhead**

   * Excessive logging slows down apps.
   * Use sparingly in production.

3. **`console.clear()`**

   * Behavior differs: clears screen (browser), just scrolls (Node).

4. **Styling logs**

   * `%c` works in browsers, not Node.

5. **Stack traces**

   * `console.trace()` prints current stack, may differ across engines.

6. **Disabled logs**

   * Some environments strip logs in production builds.

---

### ✅ Pros

* Universally available.
* Rich API beyond simple logging.
* Useful for quick debugging & profiling.

---

### ❌ Cons

* Not structured logging (use Winston/Pino for production).
* Different browsers/Node implement slightly differently.
* Too many logs clutter output.

---

### Best Practices

* ✅ Use levels: `log` for debug, `warn` for potential issues, `error` for failures.
* ✅ Use `console.table` for arrays/objects.
* ✅ Clear or disable logs in production.
* ✅ Use `console.time` for quick performance checks.
* ✅ Group related logs with `console.group`.

---

👉 Mnemonic:
**“Log, Trace, Group, Time, Count.”**

---

⚡ Next step: Do you want me to prepare a **Cheat Card comparing Browser vs Node.js `console` differences** (methods, formatting, quirks) for sharper interview answers?

Here’s your **Concept Mastery Cheat Sheet** for **Breakpoints (DevTools & VSCode)** — crisp, visual, and interview-ready.

---

# 🎯 Breakpoints in JavaScript Debugging — Cheat Card

**Concept:**
Breakpoints let you **pause code execution** at chosen lines/conditions to inspect state.

* Supported in **Browser DevTools** & **VSCode Debugger**.
* Essential for step-by-step debugging.

---

### Types of Breakpoints

1. **Line Breakpoint** → pause on a specific line.
2. **Conditional Breakpoint** → pause only if expression is true.

   ```js
   // pause only if x > 10
   ```
3. **Logpoint (VSCode)** → doesn’t pause, logs expression instead.
4. **DOM Breakpoint** (DevTools) → pause when DOM node changes.
5. **XHR/Fetch Breakpoint** → pause on specific network requests.
6. **Event Listener Breakpoint** → pause when specific event triggers.

---

### Setting Breakpoints

**Browser DevTools (Chrome/Firefox):**

* Open **Sources** tab → click line number.
* Right-click for **Conditional Breakpoint**.
* Pause on exceptions → toggle in debugger settings.

**VSCode:**

* Click in gutter beside line numbers.
* Right-click → add **Condition**, **Hit Count**, or **Logpoint**.
* Use `.vscode/launch.json` to configure debug session.

---

### Debugging Controls

* ▶️ **Continue** → resume until next breakpoint.
* ⏭️ **Step Over** → run current line, skip into functions.
* ⏬ **Step Into** → enter called function.
* ⏹️ **Step Out** → finish current function, return caller.
* 👁️ **Watch** → track variable expressions.
* 🧾 **Call Stack** → see execution chain.

---

### Example

```js
function calc(x) {
  let y = x * 2;   // set breakpoint here
  return y + 1;
}
calc(5);
```

* Pause at breakpoint → inspect `x`, `y`, call stack.

---

### Visualization

```
Code → Breakpoint hit → Pause execution
   │
   ├─ Inspect variables
   ├─ Step over/into/out
   └─ Continue or stop
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Minified code**

   * Hard to debug → use **source maps**.

2. **Async/await & promises**

   * Breakpoints may pause later (after async resume).

3. **Event loop timing**

   * `setTimeout`, `requestAnimationFrame` hit in next tick.

4. **Conditional breakpoints**

   * Slow if expression is complex (evaluated often).

5. **Hit count breakpoints (VSCode)**

   * Useful but can affect performance.

6. **Break on exceptions**

   * Can be noisy → filter out caught exceptions.

---

### ✅ Pros

* Inspect state at runtime.
* Step through code without editing.
* Catch hidden async bugs.

---

### ❌ Cons

* Slower than logging for quick checks.
* Can be tricky in heavily minified/bundled code.
* Overusing conditional breakpoints = perf hit.

---

### Best Practices

* ✅ Use breakpoints for **complex bugs**, not just console.log spam.
* ✅ Use conditional/logpoint breakpoints for loops.
* ✅ Combine with **Watch** + **Call Stack** for deeper insights.
* ✅ Enable **Pause on Exceptions** during debugging sessions.
* ✅ Use source maps for bundled/minified code.

---

👉 Mnemonic:
**“Break, Inspect, Step, Continue.”**

---

⚡ Would you like me to now prepare a **Cheat Card on “Step-through Debugging” (Step Into / Over / Out with async code)** — since that’s a tricky area interviewers often test?


Here’s your **Concept Mastery Cheat Sheet** for the **`debugger` keyword in JavaScript** — crisp, visual, and interview-ready.

---

# 🛑 `debugger` Keyword — Cheat Card

**Concept:**
`debugger` is a **built-in statement** that pauses JS execution at that line **if DevTools/Debugger is open**.

* Acts like a programmatic breakpoint.

---

### Syntax

```js
function calc(x) {
  let y = x * 2;
  debugger;  // execution pauses here
  return y + 1;
}
calc(5);
```

---

### Behavior

* When execution reaches `debugger;` → pauses.
* Opens DevTools (browser) or VSCode/Node inspector if attached.
* Developer can inspect variables, call stack, scope, memory.
* If no debugger attached → statement is ignored.

---

### Use Cases

1. **Programmatic breakpoint**

   * Drop into debugger without setting breakpoints manually.

2. **Conditional debugging**

   ```js
   if (x > 100) debugger;
   ```

3. **Debugging async code**

   * Place inside callbacks, promise chains, or async/await.

4. **Debugging minified code**

   * Combined with **source maps** for clarity.

---

### Visualization

```
Code runs → hits debugger → pauses execution
   │
   ├─ Inspect variables
   ├─ Step through code
   └─ Continue / Resume
```

🧠 **Mental Model:** `debugger` = inline “pause here” statement.

---

### Gotchas ⚠️ (Exhaustive)

1. **Ignored if no debugger**

   * Code won’t crash; statement skipped silently.

2. **Production code risk**

   * Leaving `debugger;` in shipped code halts user’s browser if DevTools open.

3. **Async timing**

   * If placed inside `setTimeout`, `await`, or callbacks → pause occurs only when reached.

4. **Works differently across environments**

   * Browser: opens DevTools.
   * Node.js: needs inspector (`node --inspect`).

5. **Not for logging**

   * Freezes execution until manually resumed.

---

### ✅ Pros

* Quick and precise debugging.
* No need to open DevTools first.
* Works inside async callbacks where breakpoints are tricky.

---

### ❌ Cons

* Halts execution → disruptive if left in production.
* Requires DevTools/Inspector to be open.
* Can clutter code if overused.

---

### Best Practices

* ✅ Use for **temporary debugging** during development.
* ✅ Always remove before committing/production.
* ✅ Combine with conditions (`if (debug) debugger;`).
* ✅ Prefer breakpoints for long-term debugging, `debugger` for ad-hoc.
* ✅ Keep source maps enabled for better debugging experience.

---

👉 Mnemonic:
**“debugger = inline breakpoint.”**

---

⚡ Would you like me to now create a **Cheat Card on Debugging Async Code (Promises + async/await with `debugger`)** — since that’s a tricky, high-value interview area?
