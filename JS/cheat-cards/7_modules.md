Here’s your **Concept Mastery Cheat Sheet** for **JavaScript Modules (ES Modules vs CommonJS)** — structured, detailed, and interview-ready.
---
# 📦 JavaScript Modules — Cheat Card
**Concept:**
Modules allow JS code to be split into **reusable, maintainable files**.
* **ES Modules (ESM)** → standard in modern JS (`import`/`export`).
* **CommonJS (CJS)** → Node.js legacy (`require`/`module.exports`).
---
### ES Modules (ESM)
**Exporting**
```js
// file: math.js
export const add = (a,b) => a+b;
export function mul(a,b) { return a*b; }
export default function sub(a,b) { return a-b; }
```
**Importing**
```js
import sub, { add, mul } from "./math.js";
console.log(add(2,3), mul(2,3), sub(5,2));
```
* **Default export** → `import x from "..."`.
* **Named export** → `import {x, y} from "..."`.
* **Alias** → `import {x as y} from "..."`.
* **Namespace** → `import * as math from "./math.js"`.
---
### CommonJS (CJS)
**Exporting**
```js
// math.js
module.exports = { add, mul };
```
**Importing**
```js
const { add, mul } = require("./math");
```
* Always synchronous.
* No tree-shaking.
---
### Key Differences: ESM vs CJS
| Feature         | ESM (`import/export`)          | CJS (`require/module.exports`) |
| --------------- | ------------------------------ | ------------------------------ |
| Standard        | ES2015+ (browser & Node ≥ v12) | Node.js legacy                 |
| Syntax          | `import`, `export`             | `require`, `module.exports`    |
| Loading         | Static (compile-time)          | Dynamic (runtime)              |
| Tree-shaking    | ✅ Yes                          | ❌ No                           |
| Top-level await | ✅ Supported                    | ❌ Not supported                |
| Default export  | Supported                      | Emulated with `module.exports` |
| Interop         | Needs `.default` sometimes     | Direct assignment              |
---
### Gotchas ⚠️ (Exhaustive)
1. **File extension**
   * Must use `.js` with `"type":"module"` in Node, or `.mjs`.
2. **Default vs named imports**
   * Wrong import style = runtime error.
3. **Top-level await**
   * Only works in ESM, not CJS.
4. **Circular dependencies**
   * Partial exports may be undefined.
5. **Mixing CJS & ESM**
   * Requires interop wrappers in Node.js.
6. **Dynamic imports**
   * `import()` is async → returns Promise.
7. **Strict mode**
   * ESM always runs in strict mode.
---
### ✅ Pros of ESM
* Standardized across browsers & Node.
* Static analysis → better optimization & tree-shaking.
* Cleaner async features (`import()`, top-level await).
---
### ❌ Cons of ESM
* Stricter rules (must specify extensions).
* Harder interop with CJS code.
* Asynchronous loading in Node may surprise devs.
---
### Best Practices
* ✅ Prefer **ES Modules** in modern codebases.
* ✅ Use `import * as` for grouped utilities.
* ✅ Keep default exports minimal; prefer named exports for clarity.
* ✅ Avoid circular dependencies; restructure code if needed.
* ✅ Use dynamic `import()` for lazy loading.
---
👉 Mnemonic:
**“Import/Export = Standard; Require = Legacy.”**
---
⚡ Next natural follow-up: a **Cheat Card on Dynamic Import & Top-Level Await** (since they often come up as “modern ES Module” interview questions).
Would you like me to prepare that as the next card?
Here’s your **Concept Mastery Cheat Sheet** for **IIFE & Namespace Pattern (pre-ES6)** — tuned for interview clarity and modern context.
---
# 🌀 IIFE & Namespace Pattern (pre-ES6) — Cheat Card
**Concept:**
Before ES6 modules, JS developers used **Immediately Invoked Function Expressions (IIFE)** and **namespaces** to:
* Encapsulate variables/functions (avoid global pollution).
* Organize code into logical groups.
---
### IIFE (Immediately Invoked Function Expression)
**Syntax**
```js
(function() {
  // private scope
  const secret = 42;
  console.log("IIFE running");
})(); // executes immediately
```
**With Parameters**
```js
(function(msg) {
  console.log(msg);
})("Hello");
```
**Returning Values**
```js
const counter = (function() {
  let count = 0;
  return {
    inc: () => ++count,
    val: () => count
  };
})();
console.log(counter.inc()); // 1
```
---
### Namespace Pattern
**Object Literal Namespace**
```js
var MyApp = MyApp || {};
MyApp.utils = {
  sum: (a,b) => a+b,
  mul: (a,b) => a*b
};
console.log(MyApp.utils.sum(2,3));
```
**Revealing Module Pattern (via IIFE)**
```js
var Calculator = (function() {
  let total = 0; // private
  function add(x) { total += x; }
  function reset() { total = 0; }
  function getTotal() { return total; }
  return { add, reset, getTotal }; // expose public API
})();
Calculator.add(5);
console.log(Calculator.getTotal()); // 5
```
---
### Visualization
```
Global Scope (polluted pre-ES6)
   ↓
IIFE → creates private scope → variables hidden
Namespace → single global object → organized APIs
```
---
### Gotchas ⚠️ (Exhaustive)
1. **Still 1 global name**
   * Namespace object itself pollutes global scope.
