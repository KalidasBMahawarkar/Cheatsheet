Here’s your **Concept Mastery Cheat Sheet** for **JavaScript Patterns & Best Practices** — concise, structured, and interview-ready.

---

# 🏗️ JS Patterns & Best Practices — Cheat Card

**Concept:**
**Patterns** = reusable solutions to common coding problems.
**Best Practices** = coding habits that improve clarity, performance, and maintainability.

---

### 1. Patterns

**Module Pattern (Pre-ES6)**

```js
const counter = (function(){
  let count = 0;
  return {
    inc: () => ++count,
    get: () => count
  };
})();
```

* Encapsulation, avoids globals.

**Revealing Module Pattern**

```js
const calc = (() => {
  let x = 0;
  function add(v){ x+=v; }
  return { add, get:()=>x };
})();
```

**Singleton**

```js
class DB {
  static instance;
  constructor(){ if (DB.instance) return DB.instance; DB.instance = this; }
}
```

**Observer (Pub/Sub)**

```js
class EventBus {
  constructor(){ this.events = {}; }
  on(ev, fn){ (this.events[ev] ||= []).push(fn); }
  emit(ev, data){ (this.events[ev]||[]).forEach(fn=>fn(data)); }
}
```

**Factory**

```js
function createUser(type) {
  if(type==="admin") return {role:"admin"};
  return {role:"user"};
}
```

**Strategy**

```js
const strategies = {
  add:(a,b)=>a+b, sub:(a,b)=>a-b
};
function exec(strategy,a,b){ return strategies[strategy](a,b); }
```

**Decorator**

```js
function withLog(fn){
  return (...args)=>{ console.log("Call", fn.name); return fn(...args); };
}
```

**IIFE**

```js
(function(){ console.log("runs immediately"); })();
```

---

### 2. Best Practices

**Code Style**

* Use `const` / `let` (avoid `var`).
* Use strict mode (`'use strict'`).
* Consistent naming (camelCase, PascalCase).
* Modularize code → SRP (Single Responsibility Principle).

**Error Handling**

* Always `try/catch` around async/await.
* Throw `Error` objects, not strings.
* Centralize logging/monitoring.

**Performance**

* Debounce/throttle high-frequency events.
* Minimize DOM reflows (batch updates).
* Use `requestAnimationFrame` for animations.
* Cache repeated calculations.

**Security**

* Escape/validate user input (XSS protection).
* Use `HttpOnly`, `Secure`, `SameSite` cookies for auth.
* Avoid `eval()` and `with`.

**Testing & Maintainability**

* Write unit tests (Jest, Mocha).
* Use linters (ESLint).
* Keep functions small, composable.
* Comment *why*, not *what*.

---

### Visualization

```
Patterns → common blueprints (Module, Singleton, Observer…)
Best Practices → rules of thumb (style, perf, security)
```

🧠 **Mental Model:** Patterns = reusable solutions; Best Practices = coding hygiene.

---

### Gotchas ⚠️ (Exhaustive)

1. **Overengineering** → don’t apply patterns blindly.
2. **Globals** → leak memory, naming conflicts.
3. **Arrow functions**

   * Great for callbacks, but don’t use where `this` is required.
4. **Promises without .catch** → unhandled rejections.
5. **Excessive delegation** → can hurt performance.
6. **Security slip** → DOM `innerHTML` = XSS vector.

---

### ✅ Pros

* Patterns → reusable, maintainable solutions.
* Best practices → stability, performance, readability.

---

### ❌ Cons

* Misused patterns add complexity.
* Strict best practices may reduce flexibility.

---

### Best Practices Summary

* ✅ Keep functions small & focused.
* ✅ Use ES Modules (not globals).
* ✅ Prefer immutability for safety.
* ✅ Profile code before optimizing.
* ✅ Apply patterns when problem fits.

---

👉 Mnemonic:
**“Pattern = blueprint. Practice = hygiene.”**

---

⚡ Would you like me to now create a **Cheat Card on “Design Patterns in JavaScript”** (GoF patterns adapted to JS — Singleton, Observer, Factory, Strategy, etc.) with deeper focus for interviews?


Here’s your **Concept Mastery Cheat Sheet** for **Functional Patterns in JavaScript** — precise, visual, and interview-ready.

---

# 🧩 Functional Patterns in JavaScript — Cheat Card

**Concept:**
Functional Programming (FP) emphasizes **immutability, pure functions, and composition**.
JS supports FP with **first-class functions, closures, and higher-order functions**.

---

### 1. Pure Functions

* Output depends only on input, no side effects.

```js
const add = (a, b) => a + b; // pure
```

✅ Easier to test, predictable.

---

### 2. Higher-Order Functions (HOFs)

* Functions that **take/return functions**.

```js
const twice = f => x => f(f(x));
const inc = x => x + 1;
twice(inc)(5); // 7
```

---

### 3. Function Composition

* Combine small functions → bigger ones.

```js
const compose = (f, g) => x => f(g(x));
const double = x => x * 2;
const square = x => x * x;
const doubleSquare = compose(square, double);
doubleSquare(3); // 36
```

---

### 4. Currying & Partial Application

**Currying**

```js
const add = a => b => a + b;
add(2)(3); // 5
```

**Partial Application**

```js
function multiply(a, b, c) { return a*b*c; }
const double = multiply.bind(null, 2);
double(3,4); // 24
```

---

### 5. Recursion & Tail Calls

```js
const fact = (n, acc=1) =>
  n <= 1 ? acc : fact(n-1, n*acc); // tail call
```

(⚠️ Tail-call optimization spec’d, but not widely supported).

---

### 6. Map / Filter / Reduce

```js
[1,2,3].map(x => x*2);     // [2,4,6]
[1,2,3].filter(x => x>1);  // [2,3]
[1,2,3].reduce((a,b)=>a+b,0); // 6
```

---

### 7. Immutability

```js
const arr = [1,2,3];
const newArr = [...arr, 4]; // don’t mutate original
```

Use **spread, Object.assign, immutable libs**.

---

### 8. Declarative Iteration

```js
// Imperative
let res = [];
for(let i=0;i<nums.length;i++) res.push(nums[i]*2);

// Declarative (FP)
const res2 = nums.map(x=>x*2);
```

---

### 9. Point-Free Style

* Define functions without mentioning arguments explicitly.

