# 🔧 JavaScript Functions & Scope

- [🔧 JavaScript Functions & Scope](#-javascript-functions--scope)
- [🆚 Function Declarations vs Function Expressions](#-function-declarations-vs-function-expressions)
- [🎛️ Default & Rest Parameters](#-default--rest-parameters)
- [🏹 JavaScript Arrow Functions](#-javascript-arrow-functions)
- [🏗️ JavaScript First-Class & Higher-Order Functions](#-javascript-first-class--higher-order-functions)
- [📍 JavaScript Function Scope vs Block Scope](#-javascript-function-scope-vs-block-scope)
- [🔒 JavaScript Closures](#-javascript-closures)
- [🔁 JavaScript Recursion](#-javascript-recursion)
- [⚡ JavaScript IIFE](#-javascript-iife)
- [🔄 JavaScript Callback Functions](#-javascript-callback-functions)
- [📦 JavaScript `arguments` Object](#-javascript-arguments-object)
- [🔚 JavaScript Tail Call Optimization](#-javascript-tail-call-optimization)

---

## Concepts:

---

### Scope

- **Global Scope** → accessible everywhere (window/globalThis).
- **Function Scope** → accessible inside function, not outside.
- **Block Scope** → `let`/`const` (since ES6) accessible inside block, not outside.
- **Lexical Scope** → accessible inside inner functions, not outside.
- **Module Scope** → each ES module has its own top-level scope accessible inside module.

- Examples:

  ```js
  // Function Scope
  function test() {
    var x = 1;
    if (true) {
      var x = 2;
    }
    console.log(x); // 2
  }
  console.log(x); // ❌ ReferenceError

  // Block Scope
  {
    let y = 10;
    const z = 20;
    console.log(y, z); // 10 20
  }
  console.log(y); // ❌ ReferenceError

  // Block Scope with TDZ
  {
    console.log(a); // ❌ ReferenceError
    let a = 5;
  }
  ```

#### alert⚠️

- `var` → attaches to `window/globalThis`.
- `let`/`const` → do **not** attach.
- Sloppy mode: function declarations inside `{}` hoisted to enclosing function/global. But in ES6 strict mode, it is block-scoped.
- `var` ignores block scope (`if`, `for`).
  ```js
  if (true) {
    var x = 1;
  }
  console.log(x); // 1
  ```
- Inner `let/const` can shadow outer vars and can confuse scope resolution.
- Closures inside loops with `var`:
  ```js
  for (var i = 0; i < 3; i++) setTimeout(() => console.log(i)); // 3,3,3
  ```
- TDZ surprises
  - Access before `let/const` → ReferenceError.
  - Even `typeof` fails:
    ```js
    console.log(typeof a); // ❌
    let a;
    ```

---

### Functions

- Functions encapsulate logic, define scope, and control variable visibility.

```js
/* --> 1.Function Declaration <--
 *Hoisted (usable before definition).
 */
function foo(a, b) {}

/* --> 2.Function Expression <--
 *Not hoisted (usable only after assignment).
 *Assigned to variable/property.
 *Real-World Usage:
 *  - Event listeners: button.addEventListener("click", () => { ... }).
 *  - Promises / async flows: .then(value => { ... }).
 *  - Higher-order functions: arr.map(x => x \* 2).
 *  - Callbacks: Passing inline logic without polluting global/module scope.
 *Provide flexibility and scoping control — used where functions are values, passed around, or need closure context.
 */

/* --> 2.1 Function Expression (Named)  <--
 *Name is only visible inside function body.
 *Name shows up in stack trace.
 *Named function expressions useful for "self-referencing recursion".
 */
const bar = function mul(a, b) {};

/* --> 2.2 Function Expression (Anonymous)  <--
 */
const bar = function (a, b) {};

/* --> 2.2.1 Function Expression (Arrow Function) <--
 *No own `this` → inherits from enclosing scope (lexical), no arguments, no prototype, no super, no new.target.
 *Not constructible (`new (()=>{})` → ❌ TypeError).
 *Shorter syntax for inline callbacks.
 *Arrow functions are always anonymous (name derived from variable).
   - Stack traces may be less clear than named function expressions.

 */
const baz = (a, b) => ({ a: 1, b: 2 });

// Constructor -> always expression, never hoisted, ignores closures (global scope only).
const obj = new Function("a", "b", "return a+b");

// IIFE (Immediately Invoked)
(function () {})();
```

##### Key Differences

| Aspect             | Declaration       | Expression                                         |
| ------------------ | ----------------- | -------------------------------------------------- |
| **Hoisting**       | Fully hoisted     | Only variable hoisted (function not initialized)   |
| **Naming**         | Always named      | Can be anonymous or named                          |
| **When available** | Anywhere in scope | Only after assignment                              |
| **Use case**       | Define main logic | Inline callbacks, closures, functional programming |

#### Function constructor

- Always expression, never hoisted, ignores closures (global scope only).
- Creates functions from strings.
- Always in global scope; ignores closures.
- Security + performance issues.

#### Default parameters

- Evaluated at call time, left-to-right.
- Using earlier param:
  ```js
  function f(a, b = a + 1) {}
  ```
- `function f(a=a){}` → ReferenceError (TDZ).

#### Arguments object

- Not an array; array-like
- Arguments looks like an array, but it is not a real array.
- It doesn’t have real array methods like .map(), .filter(), etc.
- We need to convert it to a real array using `[...arguments]` or `Array.from(arguments)` to use array methods.

  ```js
  function demo(a, b, c) {
    console.log(arguments.length); // 3
    console.log(arguments[0]); // first arg

    let arr = Array.from(arguments); // Convert to array
    console.log(arr.map((x) => x * 2)); // [2, 4, 6]
  }
  ```

- Doesn’t track parameter changes in strict mode.

  ```js
  function f(x) {
    console.log(x, arguments[0]); // 10 10
    x = 20;
    console.log(x, arguments[0]); // 20 20
  }
  f(10);

  function f(x) {
    "use strict";
    console.log(x, arguments[0]); // 10 10
    x = 20;
    console.log(x, arguments[0]); // 20 10 ❌ not synced
  }
  f(10);
  ```

- Arrow functions have no `arguments` array. (they inherit `arguments` from outer scope).
  ```js
  const f = (a) => console.log(a, arguments[0]); // ❌ ReferenceError
  ```

#### alert⚠️

- Multiple declarations in same scope → last one wins.
  - In strict ES6 modules, redeclaring in same scope throws.
- Expressions with `let`/`const` in TDZ until declared
  ```js
  foo(); // ReferenceError
  let foo = function () {};
  ```
- Arrow Functions
  - Arrow inherits `this` from surrounding scope at time of creation.
  - Great for callbacks:
    ```js
    class Timer {
      constructor() {
        this.s = 0;
        setInterval(() => this.s++, 1000);
      }
    }
    ```
  - But can cause bugs if you expect dynamic `this`:
    ```js
    const obj = { val: 42, fn: () => this.val };
    obj.fn(); // undefined (this is not obj)
    ```
  - Inside arrow, `arguments` is from outer function (if any).
- `return` ends function immediately.
- Functions as default export must be named for stack traces.
- `eval` can create vars in enclosing scope (sloppy mode).
- `with` alters scope chain unpredictably (deprecated).

---

### Closure

- Inner function **remembers variables** from outer scope even after outer function exits/closes.

```js
function outer() {
  let x = 0;
  return () => ++x;
}
const inc = outer();
inc(); // 1
inc(); // 2
```

---

### Hoisting

- Functions

  - Function declarations hoisted fully.
  - Function expressions/arrow not hoisted (only variable declaration is).
  - Duplicate function names → last one wins (sloppy mode).

    ```js
    sayHi(); // Declaration works
    function sayHi() {}

    greet(); // Expression ❌ TypeError
    var greet = function () {};
    ```

---

### this

- Function
  - Regular function → `this` depends on caller.
  - Arrow function → inherits `this` from surrounding scope.
  - In strict mode, standalone function call → `this = undefined`.
  - Losing context:
    ```js
    const m = obj.method;
    m(); // `this` lost
    ```

---

### Default & Rest Parameters

#### Default Parameters

- Values assigned **if argument is `undefined`**.
  - Default param works only for `undefined` value.
  - `null`, `0`, `""` -> override defaults -> because intentionally falsy values.
  - `greet(null)` → `"Hi null"` (not default).
  - `greet(undefined)` → `"Hi Guest"` (uses default).
- Evaluated **at call time**, left-to-right
  - `fun(a = 1, b = a + 2) -> b = 3`.
  - `fun(a = a) {} // ❌ ReferenceError (TDZ) Using itself as default`.
- Can use earlier parameters as defaults.
- Defaults create a new scope for each call.
- Defaults can be nested, but ordering still matters.
- Defaults reduce `.length` (count of params before first default).
  - `(function (a,b, c=2 ,d,e) {}).length` -> `3` because of default param `c`.

##### Types:

- Static defaults: `function f(x = 5) {}`.
- Dynamic defaults: `function f(x = Date.now()) {}`.
- Dependent defaults: `function f(a = 1, b = a * 2) {}`.

```js
function greet(name = "Guest", lang = "en") {
  return `Hi ${name}, lang=${lang}`;
}
greet(); // "Hi Guest, lang=en"
greet("Kalidas"); // "Hi Kalidas, lang=en"

// Destructuring with defaults
function f({a=1,b=2} = {}) a + b -> 3
```

#### Rest Parameters

- Collects “the rest” of args into an **array**.
- Must be **last parameter** `function f(...a, b){}` ❌ SyntaxError.
- Only one rest param allowed.
- Replaces legacy `arguments` object in modern JS.
- Types:
  - Pure rest (`function f(...args) {}`).
  - Mixed (`function f(a, b, ...others) {}`).

```js
function sum(...nums) {
  return nums.reduce((a, b) => a + b, 0);
}
sum(1, 2, 3, 4); // 10
```

##### Rest param vs arguments\*\*

- Rest is a real **array**, `arguments` is array-like.
- Arrow functions have no `arguments`, but can use rest.
- `arguments` includes defaulted/skipped params; rest collects extra only.

---

### First-Class & Higher-Order Functions

- All functions are first-class but Not all functions are HOFs coz HOFs are those functions that take other functions as args OR return functions.
- Even constructors are functions (first-class).

#### First-Class Functions

- Functions are treated as values (can be stored, passed, returned).

- Types:

  - Function stored in variable. `const greet = function() {}`
  - Function passed as argument. `setTimeout(() => {}, 1000)`
  - Function returned as value. `function outer() { return () => {}; }`
  - Stored in data structures. `const arr = [(x) => x * 2, (y) => y + 1];`

  - Pass arguments (3 ways)

    ```js
    const sayHi = (name) => console.log(`Hello, ${name}`);
    // ✅ sayHi is called with "Kalidas" after 1000ms
    setTimeout(sayHi, 1000, "Kalidas");
    setTimeout(() => sayHi("Kalidas"), 1000);
    setTimeout(sayHi.bind(null, "Kalidas"), 1000);
    ```

  - function method & `this`

    ```js
    const greeter = {
      name: "Kalidas",
      hi() {
        console.log(this.name);
      },
    };
    setTimeout(greeter.hi, 500); // ❌ undefined (lost `this`)
    setTimeout(() => greeter.hi(), 500); // ✅ arrow keeps receiver
    setTimeout(greeter.hi.bind(greeter), 500); // ✅ bind fixes `this`)
    ```

#### Higher-Order Functions

- Functions that take other functions as args OR return functions.

- Types:

  - Callback-based. `setTimeout(fn, 1000)`
  - Function factories. `makeMultiplier(2) returns new function`
  - Array utilities. `map, filter, reduce` `[1, 2, 3].map((x) => x * 2)`

- Passing a function doesn’t execute it → calling `fn()` executes it.
- Returns function as output. `const add = (x) => (y) => x + y;`
- Both. `const twice = (f) => (x) => f(f(x));`
  ```js
  const inc = (x) => x + 1;
  twice(inc)(5); // -> twice = (inc) => (x) => inc(inc(x)) -> inc(inc(5)) -> inc(6) -> 7
  ```

#### alert⚠️

- Functions are reference types.
- Functions are objects with properties and methods. They can be passed around just like Variables.
- Declared functions are also first-class functions in JavaScript.
  ```js
  function sayHi() {}
  const hi = sayHi; // ✅ function reference is passed
  ```
- HOFs like `forEach` don’t await async callbacks:
  ```js
  [1, 2].forEach(async (x) => await doWork(x)); // not sequential
  ```
- HOFs often rely on closures; careless use can trap memory:
  ```js
  function wrapper() {
    const big = [];
    return () => big;
  }
  ```
  (closure keeps `big` alive).
- Passing a function doesn’t execute it → calling `fn()` executes it.
  - Reference vs Invocation
    ```js
    setTimeout(sayHi(), 1000); // ❌ sayHi() runs now, passes return value immediately without waiting for 1000ms
    setTimeout(sayHi, 1000); // ✅ setTimeout runs later as only reference is passed to it
    ```
- Passing object methods as callbacks loses `this`:
  ```js
  const obj = {
    x: 1,
    fn() {
      return this.x;
    },
  };
  setTimeout(obj.fn, 1000); // undefined
  // fix: `obj.fn.bind(obj)` or arrow wrapper
  ```
- HOFs treat functions as data, enabling **composition** and **abstraction**.
- Anonymous callbacks hinder debugging stack traces. Prefer named expressions.

---

### Callback Hell

- Nesting many HOF callbacks → unreadable. (Solved via Promises/async).

---

### Recursion

- JS engines have stack size limits → stack overflow on deep recursion.
- Returning functions recursively may blow stack (tail-call not widely optimized).

---

## Best Practices:

- Functions:
  - function declarations
    - for hoisting, dynamic `this` needed (value of this is determined at call-time, depending on how the function is invoked);
  - arrow functions
    - for inline callbacks, closures, functional composition, lexical `this` needed (value of this is fixed at definition-time, inherited from the surrounding scope.).
  - named function expressions
    - for better stack traces and recursion.
  - anonymous function expressions
    - for inline callbacks, closures, functional composition, lexical `this` needed (value of this is fixed at definition-time, inherited from the surrounding scope.).
  - Use `const` when assigning functions.
  - Avoid `var` to prevent leakage outside blocks.
  - Use **closures** intentionally for state, not accidentally.
  - Always include braces and return explicitly.
  - Don’t use Function constructor (`new Function`) because it is always expression, never hoisted, ignores closures (global scope only).
  - Keep scope shallow; modularize code.
- Default
  - Use **defaults** instead of `param = param || value` (avoids `0`/`""` bugs).
  - Place required params first, defaults after.
  - Keep defaults simple (avoid heavy computation).
  - Prefer destructured defaults for config objects.
- Rest Parameters:
  - Use **rest** over `arguments` for clarity + real array methods.
- Callbacks:
- ✅ Prefer modern async/await over deeply nested callbacks.

---

## Mnemonics

- “Declarations are Hoisted Leaders; Expressions are Inline Workers.”
- “Default fills gaps, Rest gathers extras.”

- Normal function = “I create my own mini-world with `this` and `arguments`.”
- Arrow = “just pass through my parent’s context.”
- “Arrows are short, inherit lexically `this`, and lack `arguments` & `new`.”

- First-class functions = “functions are variables.”
- Higher-order functions = “functions use other functions.”
- Not all functions are HOFs, but all functions are first-class.

---

---

---

# 🎛️

# 🔒 JavaScript Closures

## **Concept:** A closure is a function that saves references to variables from its **lexical scope** even after the outer function has finished executing.

### Syntax

```js
function outer() {
  let count = 0;
  return function inner() {
    count++;
    return count;
  };
}
const inc = outer();
inc(); // 1
inc(); // 2
```

---

### Core Behavior

- Inner function **captures references** to outer variables, not copies.
- Preserves **lexical scope chain** at creation time.
- Enables **persistent state** across calls.

---

### Common Use-Cases

1. **Data Privacy / Encapsulation**
   ```js
   function counter() {
     let c = 0;
     return { inc: () => ++c, get: () => c };
   }
   const ctr = counter();
   ctr.inc(); // 1
   ctr.get(); // 1 (c is private)
   ```
2. **Memoization / Caching**
   ```js
   function memoize(fn) {
     const cache = {};
     return (arg) => cache[arg] ?? (cache[arg] = fn(arg));
   }
   const square = memoize((x) => x * x); // memoize fun executes and ref cache is passed to square with returned function
   square(4); // (cache[arg] = fn(arg)) -> cache[4] = fn(4) -> ((x) => x * x)(4) -> 16;
   // (function)(value) -> function(value);
   ```
3. **Once-Only Execution**
   ```js
   function once(fn) {
      let called = false,
      let result;
     return () => {
       if (!called) {
         result = fn();
         called = true;
       }
       return result;
     };
   }
   const init = once(() => console.log("run"));
   init(); // run only once
   init(); // (no effect) -> called already true, also result is cached
   ```
4. **Partial Application / Currying**
   ```js
   const add = (a) => (b) => a + b;
   add(2)(3); // 5
   ```
5. **Event Handlers**
   ```js
   for (let i = 0; i < 3; i++) {
     setTimeout(() => console.log(i), 1000);
   }
   ```

---

### Gotchas ⚠️ (Exhaustive)

1. **Reference, not copy**
   - All closures share the same variable, not its value at creation.
   ```js
   var funcs = [];
   for (var i = 0; i < 3; i++) funcs.push(() => i);
   funcs[0](); // 3 (not 0!)
   ```
   (Fix: use `let` or IIFE).
2. **Memory leaks**
   - Long-lived closures can hold large objects in memory.
   - Be careful in event listeners, callbacks, DOM references.
3. **Performance**
   - Excessive closures in hot code paths can increase GC pressure.
4. **Accidental sharing**
   - Nested closures can unintentionally mutate shared state.
5. **Debugging difficulty**
   - Variables may not show expected values due to late binding.
6. **Hoisting + TDZ**
   - Closure captures by reference, so TDZ rules apply for `let/const`.
7. **Eval / with interactions**
   - Can interfere with lexical scoping (sloppy mode).

---

### Best Practices

- ✅ Use closures for **stateful functions** (counters, caches).
- ✅ Prefer `let/const` in loops to avoid classic closure bugs.
- ✅ Release closures holding large objects when not needed.
- ✅ Use named functions when possible for better stack traces.
- ❌ Avoid deep nesting; extract closures into helpers for clarity.

---

## 👉 Mnemonic: **“Closure = Function + Lexical Scope = Private Memory.”**

Would you like me to now create a **dedicated cheat card on Currying & Partial Application** (since they are powerful closure patterns often asked in interviews)?
Here’s your **Concept Mastery Cheat Sheet** for **Recursion** in JavaScript — compact, practical, and with **all gotchas grouped together**.

---

# 🔁 JavaScript Recursion

## **Concept:** A function that calls itself until a base condition is met.

- Solves Problems with nested, hierarchical, or repetitive structure
- Execution rules:
  - Each recursive call adds a stack frame to the call stack.
  - Each recursive call has its own scope & local variables.

### Syntax

```js
// Direct recursion
function fact(n) {
  if (n <= 1) return 1;
  return n * fact(n - 1);
}
// Indirect recursion
function a(x) {
  if (x > 0) b(x - 1);
}
function b(y) {
  if (y > 0) a(y - 1);
}
```

---

### Types

- **Direct** → function calls itself.
- **Indirect/Mutual** → multiple functions call each other.
- **Tail recursion** → recursive call is last statement (can be optimized in some langs, but not reliably in JS).
- **Mutual recursion** → multiple functions call each other cyclically.

---

### Use-Cases

1. **Math problems** → factorial, Fibonacci, GCD.
2. **Tree/Graph traversal** → DOM, JSON, AST, file systems.
3. **Divide & conquer** → quicksort, mergesort, binary search.
4. **Backtracking** → N-Queens, Sudoku, pathfinding.
5. **Flattening structures** → arrays, nested objects.

---

### Examples

```js
// Factorial
const factorial = (n) => (n <= 1 ? 1 : n * factorial(n - 1));
// Sum of nested array
function sum(arr) {
  return arr.reduce((a, v) => a + (Array.isArray(v) ? sum(v) : v), 0);
}
// DOM traversal
function walk(node) {
  console.log(node.tagName);
  node.childNodes.forEach(walk);
}
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Base case missing** → infinite recursion → RangeError: Maximum call stack size exceeded.
2. **Stack overflow** → JS doesn’t guarantee tail-call optimization. Large recursions can crash.
3. **Inefficiency**
   - Naïve Fibonacci: exponential calls.
   - Fix with memoization or iteration.
4. **Indirect recursion complexity** → harder to debug if many functions mutually recurse.
5. **Arguments object** → recursive functions with `arguments.callee` are banned in strict mode.
6. **Named vs anonymous recursion**
   - Anonymous function expressions need a name inside for self-reference:
     ```js
     const f = function fact(n) {
       return n <= 1 ? 1 : n * fact(n - 1);
     };
     ```
   - Without name, can’t self-call.
7. **Async recursion**
   - Recursive `setTimeout` or `async/await` introduces event loop delays; different from sync recursion.
8. **Closures in recursion**
   - Recursive inner functions may capture outer vars and cause leaks.
9. **Debugging stack traces**
   - Anonymous recursion → poor trace readability.
10. **Recursion vs Iteration**
    - Recursion = simpler, elegant, but memory-heavy.
    - Iteration(loops) = efficient, avoids stack overflow.
11. **Tail Call Optimization (TCO)**
    - TCO = optimization technique where the last nested recursive call is the last operation in a function.
    ```js
    function factorial(n, acc = 1) {
      if (n === 0) return acc;
      return factorial(n - 1, n * acc); // tail call (last call to function is the solution) -> no stacking up of call frames
    }
    factorial(5); // factorial(5) calls factorial(4, 5) -> factorial(3, 20) -> factorial(2, 60) -> factorial(1, 120) -> factorial(0, 120) -> return 120
    function factorial(n) {
      if (n === 0) return 1;
      return n * factorial(n - 1); // NOT a tail call (must multiply after return)
    }
    factorial(5); // factorial(5) calls factorial(4) -> factorial(3) -> factorial(2) -> factorial(1) -> factorial(0) -> return 1 and then 5*(4*(3*(2*(1*1)))) = 120
    ```
12. **Security**

- Stack overflow possible with deep recursion.
  ```js
  function recurse() {
    recurse();
  }
  recurse(); // ❌ RangeError: Maximum call stack size exceeded
  ```

---

### Best Practices

- ✅ Always define a clear **base case**.
- ✅ Use **iteration** when recursion depth may be large.
- ✅ Memoize results for heavy recursive problems.
- ✅ Use named functions for better stack traces.
- ✅ For async tasks, use `async/await` recursion carefully.

---

## 👉 Mnemonic: **“Recursion = Base + Self-call, else Stack Fall.”**

---

# ⚡ JavaScript IIFE (Immediately Invoked Function Expressions)

## **Concept:** Function expression that executes immediately after it’s defined.

### Syntax

```js
(function expression)()
(nested function expression)()() // example (fn(){ (fn2(){}) })(f1)(f2);
// Classic
(function () {
  console.log("run");
})();
// Arrow IIFE
(() => console.log("run"))();
// With params
((x, y) => console.log(x + y))(2, 3);
// Named IIFE (for recursion/debugging)
(function fact(n) {
  return n <= 1 ? 1 : n * fact(n - 1);
})(5);
```

---

### Purpose

- Create a **new scope** to avoid polluting global namespace.
- Encapsulate private variables.
- Initialize modules/configs.
- Run once-only code (setup, polyfills).

---

### Variants

- **Unary operator trick** (forces expression):

```js
!(function () {
  console.log("hi");
})();
+(function () {
  console.log("hi");
})();
```

- **Async IIFE** (ES2017+):

```js
(async () => {
  const res = await fetch("/data");
  console.log(await res.json());
})();
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Declaration vs Expression**
   - `function foo(){}()` ❌ SyntaxError (declaration not callable directly).
   - Wrap in `()` to turn into expression: `(function foo(){})()`.
2. **Arrow IIFE & this**
   - Arrow IIFE doesn’t bind its own `this`; inherits lexical `this`.
   - May differ from classic IIFE behavior.
3. **Semicolon safety**
   - If previous line doesn’t end with `;`, IIFE may be misparsed.
   ```js
   let a = 5(function () {})(); // ❌ may break
   ```
   → Always end prev line with `;`.
4. **Debugging**
   - Anonymous IIFEs give poor stack traces. Prefer named IIFEs when debugging.
5. **Overuse**
   - In ES6+, block scope (`{}` + `let`/`const`) or modules often better than IIFEs.
6. **Async pitfalls**
   - Async IIFE returns a promise — forgetting to `await` it can cause race bugs.
7. **Recursion in IIFE**
   - Anonymous IIFE cannot self-call recursively; must be named.

---

### Best Practices

- ✅ Use for one-time setup, initialization, or isolating scope.
- ✅ Prefer **named IIFEs** for recursion & debugging.
- ✅ Always place semicolon before starting an IIFE.
- ❌ Don’t use IIFEs for everything; modern `let/const` + modules reduce need.
- ✅ Use async IIFEs for top-level `await` in non-module scripts.

---

## 👉 Mnemonic: **“IIFE = ( Function Expression ) () → Runs instantly.”**

Do you want me to continue with a **dedicated cheat card on Modules (ES6 import/export)** next, since IIFEs were often a pre-ES6 way to simulate modular scope?
Here’s your **Concept Mastery Cheat Sheet** for **Callback Functions** — compact, interview-ready, and with **all gotchas grouped together**.

---

# 🔄 JavaScript Callback Functions

## **Concept:** A function passed as an argument to another function, executed later (synchronously or asynchronously).

- Higher-Order Functions (HOFs) enable callbacks.

### Syntax

```js
// Synchronous callback
function greet(name, cb) {
  console.log("Hello " + name);
  cb();
}
greet("Kalidas", () => console.log("Done"));
// Asynchronous callback
setTimeout(() => console.log("Runs later"), 1000);
```

---

### Use-Cases

- Event handling (`button.addEventListener("click", cb)`)
- Array methods (`map`, `filter`, `reduce`, `forEach`)
- Async operations (`fs.readFile("file.txt", cb)`)
- Custom control flow (middleware, pipelines)

---

### Types

- **Synchronous** → executed immediately (`map`, `filter`).
- **Asynchronous** → executed later (I/O, timers, network).

---

### Gotchas ⚠️ (Exhaustive)

1. **Callback hell (Pyramid of Doom)**
   - Deep nesting of callbacks → unreadable, hard to maintain.
   - Fixed by Promises/async–await.
2. **Error handling inconsistency**
   - Node.js style uses `(err, result)` convention.
   - Forgetting to handle errors leads to silent failures.
3. **Multiple calls**
   - Callback may be invoked more than once if not guarded.
   - Can cause bugs like duplicate requests.
4. **Lost `this` context**
   ```js
   obj.method(function () {
     console.log(this);
   }); // not obj
   ```
   (Fix: use arrow function or `bind`).
5. **Order of execution confusion**
   - Async callbacks run later in event loop → not in sequence.
   - Example:
     ```js
     setTimeout(() => console.log("A"), 0);
     console.log("B"); // B before A
     ```
6. **Arguments mismatch**
   - Some APIs pass multiple args (e.g., `map((el, i, arr)=>...)`).
   - Forgetting extra args can cause subtle bugs.
7. **Memory leaks**
   - Callbacks registered but never removed (event listeners) keep objects alive.
8. **Zalgo problem**
   - Callback may run sync _or_ async depending on conditions → unpredictable flow.
     (Solved via Promises).
9. **Anonymous callbacks**
   - Harder to debug (stack traces show `<anonymous>`).
10. **Race conditions**
    - Multiple async callbacks updating same state cause inconsistent results.
11. **Surprising quirks**
    - Callback hell is not caused by callbacks themselves, but poor structuring.
    - Even Promises use callbacks under the hood.

---

### Best Practices

- ✅ Use **named functions** for clarity and debugging.
- ✅ Always handle errors in async callbacks.
- ✅ Avoid deep nesting → refactor into Promises/async-await.
- ✅ Remove event listeners after use to prevent leaks.
- ✅ Document the expected signature `(args…)` of callbacks.
- ❌ Don’t rely on execution order in async callbacks.

---

## 👉 Mnemonic: **“Callback = Function Later, Beware Hell & Context.”**

Would you like me to follow this with a **dedicated cheat card on Promises** (since they’re the modern evolution of callbacks and interviewers love comparing them)?
Here’s your **Concept Mastery Cheat Sheet** for **the `arguments` object** — compact, full coverage, and with **all gotchas grouped together**.

---

# 📦 JavaScript arguments Object

## **Concept:** Array-like object available inside **non-arrow functions** that holds all passed arguments.

### Syntax

```js
function demo(a, b) {
  console.log(arguments[0]); // first arg
  console.log(arguments.length); // count
}
demo(10, 20, 30); // 10, 3
```

---

### Properties

- **`arguments.length`** → number of arguments passed.
- **`arguments[i]`** → access by index.
- **`arguments.callee`** → reference to current function (❌ forbidden in strict mode).
- **Not an array** → no `map`, `filter` directly. Convert with `[...arguments]` or `Array.from(arguments)`.

---

### Relationship with Parameters

- **Sloppy mode**: arguments & named params are linked (changes reflect both).
- **Strict mode**: arguments & params are decoupled.

```js
function f(a) {
  a = 99;
  console.log(arguments[0]);
}
f(1); // sloppy: 99, strict: 1
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Not available in arrow functions**
   ```js
   const f = () => console.log(arguments); // ❌ ReferenceError
   ```
2. **Not a real array**
   - Missing array methods. Must convert for iteration.
3. **Strict vs sloppy mode**
   - In sloppy mode, params ↔ arguments linked.
   - In strict mode, they are independent.
4. **Default/rest params interaction**
   - Defaults/rest **don’t update** `arguments`.
   ```js
   function f(a = 5) {
     console.log(arguments.length);
   }
   f(); // 0, not 1
   ```
5. **Extra arguments**
   - Ignored in param list but present in `arguments`.
   ```js
   function g(a, b) {
     console.log(arguments[2]);
   }
   g(1, 2, 3); // 3
   ```
6. **Fewer arguments**
   - Missing ones are `undefined`.
7. **`arguments.callee`**
   - Deprecated; forbidden in strict mode.
   - Alternative: use named function expressions.
8. **Performance**
   - Historically slower in some engines due to aliasing. Modern engines optimized, but rest params preferred.
9. **Shadowing**
   - Declaring a variable/param named `arguments` shadows the object.
10. **ES6 preference**
    - Modern JS favors **rest parameters (`...args`)** over `arguments`.

---

### Best Practices

- ✅ Use **rest params (`...args`)** for modern code.
- ✅ Convert `arguments` with `[...arguments]` or `Array.from`.
- ❌ Don’t use `arguments.callee`.
- ✅ Be aware of strict vs sloppy differences.
- ✅ Use named functions instead of relying on `callee`.

---

## 👉 Mnemonic: **“Arguments = Array-like past, Rest = modern future.”**

Do you want me to next build a **cheat card on Rest Parameters vs `arguments`** (direct comparison, since interviewers often ask why rest is better)?
Here’s your **Concept Mastery Cheat Sheet** for **Tail Call Optimization (TCO)** in JavaScript — focusing on the ES6 spec, reality, and all gotchas grouped together.

---

# 🔚 JavaScript Tail Call Optimization

## **Concept:** An optimization where **a function call in tail position** (last action before return) reuses the current stack frame instead of creating a new one → avoids stack overflow.

- TCO turns recursion into loop-like execution by not adding new stack frames.

### Syntax (Tail Position)

```js
"use strict"; // required per spec
function fact(n, acc = 1) {
  if (n <= 1) return acc; // base case
  return fact(n - 1, n * acc); // tail call (last action)
}
fact(5); // 120
```

---

### ES6 Spec Notes

- Defined in **ECMAScript 2015 (ES6)**.
- Tail call must be:
  1. In **strict mode**.
  2. A direct call (not inside `try/finally`, `with`, etc).
  3. Last action of the function (`return f(...)`).
  4. Not wrapped in an operation (`return 1 + f(x)` ❌).
- Replaces current frame instead of adding new one.

---

### Benefits

- Prevents stack overflow on deep recursion.
- Improves memory efficiency.
- Enables functional-style recursion for large inputs.

---

### Gotchas ⚠️ (Exhaustive)

1. **Not widely implemented**
   - Safari (WebKit) implemented **proper TCO**.
   - V8 (Chrome/Node) & SpiderMonkey (Firefox) **do not support** TCO.
   - Means spec compliance ≠ engine compliance.
2. **Strict mode required**
   - Without `"use strict"`, spec forbids TCO.
3. **Only proper tail calls**
   - Must be exactly `return fn(...)`.
   - Wrapping or adding ops prevents optimization.
   ```js
   return 1 + fn(x); // ❌ not tail position
   return fn(x) * 2; // ❌
   ```
4. **Debugging trade-off**
   - Stack traces shorter, harder to debug if TCO applied.
5. **Async recursion unaffected**
   - TCO applies only to synchronous calls.
   - `await fn()` always creates new promise/frame.
6. **Spec vs real life**
   - ES6 spec mandates TCO, but most engines skipped for performance & debuggability reasons.
   - So writing code expecting TCO is unsafe today.
7. Adding logging after tail call breaks optimization.
8. **Manual trampolining often needed**
   - Simulate TCO using loops or “trampoline” functions.

---

### Workarounds (Without Native TCO)

```js
// Trampoline technique
function trampoline(fn) {
  while (typeof fn === "function") {
    fn = fn();
  }
  return fn;
}
function fact(n, acc = 1) {
  return n <= 1 ? acc : () => fact(n - 1, acc * n);
}
trampoline(fact(100000)); // works without overflow
```

---

### Best Practices

- ✅ Don’t rely on spec TCO in production → assume not supported.
- ✅ Use iteration for very deep recursion in JS.
- ✅ Use trampolines for safe recursive patterns.
- ✅ Use memoization/loops for performance-critical recursive tasks.

---

## 👉 Mnemonic: **“Tail Call = Return Call Only, but Engines Rarely Honor It.”**

Do you want me to follow this with a **dedicated cheat card on Recursion Alternatives** (loops, trampolines, generators) — since interviewers often ask “what if JS doesn’t optimize tail calls”?

```

```