2. **Tight coupling**
   * Multiple namespaces = manual dependency order.
3. **Hard to scale**
   * No built-in imports/exports.
4. **IIFE readability**
   * Beginners find syntax confusing (`(function(){})()`).
5. **Async loading**
   * Scripts must be loaded in correct order manually.
---
### ✅ Pros
* Avoids leaking variables into global scope.
* Simple way to encapsulate private data.
* Common pattern before ES6 modules.
* Revealing Module Pattern gave OOP-style structure.
---
### ❌ Cons
* Not a true module system.
* Namespace still pollutes global scope.
* No dependency management or tree-shaking.
* Replaced by **ES6 Modules** (`import`/`export`).
---
### Best Practices (pre-ES6)
* ✅ Use IIFE for private scope.
* ✅ Use namespaces to group utilities.
* ✅ Apply Revealing Module Pattern for clear APIs.
* ✅ Prefix namespaces to avoid collisions.
---
👉 Mnemonic:
**“IIFE hides, Namespace organizes.”**
---
⚡ Do you want me to now prepare a **Cheat Card comparing Pre-ES6 Patterns (IIFE/Namespace) vs ES6 Modules** — so you can answer interviewers who ask “How did we manage modules before ES6?”?

Here’s your **Concept Mastery Cheat Sheet** for **CommonJS (`require`, `module.exports`)** — clean, dense, and interview-ready.
---
# 📦 CommonJS (CJS) — Cheat Card
**Concept:**
**CommonJS (CJS)** is the **module system for Node.js** (pre-ESM).
* Uses **`require`** to import and **`module.exports`** (or `exports`) to export.
* Synchronous, file-based loading.
---
### Exporting
```js
// math.js
function add(a,b){ return a+b; }
function mul(a,b){ return a*b; }
module.exports = { add, mul };
// or: exports.add = add;
```
---
### Importing
```js
// app.js
const { add, mul } = require("./math");
console.log(add(2,3)); // 5
```
---
### Flow
1. Each file = its own **module scope** (not global).
2. `module.exports` defines what is exposed.
3. `require(path)` loads module, returns exports object.
4. Modules are **cached** → loaded once, reused.
---
### Features
* **Single export**
  ```js
  module.exports = function(x){ return x*2; };
  ```
* **Multiple exports**
  ```js
  exports.a = 1;
  exports.b = 2;
  ```
* **Circular deps supported** → partially resolved exports may be undefined.
---
### Visualization
```
File math.js → module.exports = {…}
   ↓
require("./math") → { add, mul }
```
---
### Gotchas ⚠️ (Exhaustive)
1. **Synchronous loading**
   * Blocks execution while loading (fine for server, not browser).
2. **exports vs module.exports**
   * `exports` is a reference; reassigning it breaks.
   ```js
   exports = { foo: 1 }; // ❌ ignored
   module.exports = { foo: 1 }; // ✅ works
   ```