```js
const incAll = arr => arr.map(x => x+1);
```

---

### Visualization

```
Functional Core
 ├─ Pure functions
 ├─ HOFs (map, filter, reduce)
 ├─ Currying / Partial
 ├─ Composition
 └─ Immutability
```

🧠 **Mental Model:** Build apps by **gluing pure functions** together, not mutating state.

---

### Gotchas ⚠️ (Exhaustive)

1. **Performance cost** → FP can create many intermediate arrays.
2. **Recursion limits** → stack overflow if no TCO.
3. **Mutation traps** → forgetting immutability breaks FP principles.
4. **Closures** → can capture stale/large references → memory leaks.
5. **Readability** → point-free can be confusing if overused.
6. **Side effects still needed** (I/O, DOM) → must isolate them.

---

### ✅ Pros

* Easier to test/debug.
* Reusable, composable functions.
* Less state mutation = fewer bugs.
* Declarative, concise code.

---

### ❌ Cons

* Performance overhead (allocations).
* Harder to debug when over-abstracted.
* JS engines optimize imperative loops better.

---

### Best Practices

* ✅ Favor pure, small functions.
* ✅ Use map/filter/reduce over loops when clarity > speed.
* ✅ Isolate side effects at app boundaries.
* ✅ Use immutability helpers (spread, immutable.js).
* ✅ Compose functions for readability.

---

👉 Mnemonic:
**“Pure + Compose + Immutability = FP.”**

---

⚡ Next step: Do you want me to expand this into a **Cheat Card on Advanced FP Patterns** (Monads, Functors, Pipelines, Transducers) — for senior-level interviews?


Here’s your **Concept Mastery Cheat Sheet** for **Currying, Partial Application, and Composition in JavaScript** — dense, visual, and interview-ready.

---

# 🧮 Currying • Partial Application • Composition — Cheat Card

**Concept:**
These **functional programming patterns** transform how functions are written and combined.

* **Currying** → break multi-arg fn into unary functions.
* **Partial Application** → pre-fill some args.
* **Composition** → combine functions into pipelines.

---

### 1. Currying

Transforms: `f(a,b,c)` → `f(a)(b)(c)`

```js
const add = a => b => a + b;
add(2)(3); // 5
```

**Auto-curry helper:**

```js
const curry = f => (...args) =>
  args.length >= f.length
    ? f(...args)
    : curry(f.bind(null, ...args));

const sum = (a,b,c) => a+b+c;
curry(sum)(1)(2)(3); // 6
```

✅ Use cases: reusable functions, FP pipelines.
❌ Can reduce readability if overused.

---

### 2. Partial Application

Pre-fills some args, returns fn expecting rest.

```js
function multiply(a,b,c) { return a*b*c; }

const double = multiply.bind(null, 2);
double(3,4); // 24
```

Alternative with arrow:

```js
const partial = (fn, ...fixed) => (...rest) => fn(...fixed, ...rest);
const add = (a,b,c) => a+b+c;
const add5 = partial(add, 5);
add5(10, 2); // 17
```

✅ Great for config-driven functions.
❌ Can create too many small wrappers.

---

### 3. Function Composition

Combine fns right-to-left (classic math style).

```js
const compose = (f,g) => x => f(g(x));

const double = x => x*2;
const square = x => x*x;

const doubleSquare = compose(square, double);
doubleSquare(3); // 36
```

**Left-to-right (pipe style):**

```js
const pipe = (...fns) => x => fns.reduce((v,f) => f(v), x);
const pipeline = pipe(double, square);
pipeline(3); // 36
```

✅ Encourages declarative, reusable flows.
❌ Debugging composed pipelines can be tricky.

---

### Visualization

```
Currying → f(a)(b)(c)
Partial → f(a, _, c)
Compose → f(g(x))
Pipe    → (...fns)(x)
```

🧠 **Mental Model:**

* Currying = “one arg at a time.”
* Partial = “lock some args.”
* Composition = “chain functions.”

---

### Gotchas ⚠️ (Exhaustive)

1. **Currying vs Partial**

   * Currying splits into unary fns.
   * Partial just fixes some args.

2. **Context (`this`) loss**

   * `.bind` partials preserve `this`; arrow-based may not.

3. **Composition order**

   * `compose(f,g)(x) = f(g(x))`.
   * `pipe(f,g)(x) = g(f(x))`.

4. **Debugging pipelines**

   * Insert logs (`tap`) in between steps.

5. **Over-abstraction**

   * Excessive currying/composition reduces readability in real-world code.

---

### ✅ Pros

* Declarative, reusable function building.
* Easy configuration of functions.
* Clean separation of transformation steps.

### ❌ Cons

* Can confuse less experienced devs.
* Performance overhead (extra function wrappers).
* Debugging stack traces gets harder.

---

### Best Practices

* ✅ Use currying for libraries/pipelines, not everywhere.
* ✅ Use partial for configs (e.g., `fetchWithAuth = fetch.bind(...headers)`).
* ✅ Prefer `pipe` for left-to-right readability.
* ✅ Keep composed pipelines small & named.
* ✅ Add logging/taps for debug clarity.

---

👉 Mnemonic:
**“Curry = split, Partial = prefill, Compose = combine.”**

---

⚡ Do you want me to create a **Cheat Card on Advanced FP Utilities (`tap`, `memoize`, `throttle`/`debounce` in FP style)** — so you’ll have a complete FP interview toolkit?


Here’s your **Concept Mastery Cheat Sheet** for **OOP (Object-Oriented Programming) Patterns in JavaScript** — compact, structured, and interview-ready.

---

# 🏛️ OOP Patterns in JavaScript — Cheat Card

**Concept:**
JavaScript supports **OOP** through **prototypes** and **classes (ES6+)**.
OOP patterns model real-world entities with **encapsulation, inheritance, and polymorphism**.

---

### 1. Constructor Pattern

```js
function User(name) {
  this.name = name;
}
User.prototype.greet = function() { return `Hi, ${this.name}`; };

const u = new User("Kalidas");
```

* Encapsulates state + methods via `prototype`.

---

### 2. Class Pattern (ES6 sugar)

```js
class User {
  constructor(name) { this.name = name; }
  greet() { return `Hi, ${this.name}`; }
}
```

* Cleaner syntax over prototypes.

---

### 3. Inheritance Pattern