3. **Caching**
   * A required module is cached; changes won’t reload unless cache cleared.
4. **Circular dependencies**
   * Partially initialized objects returned if modules depend on each other.
5. **Not tree-shakable**
   * Dead code elimination not possible, unlike ES modules.
6. **Interop with ES Modules**
   * Mixing `import`/`export` with CJS requires interop wrappers.
---
### ✅ Pros
* Simple, widely adopted in Node.js.
* Mature ecosystem (NPM packages use CJS).
* Modules execute in isolated scope.
* Supports conditional requires (dynamic).
---
### ❌ Cons
* Synchronous (bad for browsers).
* No tree-shaking.
* Circular deps tricky.
* Interop issues with modern ES Modules.
---
### Best Practices
* ✅ Use ES Modules in modern Node.js unless stuck with CJS packages.
* ✅ Be careful with `exports` vs `module.exports`.
* ✅ Use dynamic `require` only when needed.
* ✅ Avoid circular dependencies by restructuring code.
* ✅ Convert legacy CJS to ESM gradually.
---
👉 Mnemonic:
**“require loads, module.exports exposes.”**
---
⚡ Do you want me to now prepare a **Cheat Card comparing CommonJS vs ES Modules** side by side (syntax, behavior, pros/cons) for quick interview answers?
Here’s your **Concept Mastery Cheat Sheet** for **ES Modules (`import`, `export`)** — sharp, visual, and fully interview-ready.
---
# 📦 ES Modules (ESM) — Cheat Card
**Concept:**
**ES Modules (ESM)** are the official JavaScript module system (ES2015+).
* Use **`import`** and **`export`** keywords.
* Support **static analysis**, **tree-shaking**, and **async loading**.
* Native in browsers and Node.js (≥12+ with `"type":"module"`).
---
### Exporting
```js
// math.js
export const add = (a, b) => a + b;
export function mul(a, b) { return a * b; }
export default function sub(a, b) { return a - b; }
```
---
### Importing
```js
// app.js
import sub, { add, mul } from "./math.js";
console.log(add(2,3));  // 5
console.log(mul(2,3));  // 6
console.log(sub(5,2));  // 3
```
---
### Variants
* **Default export**
  ```js
  export default function() {}
  import myFn from "./file.js";
  ```
* **Named export**
  ```js
  export const foo = 1;
  import { foo } from "./file.js";
  ```
* **Renaming**
  ```js
  import { foo as bar } from "./file.js";
  ```
* **Namespace import**
  ```js
  import * as math from "./math.js";
  math.add(2,3);
  ```
* **Dynamic import (async)**
  ```js
  const mod = await import("./math.js");
  mod.add(1,2);
  ```
---
### Visualization
```
math.js → export { add, mul, default: sub }
   ↓
app.js → import sub, { add, mul } from "math.js"
```
---
### Gotchas ⚠️ (Exhaustive)
1. **File extension**
   * Node requires `.js` (with `"type":"module"`) or `.mjs`.
2. **Static imports only at top-level**
   * Except dynamic `import()`.
3. **Default vs named mismatch**
   * Wrong style → runtime error.
4. **Top-level await**
   * Allowed only in ESM, not CommonJS.
5. **Circular dependencies**
   * Modules export *live bindings* → may be `undefined` temporarily.
6. **Always strict mode**
   * No sloppy mode in ESM.
7. **Interop with CommonJS**
   * Importing CJS may need `.default` property.
---
### ✅ Pros
* Standardized, works in browsers + Node.
* Static analysis → tree-shaking, better tooling.
* Async-friendly (`import()`, top-level await).
* Clear syntax vs legacy patterns.
---
### ❌ Cons
* Strict rules (extensions, top-level import only).
* Harder interop with CommonJS.
* Asynchronous module resolution may surprise in Node.
---
### Best Practices
* ✅ Prefer **named exports** for clarity.
* ✅ Use **default export** sparingly (one per file).
* ✅ Use **dynamic import** for lazy-loading.
* ✅ Avoid circular dependencies; restructure if needed.
* ✅ Always specify file extensions in Node.js.
---
👉 Mnemonic:
**“Export shares, Import cares.”**
---
⚡ Next step: Would you like me to now prepare a **side-by-side Cheat Card: ES Modules vs CommonJS** (syntax + differences + pros/cons) so you can answer comparisons instantly in interviews?