```js
class Animal {
  speak() { return "sound"; }
}
class Dog extends Animal {
  speak() { return "woof"; } // override
}
```

* **IS-A** relationship (Dog IS-A Animal).

---

### 4. Encapsulation

```js
class Counter {
  #count = 0;       // private field
  inc() { return ++this.#count; }
  get value() { return this.#count; }
}
```

* Private fields (`#`) hide internal state.

---

### 5. Polymorphism

* Methods with the same interface, different implementations.

```js
class Shape { area() { throw "abstract"; } }
class Circle extends Shape {
  constructor(r) { super(); this.r=r; }
  area() { return Math.PI * this.r * this.r; }
}
class Square extends Shape {
  constructor(s) { super(); this.s=s; }
  area() { return this.s * this.s; }
}
```

---

### 6. Singleton Pattern

```js
class DB {
  static instance;
  constructor() {
    if (DB.instance) return DB.instance;
    DB.instance = this;
  }
}
```

* Ensures only one instance.

---

### 7. Factory Pattern

```js
class UserFactory {
  static create(type) {
    if (type === "admin") return { role: "admin" };
    return { role: "user" };
  }
}
```

* Abstracts object creation logic.

---

### 8. Observer Pattern (OOP Style)

```js
class Subject {
  observers = [];
  subscribe(fn){ this.observers.push(fn); }
  notify(data){ this.observers.forEach(fn=>fn(data)); }
}
```

* Encapsulates pub/sub.

---

### Visualization

```
OOP Core
 ├─ Constructor/Class
 ├─ Inheritance
 ├─ Encapsulation
 ├─ Polymorphism
 └─ Patterns (Singleton, Factory, Observer)
```

🧠 **Mental Model:** OOP = “Objects model entities with state + behavior.”

---

### Gotchas ⚠️ (Exhaustive)

1. **JS isn’t classical OOP** → prototype-based, classes are syntax sugar.
2. **Multiple inheritance** not supported (use mixins/composition).
3. **Private fields** (#) not accessible outside.
4. **this binding** issues → methods lose `this` if detached.
5. **Singletons** can make testing harder.
6. **Factories** may lose clarity vs `new`.
7. **Overusing inheritance** → rigid code (prefer composition).

---

### ✅ Pros

* Models real-world problems.
* Encapsulation improves modularity.
* Reuse via inheritance/polymorphism.
* Familiar pattern across languages.

---

### ❌ Cons

* Overuse → rigid, complex hierarchy.
* In JS, **prototypal inheritance + composition** often simpler.
* Harder to test vs pure functions.

---

### Best Practices

* ✅ Prefer **composition over inheritance** when possible.
* ✅ Keep classes small, single responsibility.
* ✅ Use private fields for encapsulation.
* ✅ Avoid global singletons → inject dependencies.
* ✅ Mix OOP + FP where suitable.

---

👉 Mnemonic:
**“OOP = Encapsulate + Inherit + Reuse + Model.”**

---

⚡ Do you want me to prepare a **Cheat Card on “Design Patterns in JS (OOP-style)”** (Factory, Singleton, Observer, Decorator, Strategy) — as a deeper extension of this?

Here’s your **Concept Mastery Cheat Sheet** for **Constructor, Factory, and Singleton Patterns in JavaScript** — structured, visual, and interview-ready.

---

# 🏗️ Constructor • Factory • Singleton — Cheat Card

**Concept:**
These are **object creation patterns** in OOP/JS.

* **Constructor** → create objects with `new`.
* **Factory** → function/class returns objects (no `new` required).
* **Singleton** → ensures only one instance exists.

---

### 1. Constructor Pattern

```js
function User(name, role) {
  this.name = name;
  this.role = role;
}
User.prototype.greet = function() {
  return `Hi, I am ${this.name}, ${this.role}`;
};

const u1 = new User("Kalidas", "Admin");
```

* Uses `new` → auto-creates object, sets `this`.
* Methods shared via `prototype`.

✅ Pros: Familiar OOP, efficient with prototypes.
❌ Cons: Must remember `new` (without it → `this` bugs).

---

### 2. Factory Pattern

```js
function createUser(name, role) {
  return {
    name,
    role,
    greet() { return `Hi, I am ${this.name}, ${this.role}`; }
  };
}

const u2 = createUser("Kalidas", "User");
```

* Encapsulates object creation logic.
* No `new` required.
* Can return different types (Admin/User/etc).

✅ Pros: Flexible, hides creation complexity.
❌ Cons: Methods not shared (unless using prototypes/classes → memory overhead).

---

### 3. Singleton Pattern

```js
class DB {
  static instance;
  constructor(conn) {
    if (DB.instance) return DB.instance; // return existing
    this.conn = conn;
    DB.instance = this;
  }
}

const db1 = new DB("db://main");
const db2 = new DB("db://other");
console.log(db1 === db2); // true
```

* Ensures **only one instance**.
* Useful for DB connections, config, logging.

✅ Pros: Guarantees global shared instance.
❌ Cons: Hidden dependencies, hard to test/mock.

---

### Visualization

```
Constructor → new User()
Factory     → createUser()
Singleton   → same instance every time
```

🧠 **Mental Model:**

* **Constructor** = blueprint.
* **Factory** = workshop.
* **Singleton** = one-and-only.

---

### Gotchas ⚠️

1. Constructor: forgetting `new` → `this` leaks to global (in sloppy mode).
2. Factory: methods not memory-efficient if created inline per object.
3. Singleton: introduces global state, makes unit testing harder.

---

### Best Practices

* ✅ Use **Constructors/Classes** for standard OOP objects.
* ✅ Use **Factory** for flexible creation logic.
* ✅ Use **Singleton** sparingly (only for global resources).
* ✅ Prefer **dependency injection** over Singleton when possible.
* ✅ Mix patterns depending on use case.

---

👉 Mnemonic:
**“Constructor = new, Factory = create, Singleton = one.”**

---

⚡ Next step: Do you want me to also prepare a **Cheat Card comparing Factory vs Constructor vs Class (when to use each)** — since that’s a frequent interview follow-up?

Here’s your **Concept Mastery Cheat Sheet** for **Async Patterns in JavaScript** — crisp, comparative, and fully interview-ready.

---

# ⚡ Async Patterns in JavaScript — Cheat Card

**Concept:**
JavaScript is **single-threaded**, so async patterns help avoid blocking the event loop.
Main patterns evolved over time: **Callbacks → Promises → async/await → Advanced (Generators, Observables).**

---

### 1. Callbacks

```js
fs.readFile("file.txt", (err, data) => {
  if (err) return console.error(err);
  console.log(data);
});
```

✅ Simple, widely supported.
❌ Callback Hell → nested, hard to manage errors.

---

### 2. Promises

```js
fetch("/api")
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

* **States**: pending → fulfilled/rejected.
* Chainable, better error handling.

---

### 3. async / await

```js
try {
  const res = await fetch("/api");
  const data = await res.json();
  console.log(data);
} catch (e) {
  console.error(e);
}
```

* Sugar over Promises.
* Linear, synchronous-like style.
* Still Promise-based under the hood.

---

### 4. Promise Utilities

```js
await Promise.all([p1, p2]);       // all must fulfill
await Promise.allSettled([p1, p2]); // statuses of all
await Promise.race([p1, p2]);       // first settled wins
await Promise.any([p1, p2]);        // first fulfilled wins
```

---

### 5. Generators (async control)

```js
function* gen() {
  yield fetch("/a");
  yield fetch("/b");
}
```

* Pausable functions.
* Used by libs like `co` before async/await.

---

### 6. Observables (RxJS)

```js
const { fromEvent } = rxjs;
fromEvent(button, "click").subscribe(e => console.log(e));
```

* Streams of values over time.
* Powerful for UI/reactive systems.

---

### Visualization

```
Callbacks → nested hell
Promises  → flat chain
Async/await → clean, linear
Generators → manual async control
Observables → async streams
```

🧠 **Mental Model:** Async patterns = evolution towards **clarity + composability**.

---

### Gotchas ⚠️ (Exhaustive)

1. **Callbacks**

   * Hard to handle errors across chains.
   * Pyramid of doom (nested calls).

2. **Promises**

   * Must `.catch()` or errors go unhandled.
   * `then` returns new promise → chaining quirks.

3. **async/await**

   * Sequential by default → can kill performance if not parallelized.
   * Wrap in `Promise.all` for concurrency.

4. **Generators**

   * Not natively async → need runner libs (`co`).

5. **Observables**

   * Not built-in; require RxJS or similar.
   * Steep learning curve.

6. **Unhandled rejections** (Node ≥ v15 crash by default).

---

### ✅ Pros

* Multiple tools for different use cases.
* async/await = cleaner control flow.
* Promises/utilities = powerful composition.
* Observables = advanced async (streams).

---

### ❌ Cons

* Too many options → confusion.
* Poor error handling if not used carefully.
* Observables add extra complexity.

---

### Best Practices

* ✅ Prefer async/await for readability.
* ✅ Use `Promise.all` for parallel async tasks.
* ✅ Always catch errors (`try/catch` or `.catch()`).
* ✅ Avoid callback-style APIs if Promise versions exist.
* ✅ Use Observables only for event streams, not simple async.

---

👉 Mnemonic:
**“Callback → Promise → Await → Stream.”**

---

⚡ Would you like me to now prepare a **Cheat Card on Async Error Handling Patterns** (try/catch vs `.catch` vs global handlers) — since that’s often a tricky interview follow-up?


Here’s your **Concept Mastery Cheat Sheet** for **Callback → Promise → async/await** — showing the **evolution of async patterns** in JavaScript.

---

# ⚡ Callback → Promise → async/await — Cheat Card

**Concept:**
JS async handling evolved to improve **readability, composability, and error handling**:

1. **Callbacks** (early async model).
2. **Promises** (structured async).
3. **async/await** (syntactic sugar over Promises).

---

### 1. Callbacks

```js
function getData(cb) {
  setTimeout(() => cb(null, "done"), 1000);
}

getData((err, res) => {
  if (err) return console.error(err);
  console.log(res);
});
```

✅ Pros: Simple, widely supported.
❌ Cons: **Callback hell**, error handling messy, inversion of control.

---

### 2. Promises

```js
function getData() {
  return new Promise(resolve =>
    setTimeout(() => resolve("done"), 1000)
  );
}

getData()
  .then(res => console.log(res))
  .catch(err => console.error(err));
```

✅ Pros:

* Flat chains (`then`) reduce nesting.
* Composability (`Promise.all`, `race`, etc.).
* Centralized error handling (`catch`).

❌ Cons:

* Still verbose (`then` chaining).
* Debugging async stacks can be tricky.

---

### 3. async/await

```js
async function run() {
  try {
    const res = await getData();
    console.log(res);
  } catch (err) {
    console.error(err);
  }
}
run();
```

✅ Pros:

* **Synchronous look** for async code.
* Integrated error handling with `try/catch`.
* Easier debugging & step-through.

❌ Cons:

* Sequential by default (need `Promise.all` for concurrency).
* Requires ES2017+.

---

### Visualization

```
Callbacks → deeply nested
Promises  → flat, chainable
async/await → linear, readable
```

🧠 **Mental Model:**

* **Callback** → pass function in.
* **Promise** → return an object representing future value.
* **async/await** → write async like sync.

---

### Gotchas ⚠️ (Exhaustive)

1. **Callbacks** → “Pyramid of Doom” when chaining.
2. **Promises** → must `.catch()`, or rejection becomes unhandled.
3. **async/await** →

   * Each `await` waits sequentially unless wrapped in `Promise.all`.
   * Errors must be wrapped in `try/catch`.

---

### ✅ Pros of Evolution

* Callbacks → foundation, event-driven.
* Promises → structured async, error handling.
* async/await → cleaner, modern async style.

---

### ❌ Cons of Evolution

* Callbacks → unmanageable for complex flows.
* Promises → chaining boilerplate.
* async/await → sequential pitfalls if misused.

---

### Best Practices

* ✅ Prefer **async/await** for readability.
* ✅ Use **Promises utilities** for parallel async work.
* ✅ Avoid callback-based APIs (wrap in Promises).
* ✅ Always handle errors with `.catch` or `try/catch`.

---

👉 Mnemonic:
**“Callback = Nest, Promise = Chain, Await = Sync style.”**

---

⚡ Would you like me to now prepare a **Cheat Card on Mixing Patterns** (e.g., converting callbacks → Promises with `util.promisify`, combining Promises + async/await)? That’s a common **interview-level twist**.


Here’s your **Concept Mastery Cheat Sheet** for **Throttling & Debouncing in JavaScript** — crisp, practical, and interview-ready.

---

# ⏱️ Throttling vs Debouncing — Cheat Card

**Concept:**
Both are **rate-limiting techniques** to control how often a function executes in response to frequent events (scroll, resize, input).

* **Throttle** → ensure fn runs at most **once every X ms**.
* **Debounce** → delay fn until after events stop for **X ms**.

---

### 1. Throttling

```js
function throttle(fn, delay) {
  let last = 0;
  return (...args) => {
    const now = Date.now();
    if (now - last >= delay) {
      fn(...args);
      last = now;
    }
  };
}

// Usage
window.addEventListener("scroll", throttle(() => {
  console.log("Scroll handler");
}, 200));
```

✅ Runs regularly at intervals.
❌ May miss last event if user stops abruptly.

---

### 2. Debouncing

```js
function debounce(fn, delay) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}