Here’s your **Concept Mastery Cheat Sheet** for **Named vs Default Exports in ES Modules** — structured, visual, and packed with interview-ready details.
---
# 🔀 Named vs Default Exports — Cheat Card
**Concept:**
In ES Modules, you can export code either as **named exports** (multiple per file) or as a **default export** (only one per file).
---
### Named Exports
```js
// math.js
export const add = (a,b) => a+b;
export function mul(a,b){ return a*b; }
```
**Importing:**
```js
import { add, mul } from "./math.js";
import { add as sum } from "./math.js"; // alias
```
* Multiple allowed per file.
* Imported using **destructuring-like syntax**.
* Must match exact names (case-sensitive).
---
### Default Exports
```js
// math.js
export default function sub(a,b){ return a-b; }
```
**Importing:**
```js
import sub from "./math.js";
```
* Only **one default export** per file.
* Name can be **anything** when importing.
* Good for a module’s “main thing.”
---
### Mixed Exports
```js
// math.js
export default function div(a,b){ return a/b; }
export const PI = 3.14;
```
**Importing:**
```js
import div, { PI } from "./math.js";
```
---
### Visualization
```
Named Exports → { add, mul }
Default Export → sub
Mixed → sub + { PI }
```
---
### Gotchas ⚠️ (Exhaustive)
1. **Only one default per module**
   * Multiple defaults → syntax error.
2. **Name mismatch**
   * Named imports must match exactly.
   * Default can be renamed freely.
3. **Import style errors**
   ```js
   import { default } from "./file.js"; // ❌ wrong
   import myFn from "./file.js";       // ✅ correct
   ```
4. **Interop with CommonJS**
   * `require()` CJS in ESM may need `.default`.
5. **Tree-shaking**
   * Named exports are better for tree-shaking (unused code removed).
   * Default exports are harder for bundlers to optimize.
6. **Mixed usage confusion**
   * Mixing default + named can confuse new devs.
---
### ✅ Pros & ❌ Cons
**Named Exports**
* ✅ Clear, multiple exports per file.
* ✅ Tree-shaking friendly.
* ❌ Must remember exact names.
**Default Export**
* ✅ Simple when file has single purpose.
* ✅ Import name flexible.
* ❌ Only one per file.
* ❌ Less tree-shaking friendly.
---
### Best Practices
* ✅ Prefer **named exports** for utilities/libraries.
* ✅ Use **default export** for a module’s main function/class.
* ✅ Avoid mixing too many named + default in one file.
* ✅ Be consistent across project → reduces confusion.
---
👉 Mnemonic:
**“Named = many, must match. Default = one, name free.”**
---
⚡ Do you want me to now prepare a **Cheat Card comparing ES Modules vs CommonJS (require vs import/export)** side by side — so you can answer comparison questions instantly in interviews?

Here’s your **Concept Mastery Cheat Sheet** for **Dynamic `import()` in ES Modules** — compact, visual, and interview-ready.
---
# 📥 Dynamic `import()` — Cheat Card
**Concept:**
`import()` is a function-like syntax that allows **asynchronous, runtime module loading**.
* Returns a **Promise** resolving to the module’s exports.
* Useful for **code-splitting, lazy-loading, and conditional imports**.
---
### Syntax
```js
import("./math.js")
  .then(mod => {
    console.log(mod.add(2,3));
  })
  .catch(err => console.error("Failed:", err));
```
**With `await` (inside async function / top-level in ESM):**
```js
const mod = await import("./math.js");
console.log(mod.add(2,3));
```
---
### Use Cases
* **Lazy loading**
  ```js
  if (condition) {
    const utils = await import("./utils.js");
    utils.doWork();
  }
  ```
* **Code splitting (bundlers)**
  ```js
  button.onclick = async () => {
    const { showModal } = await import("./modal.js");
    showModal();
  };
  ```