// Usage
input.addEventListener("input", debounce(() => {
  console.log("Search triggered");
}, 300));
```

✅ Runs only once after user stops triggering.
❌ Doesn’t run during continuous events (only at end).

---

### Visualization

```
Events:  *************
Throttle: *     *     *
Debounce:              *
```

🧠 **Mental Model:**

* **Throttle** → steady pace.
* **Debounce** → wait for silence.

---

### Gotchas ⚠️ (Exhaustive)

1. **Throttle**

   * May skip final call → use trailing option if needed.
   * Useful for scroll/resize animations.

2. **Debounce**

   * User typing → runs only after pause.
   * Without immediate option, first event is delayed too.

3. **Implementation variants**

   * Both can be written with leading/trailing edge execution configs.

4. **Context (`this`)**

   * Must use arrow fn or `fn.apply(this, args)` inside wrapper.

---

### ✅ Pros

* Improves performance for high-frequency events.
* Prevents unnecessary server requests.
* Smoother UX.

---

### ❌ Cons

* Adds slight delay in execution.
* Must choose correct strategy (wrong one = UX issues).
* More complex code than naive event handlers.

---

### Best Practices

* ✅ Use **debounce** for input validation, auto-save, search fields.
* ✅ Use **throttle** for scroll, resize, mousemove handlers.
* ✅ Allow config (leading/trailing).
* ✅ Test UX impact (too short/long delays hurt experience).

---

👉 Mnemonic:
**“Throttle = limit rate, Debounce = wait silence.”**

---

⚡ Want me to create a **Cheat Card on Leading vs Trailing Execution (debounce/throttle variants)**? That’s often an **interview-level twist** where candidates get stuck.


Here’s your **Concept Mastery Cheat Sheet** for **Retry with Backoff in JavaScript** — compact, visual, and interview-ready.

---

# 🔄 Retry with Backoff — Cheat Card

**Concept:**
Retrying = re-attempting a failed operation.
**Backoff** = spacing retries with increasing delays to reduce load & collisions.

* **Fixed backoff** → constant delay.
* **Exponential backoff** → delay grows exponentially.
* **Jitter** → randomization to avoid thundering herd.

---

### Basic Retry

```js
async function retry(fn, retries = 3, delay = 1000) {
  for (let i = 0; i < retries; i++) {
    try {
      return await fn();
    } catch (e) {
      if (i === retries - 1) throw e;
      await new Promise(r => setTimeout(r, delay));
    }
  }
}
```

---

### Exponential Backoff

```js
async function retryWithBackoff(fn, retries = 5, delay = 500) {
  for (let i = 0; i < retries; i++) {
    try {
      return await fn();
    } catch (e) {
      if (i === retries - 1) throw e;
      const backoff = delay * 2 ** i;
      await new Promise(r => setTimeout(r, backoff));
    }
  }
}
```

---

### Backoff with Jitter

```js
function jitter(base) {
  return Math.random() * base;
}

async function retryWithJitter(fn, retries = 5, delay = 500) {
  for (let i = 0; i < retries; i++) {
    try {
      return await fn();
    } catch (e) {
      if (i === retries - 1) throw e;
      const backoff = delay * 2 ** i + jitter(1000);
      await new Promise(r => setTimeout(r, backoff));
    }
  }
}
```

---

### Visualization

```
Fixed:        1s — 1s — 1s
Exponential:  0.5s — 1s — 2s — 4s
With Jitter:  0.5s ± rand — 1s ± rand — 2s ± rand
```

🧠 **Mental Model:** Start small, back off exponentially, add jitter to avoid sync retries.

---

### Gotchas ⚠️ (Exhaustive)

1. **Don’t retry everything**

   * Retry only transient errors (e.g., 500, network timeout).
   * Don’t retry 400/validation errors.

2. **Exponential backoff can explode**

   * Add **max cap** (e.g., 30s).

3. **Jitter is critical**

   * Without jitter, many clients retry at the same time → overload server.

4. **Async context**

   * `await` inside loops ensures delays happen correctly.

5. **Abort support**

   * Combine with `AbortController` for cancellable retries.

---

### ✅ Pros

* Improves resilience.
* Handles flaky network/services gracefully.
* Prevents server overload (with jitter).

---

### ❌ Cons

* Increases latency.
* Complexity (state, timers).
* Wrong config (too many retries) = worse outages.

---

### Best Practices

* ✅ Retry only **idempotent** operations (GET, safe POSTs).
* ✅ Use exponential backoff + jitter.
* ✅ Cap max retries & max delay.
* ✅ Log failures for monitoring.
* ✅ Provide cancellation with `AbortController`.

---

👉 Mnemonic:
**“Retry smart: exponential + jitter + cap.”**

---

⚡ Do you want me to also prepare a **Cheat Card on Circuit Breaker Pattern** (often paired with retry/backoff to protect services) — since it’s another common interview follow-up?


Here’s your **Concept Mastery Cheat Sheet** for **Module Patterns in JavaScript** — compact, comparative, and interview-ready.

---

# 📦 JavaScript Module Patterns — Cheat Card

**Concept:**
Modules = encapsulate code into **independent, reusable units**.
They manage **scope, dependencies, and exports**.
Patterns evolved: **IIFE (pre-ES6) → CommonJS → AMD → UMD → ES Modules (modern).**

---

### 1. IIFE (Immediately Invoked Function Expression)

```js
const Counter = (function () {
  let count = 0; // private
  return {
    inc: () => ++count,
    get: () => count
  };
})();
Counter.inc(); // 1
```

✅ Encapsulation (private vars).
❌ No built-in dependency mgmt.

---

### 2. Revealing Module Pattern

```js
const Calc = (function () {
  let x = 0;
  function add(v) { x += v; }
  function get() { return x; }
  return { add, get }; // reveal public API
})();
```

✅ Cleaner public API.
❌ Still manual dependency management.

---

### 3. CommonJS (Node.js)

```js
// math.js
module.exports = {
  add: (a,b) => a+b
};

// app.js
const math = require("./math");
console.log(math.add(2,3));
```

✅ Synchronous, simple, widely used in Node.
❌ Not for browsers (without bundlers).

---

### 4. AMD (Asynchronous Module Definition)

```js
define(["dep"], function(dep) {
  return { hi: () => dep.say("Hi") };
});
```

✅ Async loading in browsers.
❌ Verbose, rarely used today.

---

### 5. UMD (Universal Module Definition)

```js
(function (root, factory) {
  if (typeof module === "object" && module.exports)
    module.exports = factory();
  else
    root.MyLib = factory();
}(this, function () {
  return { hi: () => "Hi" };
}));
```

✅ Works in Node & browsers.
❌ Boilerplate-heavy.

---

### 6. ES Modules (Modern, ES6+)

```js
// math.js
export const add = (a,b) => a+b;

// app.js
import { add } from "./math.js";
console.log(add(2,3));
```

✅ Native support, static analysis, tree-shaking.
❌ Must use bundlers in older environments.

---

### Visualization

```
Pre-ES6 → IIFE, Revealing
Node.js → CommonJS
Browser async → AMD
Cross-env → UMD
Modern → ES Modules
```

🧠 **Mental Model:**
Modules = “containers + exports.”
ESM = standard; older ones = compatibility hacks.

---

### Gotchas ⚠️ (Exhaustive)

1. **IIFE** → pollutes global scope if not careful.
2. **CommonJS** → synchronous, not good for browsers.
3. **AMD** → outdated, verbose.
4. **UMD** → transitional, heavy boilerplate.
5. **ESM** → strict mode by default, must use `type="module"` in HTML.
6. **ESM imports are static** → can’t conditionally import without `import()`.
7. **Mixing CommonJS & ESM** in Node can be tricky (default vs named exports).

---

### ✅ Pros

* Encapsulation, reusability.
* Dependency management.
* ES Modules → tree shaking & optimization.

---

### ❌ Cons

* Too many competing formats historically.
* Legacy formats add complexity.
* Browser vs Node differences.

---

### Best Practices

* ✅ Use **ES Modules** (standard, tree-shakable).
* ✅ Avoid legacy AMD/UMD unless working with old libs.
* ✅ Group related functions/classes inside modules.
* ✅ Use index.js barrels for re-exports.
* ✅ Keep modules small, single responsibility.

---

👉 Mnemonic:
**“IIFE → CommonJS → ESM = evolution of modules.”**

---

⚡ Want me to now prepare a **Cheat Card on “ESM vs CommonJS deep-dive”** (interoperability, default vs named exports, Node resolution quirks) — since that’s a common senior-level interview twist?


Here’s your **Concept Mastery Cheat Sheet** for **Error Handling Patterns in JavaScript** — crisp, structured, and interview-ready.

---

# 🚨 Error Handling Patterns in JavaScript — Cheat Card

**Concept:**
Error handling = detecting, reporting, and recovering from unexpected conditions.
JS offers **sync (try/catch)**, **async (Promises, async/await)**, and **global handling patterns**.

---

### 1. Synchronous (try/catch/finally)

```js
try {
  risky();
} catch (err) {
  console.error("Handled:", err.message);
} finally {
  cleanup();
}
```

✅ Captures exceptions thrown synchronously.
❌ Doesn’t catch async errors by default.

---

### 2. Callback Error-First Pattern

```js
fs.readFile("file.txt", (err, data) => {
  if (err) return console.error("Error:", err);
  console.log(data);
});
```

* Node.js standard: `(err, result)` signature.

---

### 3. Promises (.catch)

```js
fetch("/api")
  .then(res => res.json())
  .catch(err => console.error("Failed:", err));
```

* Errors bubble down to nearest `.catch`.

---

### 4. async/await with try/catch

```js
async function run() {
  try {
    const res = await fetch("/api");
    return await res.json();
  } catch (err) {
    console.error("Error:", err);
  }
}
```

* Cleaner than `.catch` chains.

---

### 5. Error Propagation

```js
async function a() {
  throw new Error("fail");
}
a()
  .catch(err => console.error("Caught:", err))
  .then(() => console.log("Continues…"));
```

* Errors propagate until handled.

---

### 6. Retry + Backoff Pattern

```js
async function retry(fn, retries=3) {
  for (let i=0;i<retries;i++) {
    try { return await fn(); }
    catch(e) { if (i===retries-1) throw e; }
  }
}
```

* Useful for transient network errors.

---

### 7. Graceful Fallbacks

```js
function parseJSON(str) {
  try { return JSON.parse(str); }
  catch { return {}; } // fallback value
}
```

* Prevents crashes, provides safe defaults.

---

### 8. Global Error Handlers

**Browser:**

```js
window.onerror = (msg, src, line, col, err) => {
  console.error("Global:", err);
};
window.onunhandledrejection = e => {
  console.error("Unhandled promise:", e.reason);
};
```

**Node.js:**

```js
process.on("uncaughtException", err => console.error(err));
process.on("unhandledRejection", err => console.error(err));
```

---

### Visualization

```
try/catch → sync code
.catch    → async Promises
try/await → async clarity
Global    → last resort
```

🧠 **Mental Model:** Handle **locally if possible**, escalate if not, fallback globally.

---

### Gotchas ⚠️ (Exhaustive)

1. **try/catch doesn’t catch async** unless awaited.
2. **Unhandled rejections** crash Node (>= v15).
3. **Global handlers** are last resort, not substitutes.
4. **Swallowing errors silently** makes debugging impossible.
5. **Retry loops without backoff** can overload servers.
6. **finally always runs** (even after return/throw).

---

### ✅ Pros

* Multiple levels of defense.
* Clear async error handling.
* Global safety nets.

---

### ❌ Cons

* Misuse → swallowed or ignored errors.
* Retry patterns can increase latency.
* Global catch hides root cause if abused.

---

### Best Practices

* ✅ Always handle async errors (`.catch` or try/await).
* ✅ Provide meaningful error messages.
* ✅ Use custom error classes for clarity.
* ✅ Log + monitor errors in production.
* ✅ Fail gracefully (fallbacks, retries).
* ✅ Keep global handlers only as a safety net.

---

👉 Mnemonic:
**“Catch local, fallback global, never swallow silently.”**

---

⚡ Do you want me to prepare a **Cheat Card on Custom Error Classes & Hierarchies** (e.g., `ValidationError`, `NetworkError`, `AuthError`) — since that’s often expected in senior-level interviews?


Here’s your **Concept Mastery Cheat Sheet** for **Immutable Data Patterns in JavaScript** — sharp, practical, and interview-ready.

---

# 🧊 Immutable Data Patterns — Cheat Card

**Concept:**
**Immutability** = once created, data cannot be changed.
Instead of mutating objects/arrays, return **new copies**.

* Prevents accidental side effects.
* Essential for FP, React, Redux, concurrency safety.

---

### 1. Objects

```js
const user = { name: "Kalidas", age: 27 };