* **Dynamic path imports**
  ```js
  const lang = "en";
  const messages = await import(`./i18n/${lang}.js`);
  ```
---
### Visualization
```
Static import → loaded at compile time
Dynamic import() → loaded at runtime (Promise-based)
```
---
### Gotchas ⚠️ (Exhaustive)
1. **Async only**
   * Always returns Promise. Must use `await` or `.then()`.
2. **Path restrictions**
   * Path must be string/templated literal resolvable at runtime.
   * Not all bundlers support fully dynamic variables.
3. **Interop quirks**
   * Importing CommonJS may require `.default`.
4. **Performance**
   * Dynamic imports split bundles → may cause runtime delays if overused.
5. **Top-level await**
   * Works only in ES Modules, not CommonJS.
6. **Error handling**
   * Must wrap in `try/catch` or `.catch()`.
---
### ✅ Pros
* Enables **lazy loading** for performance.
* Reduces initial bundle size.
* Useful for conditional/locale-specific code.
* Works natively in modern browsers and Node.
---
### ❌ Cons
* Asynchronous → may complicate flow.
* Overuse can fragment bundles and increase requests.
* Harder for bundlers if paths are too dynamic.
---
### Best Practices
* ✅ Use for code that’s not always needed (routes, modals).
* ✅ Wrap in `try/catch` to handle load errors.
* ✅ Keep paths semi-static for bundlers (avoid raw user input).
* ✅ Combine with top-level await for clarity.
* ✅ Don’t replace all static imports — only use when truly conditional.
---
👉 Mnemonic:
**“Static loads always; dynamic loads when needed.”**
---
⚡ Would you like me to now prepare the **Cheat Card on Top-Level Await** (since it pairs directly with dynamic `import()` in ES Modules)?

Here’s your **Concept Mastery Cheat Sheet** for **Tree Shaking in JavaScript** — precise, complete, and interview-ready.
---
# 🌲 Tree Shaking — Cheat Card
**Concept:**
**Tree shaking** = dead-code elimination for ES Modules.
* Removes **unused exports** at build time.
* Relies on **static `import`/`export` analysis**.
---
### How It Works
1. Use **ESM** (`import/export`) → static structure, no runtime surprises.
2. Bundler (Webpack, Rollup, Vite, etc.) analyzes imports.
3. Unused code (not imported) is dropped from final bundle.
---
### Example
**math.js**
```js
export const add = (a,b) => a+b;
export const mul = (a,b) => a*b;
export const sub = (a,b) => a-b;
```
**app.js**
```js
import { add } from "./math.js";
console.log(add(2,3));
```
**Bundle Result:** only `add` included → `mul` & `sub` removed.
---
### Visualization
```
math.js: { add, mul, sub }
   ↓ import { add }
bundle: { add }
```
🧠 **Mental Model:** Bundler “shakes the tree” → keeps only branches (exports) you actually use.
---
### Gotchas ⚠️ (Exhaustive)
1. **Only works with ESM**
   * `require()` (CommonJS) = dynamic → not tree-shakable.
2. **Side effects**
   * Functions with global effects may not be removed.
   ```js
   export const log = console.log("loaded"); // kept even if unused
   ```
3. **Default exports**
   * Harder to analyze; bundlers may keep entire module.
4. **Dynamic imports**
   * `import *` may keep more than needed.
5. **Package.json `"sideEffects"`**
   * Must declare `"sideEffects": false` for bundlers to safely drop unused files.
6. **Minification required**
   * Tree shaking often combined with Terser/Uglify to actually strip code.
7. **Circular dependencies**
   * Can prevent correct elimination.
---
### ✅ Pros
* Smaller bundles → faster load times.
* Removes dead code automatically.
* Encourages modular design.
---
### ❌ Cons
* Only works reliably with pure, side-effect-free modules.
* May keep unexpected code due to side effects.
* Debugging what was removed can be tricky.
---
### Best Practices
* ✅ Prefer **named exports** over default.
* ✅ Mark packages with `"sideEffects": false` in `package.json`.
* ✅ Write **pure functions** without hidden side effects.
* ✅ Avoid `import *` unless needed.
* ✅ Use modern bundlers (Webpack, Rollup, Vite, ESBuild).
---
👉 Mnemonic:
**“Import less, ship less.”**
---
⚡ Do you want me to now create a **Cheat Card comparing Tree Shaking in ESM vs CommonJS** (why one works, the other doesn’t) — for a sharp interview answer?