// ❌ Mutation
user.age = 28;

// ✅ Immutable update
const updated = { ...user, age: 28 };
```

**Deep copy (structuredClone):**

```js
const deepCopy = structuredClone(user);
```

---

### 2. Arrays

```js
const arr = [1,2,3];

// Add
const arr2 = [...arr, 4];

// Remove
const arr3 = arr.filter(x => x !== 2);

// Update
const arr4 = arr.map(x => x === 2 ? 20 : x);
```

---

### 3. Strings (already immutable)

```js
let s = "hello";
s[0] = "H";   // ❌ no effect
s = "Hello";  // ✅ reassign
```

---

### 4. Object.freeze (shallow)

```js
const obj = Object.freeze({ a: 1 });
obj.a = 2; // ❌ ignored or error (strict mode)
```

* Shallow only → nested objects still mutable.

---

### 5. Persistent Data Structures (Libs)

```js
// Immutable.js example
const { Map } = require("immutable");
let m1 = Map({ x: 1 });
let m2 = m1.set("x", 2);
```

* Returns new structures with **structural sharing**.

---

### 6. Pure Functions

```js
function addItem(list, item) {
  return [...list, item]; // return new list
}
```

* No in-place mutation.

---

### Visualization

```
Old Object  ──X──> (never changed)
   ↓
New Object (copied + updated)
```

🧠 **Mental Model:** Treat data like **snapshots** instead of clay.

---

### Gotchas ⚠️ (Exhaustive)

1. **Spread & Object.assign are shallow**

   * Deep nested objects still mutable.
   * Use `structuredClone` or libraries.

2. **Performance overhead**

   * Copying large objects/arrays can be costly.

3. **Object.freeze is shallow**

   * Doesn’t protect nested objects.

4. **Reference equality**

   * Immutable updates create new refs → good for React/Redux diffing.

5. **Mutation by libraries**

   * Some APIs mutate (e.g., `Array.prototype.sort`).
   * Always check if method is mutating.

---

### ✅ Pros

* Predictable state changes.
* Easier debugging & testing.
* Works well with React/Redux.
* Prevents unintended side effects.

---

### ❌ Cons

* Performance overhead if naive.
* Boilerplate without helpers.
* Need special libraries for deep immutability.

---

### Best Practices

* ✅ Use spread (`...`) or array methods (`map`, `filter`, `concat`).
* ✅ Use `structuredClone` for safe deep copies.
* ✅ Avoid mutating built-ins (sort, splice).
* ✅ Use libraries (Immer, Immutable.js) for large state trees.
* ✅ In React, always return new objects for state updates.

---

👉 Mnemonic:
**“Don’t mutate → duplicate + update.”**

---

⚡ Do you want me to prepare a **Cheat Card on Libraries for Immutability** (Immer, Immutable.js, Mori, seamless-immutable) with pros/cons, since interviews often ask about scaling immutability in real-world apps?


Here’s your **Concept Mastery Cheat Sheet** for **Proxy & Reflect API in JavaScript** — compact, detailed, and interview-ready.

---

# 🪞 Proxy & Reflect API — Cheat Card

**Concept:**

* **Proxy** = wrapper around an object/function that intercepts operations (get, set, apply, etc.).
* **Reflect** = built-in API to perform default object operations (mirrors Proxy traps).
  Together, they allow **meta-programming** (customizing language behavior).

---

### 1. Proxy Basics

```js
const target = { x: 42 };

const proxy = new Proxy(target, {
  get(obj, prop) {
    console.log("get:", prop);
    return obj[prop];
  },
  set(obj, prop, val) {
    console.log("set:", prop, "=", val);
    obj[prop] = val;
    return true;
  }
});

proxy.x;      // logs get
proxy.y = 10; // logs set
```

---

### 2. Common Traps

| Trap                                 | Triggered by                 |
| ------------------------------------ | ---------------------------- |
| `get`                                | property access (`obj.prop`) |
| `set`                                | property write               |
| `has`                                | `prop in obj`                |
| `deleteProperty`                     | `delete obj.prop`            |
| `apply`                              | function call                |
| `construct`                          | `new obj()`                  |
| `ownKeys`                            | `Object.keys`, `for...in`    |
| `getOwnPropertyDescriptor`           | property descriptors         |
| `defineProperty`                     | `Object.defineProperty`      |
| `getPrototypeOf` / `setPrototypeOf`  | prototypes                   |
| `isExtensible` / `preventExtensions` | extensibility                |
| `enumerate` (legacy)                 | for-in loops                 |

---

### 3. Reflect API

```js
const obj = { a: 1 };

// Instead of direct ops
obj.a = 2;