Here’s your **Concept Mastery Cheat Sheet** for **Module Resolution (Node.js & Browsers)** — sharp, visual, and interview-ready.
---
# 🔍 Module Resolution (Node.js & Browsers) — Cheat Card
**Concept:**
**Module resolution** = the process of locating and loading a module when you `import` or `require` it.
* **Node.js** → uses a hierarchical, file-system based algorithm.
* **Browsers** → expect explicit paths/URLs, or use bundlers/CDNs.
---
### Node.js Module Resolution
**Importing/Require types:**
```js
require("./local");   // relative path
require("fs");        // core module
require("express");   // npm package
```
**Resolution order:**
1. **Core modules** (built-in: `fs`, `path`, `http`) → resolved immediately.
2. **Relative/absolute paths**
   * `./`, `../`, or `/` → resolved from current file’s directory.
   * Searches with extensions: `.js`, `.json`, `.node`.
3. **Packages (node_modules)**
   * Search `node_modules` in current dir, then parent dirs, up to root.
   * Uses `package.json` → `"main"` or `"exports"`.
4. **Caching**
   * Modules loaded once and cached.
**Example:**
```js
// a.js
const util = require("util"); // core
const lib = require("./lib"); // relative file
```
---
### ES Modules in Node.js
```js
import { readFile } from "fs";
import myFn from "./utils.js";
```
* Needs `"type": "module"` in `package.json`, or `.mjs` extension.
* File extension required (`.js`, `.mjs`).
* Supports `import.meta.url` for file context.
---
### Browser Module Resolution
**Static import (must be explicit path):**
```html
<script type="module">
  import { add } from "./math.js";   // must use full path
</script>
```
**Dynamic import:**
```js
const mod = await import("./utils.js");
```
* No Node-style searching.
* Path/URL must be absolute or relative to file.
* Can import from CDNs:
  ```js
  import _ from "https://cdn.skypack.dev/lodash";
  ```
---
### Visualization
```
Node.js: core → relative/absolute → node_modules → parent dirs
Browser: explicit URL/path only (no node_modules)
```
---
### Gotchas ⚠️ (Exhaustive)
1. **Node.js CJS vs ESM**
   * Different resolution rules.
   * CJS auto-adds extensions; ESM requires explicit `.js`.
2. **package.json fields**
   * `"main"` (CJS entry), `"exports"` (ESM entry).
3. **index.js default**
   * Node auto-loads `./dir/index.js` if `./dir` is required.
4. **Browser strictness**
   * Must include extensions (e.g., `.js`).
5. **Bundlers**
   * Tools like Webpack/Vite simulate Node resolution for browsers.
6. **Bare imports in browsers**
   * Not supported natively → need import maps or bundlers.
7. **Case sensitivity**
   * On Linux/macOS, paths are case-sensitive.
---
### ✅ Pros
* Node.js: flexible resolution (core, npm, local).
* Browser: predictable and URL-based.
* Bundlers unify both worlds.
---
### ❌ Cons
* Node vs browser mismatch → confusion.
* ESM stricter (extensions required).
* Circular resolution may cause partial exports.
---
### Best Practices
* ✅ Always use explicit relative paths in ESM.
* ✅ Be consistent with extensions (`.js`).
* ✅ Avoid deep relative hell (`../../../`) → use aliases in bundlers.
* ✅ For browser code, prefer CDNs or bundlers.
* ✅ In Node, know when you’re in CJS vs ESM mode.
---
👉 Mnemonic:
**“Node resolves by search, Browser requires the path.”**
---
⚡ Would you like me to next prepare a **Cheat Card: Common Pitfalls in Module Resolution (extensions, index.js, interop issues)** — so you’re fully covered for tricky interview questions?