// Use Reflect
Reflect.set(obj, "a", 2);
Reflect.get(obj, "a"); // 2
Reflect.has(obj, "a"); // true
Reflect.deleteProperty(obj, "a");
```

* Ensures **default behavior** inside Proxy traps.

```js
const proxy = new Proxy(obj, {
  get(t, p, r) {
    console.log("Intercept:", p);
    return Reflect.get(t, p, r); // forward safely
  }
});
```

---

### 4. Use Cases

✅ **Validation**

```js
const person = new Proxy({}, {
  set(obj, prop, val) {
    if (prop === "age" && val < 0) throw new Error("Invalid age");
    return Reflect.set(obj, prop, val);
  }
});
```

✅ **Default Values**

```js
const withDefaults = new Proxy({}, {
  get: (obj, prop) => prop in obj ? obj[prop] : "N/A"
});
```

✅ **Function Interception**

```js
const sum = (a,b) => a+b;
const logged = new Proxy(sum, {
  apply(fn, thisArg, args) {
    console.log("call:", args);
    return Reflect.apply(fn, thisArg, args);
  }
});
```

✅ **Revocable Proxy**

```js
const { proxy, revoke } = Proxy.revocable({}, {});
revoke(); // proxy unusable
```

---

### Visualization

```
Object  <— Proxy (traps ops) —> Reflect (default behavior)
```

🧠 **Mental Model:** Proxy = gatekeeper, Reflect = toolkit for safe defaults.

---

### Gotchas ⚠️ (Exhaustive)

1. **Performance cost** — Proxies are slower than direct access.
2. **Not polyfillable** — must be natively supported.
3. **Infinite recursion risk** if trap doesn’t call `Reflect`.
4. **JSON.stringify ignores proxies** (serializes target).
5. **Revoked proxies** throw on any operation.
6. **`this` context quirks** — traps need correct `receiver` arg.
7. **Invariants** — traps must follow rules (e.g., can’t report nonexistent property as existing if not in target).

---

### ✅ Pros

* Encapsulation & validation.
* Virtualized objects (default values, DB wrappers, API mocking).
* Debugging/logging tools.
* Foundation for frameworks (Vue, MobX).

---

### ❌ Cons

* Slower than native ops.
* Harder debugging (abstracted ops).
* Invariants can cause unexpected errors.

---

### Best Practices

* ✅ Use Proxies for **cross-cutting concerns** (logging, validation, defaults).
* ✅ Always use **Reflect** inside traps for safety.
* ✅ Don’t overuse Proxies in performance-critical paths.
* ✅ Consider simpler patterns (getter/setter) if Proxy is overkill.

---

👉 Mnemonic:
**“Proxy intercepts, Reflect executes.”**

---

⚡ Do you want me to also prepare a **Cheat Card on Real-World Proxy Uses (e.g., Vue reactivity, API mocks, access control, caching)** — since interviews often go beyond theory?



Here’s your **Concept Mastery Cheat Sheet** for **Design Patterns in JavaScript (Observer, Decorator, Strategy)** — concise, code-backed, and interview-ready.

---

# 🏗️ Design Patterns in JavaScript — Observer, Decorator, Strategy

**Concept:**
Design Patterns = reusable **blueprints** for solving common problems in software design.
These three are frequently asked in **JS interviews**.

---

## 👀 1. Observer Pattern

**Idea:** Objects (subscribers) watch another object (subject) and react to changes.

```js
class Subject {
  constructor(){ this.observers = []; }
  subscribe(fn){ this.observers.push(fn); }
  unsubscribe(fn){ this.observers = this.observers.filter(o => o!==fn); }
  notify(data){ this.observers.forEach(fn => fn(data)); }
}

// Usage
const subject = new Subject();
subject.subscribe(data => console.log("Observer A:", data));
subject.notify("Hello!"); // Observer A: Hello!
```

✅ Pros: Decoupled, async-friendly (events).
❌ Cons: Can grow complex with many observers.

📌 Real-world: EventEmitter, DOM `addEventListener`.

---

## 🎨 2. Decorator Pattern

**Idea:** Dynamically add/modify behavior of objects/functions without altering their source.

```js
function withLogging(fn) {
  return (...args) => {
    console.log("Calling:", fn.name, "with", args);
    return fn(...args);
  };
}

const sum = (a,b) => a+b;
const loggedSum = withLogging(sum);

loggedSum(2,3); // Logs + 5
```

✅ Pros: Extensible without modifying core code.
❌ Cons: Can lead to too many wrappers → complexity.

📌 Real-world: Middleware (Express.js), React HOCs.

---

## 🎯 3. Strategy Pattern

**Idea:** Define a family of algorithms, encapsulate them, and make them interchangeable.

```js
class PaymentStrategy {
  pay(amount){ throw "Not implemented"; }
}
class CardPayment extends PaymentStrategy {
  pay(amount){ console.log(`Paid ${amount} with Card`); }
}
class UpiPayment extends PaymentStrategy {
  pay(amount){ console.log(`Paid ${amount} with UPI`); }
}

class PaymentContext {
  setStrategy(strategy){ this.strategy = strategy; }
  pay(amount){ this.strategy.pay(amount); }
}

// Usage
const ctx = new PaymentContext();
ctx.setStrategy(new UpiPayment());
ctx.pay(100); // Paid 100 with UPI
```

✅ Pros: Easy to switch/extend behaviors.
❌ Cons: More boilerplate (classes/objects).

📌 Real-world: Sorting algorithms, payment gateways, authentication flows.

---

### Visualization

```
Observer → one-to-many notification
Decorator → wrap & extend behavior
Strategy → interchangeable algorithms
```

🧠 **Mental Model:**

* Observer = “Event system.”
* Decorator = “Wrapper.”
* Strategy = “Pluggable algorithms.”

---

### Gotchas ⚠️ (Exhaustive)

1. **Observer**: Risk of memory leaks if subscribers not unsubscribed.
2. **Decorator**: Hard to debug stack if many layers wrap same function.
3. **Strategy**: Boilerplate overhead if too many strategies.
4. Misuse → Overengineering (don’t apply patterns unnecessarily).

---

### Best Practices

* ✅ Use Observer for events/data streams.
* ✅ Use Decorator for cross-cutting concerns (logging, caching).
* ✅ Use Strategy for algorithm/config-driven contexts.
* ✅ Keep implementations simple → patterns should solve, not complicate.

---

👉 Mnemonic:
**“Observer = Watch, Decorator = Wrap, Strategy = Swap.”**

---

⚡ Would you like me to expand this into a **Full Design Patterns Cheat Sheet** (Factory, Singleton, Proxy, Command, Adapter, Composite, etc.) — so you have them all in one place for interviews?
