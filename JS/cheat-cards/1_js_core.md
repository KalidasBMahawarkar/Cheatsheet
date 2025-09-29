# 📜 JS Cheat Card

---

## Concepts:

---

### **ECMAScript History**

#### Origins

- **1995**: _Brendan Eich_ creates JS in 10 days @ Netscape (Mocha → LiveScript → JavaScript).
- **1996**: Microsoft clones as _JScript_ in IE.
- **1997**: ECMA adopts, defines **ECMAScript (ES)** standard.

#### Major ES Versions

- **ES1 (1997):** First spec.
- **ES2 (1998):** Align w/ ISO/IEC 16262.
- **ES3 (1999):** RegExp, try/catch, `do-while`. Widely adopted.
- **ES4 (abandoned, 2008):** Too ambitious (classes, modules, types).
- **ES5 (2009):** `"use strict"`, JSON, `Object.defineProperty`, getters/setters.
- **ES6 / ES2015:** Big bang → `let/const`, arrow fn, classes, modules (`import/export`), Promises, template literals, spread/rest, `Map/Set`, `Symbol`.
- **ES2016:** `Array.includes`, `**` operator.
- **ES2017:** `async/await`, Object.values/entries.
- **ES2018:** Rest/spread in objs, async iterators, RegExp goodies.
- **ES2019:** `Array.flat/flatMap`, `Object.fromEntries`, `trimStart/End`.
- **ES2020:** Optional chaining `?.`, nullish `??`, `BigInt`, `Promise.allSettled`, `globalThis`.
- **ES2021:** `String.replaceAll`, Logical assignment (`||=`, `&&=`), WeakRefs.
- **ES2022:** Top-level `await`, class fields, private `#` methods, `.at()`.
- **ES2023:** Array `findLast`, Hashbang `#!`.
- **ES2024:** `Promise.withResolvers`, `Atomics.waitAsync`, iterator helpers.

---

### **Syntax**

- Rules & building blocks of JS programs

#### Syntax Rules

- **Case-sensitive**: `var foo` ≠ `var Foo`
- **Semicolons**: optional (ASI = Automatic Semicolon Insertion), but safer to include
- **Identifiers**: start with letter, `_`, `$`; can’t start with digit; Unicode allowed
- **Comments**: `//` (single-line), `/* */` (multi-line)
- **Blocks**: `{ ... }` → scope boundary
- **Whitespace**: ignored except inside strings/regex

#### Statements vs Expressions

- **Declarations**: `var`, `let`, `const`, `function`, `class`, `import`, `export`
- **Statement** → does something.
  - Empty statement: `;` (no-op)
  - Labelled statement: `label: statement` (rare)
- **Expression** → produces value. (assignments, calls, operators, literals)
- **Control Flow**:
  - Conditionals: `if`, `else`, `switch`
  - Loops: `for`, `while`, `do…while`, `for…in`, `for…of`
  - Exception handling: `try…catch…finally`, `throw`
- **Jumps**: `break`, `continue`, `return`

#### alert⚠️

- Expressions split across lines misparsed due to ASI (e.g., `a = b + c \n (d+e)`).
- Always use `;` → avoid ASI traps.
- `++`/`--` can break without semicolon.
- `return` and then a value on a newline → returns `undefined` instead of code on newline.
- Syntax errors show up at parse time.
- Empty statement traps:
  ```js
  while (cond); // runs once only -> semicolon `;` immediately after while (cond) is treated as an empty statement.
  {
    doStuff(); // Loop’s body becomes block statement so after while the code sequentially runs once, not looped
  }
  ```

---

### **Comments**

- Inline annotations that do not affect execution
- Use comments to explain why or how, not what.
  ```js
  // WHY: API returns prices in cents, but UI needs dollars.
  function formatPriceToDollars(cents) {
    return (cents * 0.01).toFixed(2);
  }
  ```

#### Syntax

```js
// single-line comment : quick notes, disable one line
/* 
  multi-line comment : block notes, temporary code removal
  */
/**
 * JSDoc style comment : structured docs for functions, classes, APIs (`@param`, `@returns`)
 * Validates email format
 * @param {string} email
 * @returns {boolean}
 */
function validateEmail(email) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}
/*! 
  …preserved by some minifiers (important for legal headers)… 
  */
```

#### alert⚠️

1. **Nested block comments** ❌ not allowed
   ```js
   /* outer /* inner */ still outer */
   // SyntaxError
   ```
2. **Inside strings/regex**
   - `"// not a comment"` → treated as string
   - `/\/\//` → regex for `//`
3. **Conditional compilation (IE legacy)**
   - `/*@cc_on ... @*/` only worked in old JScript. Deprecated.
4. **Source maps / build tools**
   - `//# sourceMappingURL=...` must be last line, else breaks tools.
5. **Minification stripping**
   - Build tools often strip comments → don’t store metadata there unless JSDoc.
6. **Performance myth**
   - Comments don’t slow execution, but excessive ones bloat file size (important in legacy non-gzip serving).
7. **License comments**
   - `/*! … */` preserved by some minifiers (important for legal headers).
8. **Comments inside JSON not allowed**

---

### **Variables**

- Named storage for values in memory.

#### Types

```js
var name = value; // function/global scoped -> hoisted, re-declarable, re-assignable.
let name = value; // block scoped -> hoisted(uninitialized -> TDZ), re-assignable.
const name = value; // block scoped -> hoisted(uninitialized -> TDZ), immutable(but value can be mutable object), must initialize immediately.
```

#### TDZ (Temporal Dead Zone)

- Accessing `let/const` before declaration → ReferenceError.
- Even `typeof` fails (`typeof x; let x=5;` ❌).

#### Dynamic typing

- variable’s type is determined at runtime (when the code runs), not at compile time.
- Use TypeScript / JSDoc if you need static typing.

#### Behavior

- **Hoisting**:
  - `var` → hoists + sets `undefined`.
  - `let/const` → hoists but in TDZ until declaration.
    ```js
    console.log(a); // undefined
    var a = 5;
    console.log(b); // ReferenceError (TDZ)
    let b = 5;
    ```
- **Scope**:
  - `var` ignores `{}`, respects function/global.
  - `let/const` → respect block `{}`.
    ```js
    if (true) {
      var x = 1;
    }
    console.log(x); // 1
    if (true) {
      let y = 1;
    }
    console.log(y); // ReferenceError
    ```
- **Re-declaration**:
  - `var` allows in same scope.
  - `let/const` throw error.
    - `var a=1; var a=2;` → ✅ allowed.
    - `let a=1; let a=2;` → ❌ SyntaxError.
    - `let` + `var` conflict → ❌ SyntaxError in same scope.
- **Assignment**:
  - `let` can be updated.
  - `const` cannot be reassigned, but objects/arrays inside can be mutated.
    ```js
    const arr = [1];
    arr.push(2); // ✅ allowed
    arr = [2]; // ❌ TypeError
    ```
- **CaseSensitive**:
  - `var foo` ≠ `var Foo`
  - `let foo` ≠ `let Foo`
  - `const foo` ≠ `const Foo`
- **Multiple declarations**:
  - `let x = 1, y = 2;`

#### alert⚠️

- Global `var` adds property to `window`.
  ```js
  var x = 10; // global object binding (if declared at top level)
  x = 7; // ❌ implicitly becomes global (bad practice)
  ("use strict");
  x = 7; // ReferenceError (cannot create implicit global)
  let y = 10; // do not add property to global object.
  ```
- `var` in `for` loop shares one variable across iterations.
- `let` creates new binding per iteration.
  ```js
  for (var i = 0; i < 3; i++) setTimeout(() => console.log(i)); // 3,3,3
  for (let i = 0; i < 3; i++) setTimeout(() => console.log(i)); // 0,1,2
  ```
- Destructuring const must initialize immediately.
  ```js
  const { a } = obj; // must initialize immediately
  ```
- Eval/with quirks
  - In sloppy mode, `eval("var a=5")` creates function/global var.
  - `with` can shadow existing vars unpredictably (deprecated).

---

### Data Types

- Categories of values that JS can handle.

#### Primitives (immutable, copied by value)

- **string** → `"hello"`
- **number (64-bit floats)**
  - `42`, `3.14`, `NaN`, `Infinity`
  - `0.1 + 0.2 !== 0.3` (floating precision).
  - Safe integers only up to `2^53 - 1` (`Number.MAX_SAFE_INTEGER`).
- **boolean** → `true`, `false`
- **null** → intentional empty value
- **undefined** → uninitialized / missing value
- **symbol**
  - unique identifiers (`Symbol("id")`)
  - Unique always: `Symbol("a") !== Symbol("a")`.
  - Use `Symbol.for("a")` for global reuse.
- **bigint**
  - arbitrary precision integers (`123n`)
  - Not interchangeable with `number` `(10n + 5)` ❌ TypeError. Must use same type.

##### alert⚠️

- `"hi".length` works bcoz JS wraps string in `String` object temporarily.

#### Non-Primitives (mutable, copied by reference)

- **Object** → `{ key: "value" }`
- **Array** → `[1,2,3]`
- **Function** → `function(){}`
- **Date, RegExp, Map, Set, WeakMap, WeakSet**
- **Typed Arrays, ArrayBuffer, DataView**
- **Error types** → `Error`, `TypeError`, `RangeError`, etc.

#### Type Checking quirks

```js
typeof null; // "object"  ⚠️ bug
typeof []; // "object"
Array.isArray([]); // true
typeof NaN; // "number"
typeof Symbol(); // "symbol"
typeof function () {}; // "function"
typeof undeclaredVar; // "undefined"
```

---

### Equality

- **Loose equality (`==`)**: allows coercion.
- **Strict equality (`===`)**: no coercion.

  ```js
  0 == false; // true
  0 === false; // false
  "" == false; // true
  0 == "0"; // true.
  0 == -0 or 0 === -0; // both are true cuz js treats +0 and -0 as equal in equality.
  Object.is(0,-0); // false cuz js treats +0 and -0 as distinct in object.is.

  NaN == NaN // false
  NaN !== NaN; // NAN is the only value NaN not equal to itself use `Number.isNaN`
  null == 0 // false
  undefined == null; // true (loose).
  undefined === null; // false (strict).
  {} === {} // false -> comparing objects compares reference, not contents.
  ```

---

### Coercion

#### Explicit (Casting)

→ We convert manually.

```js
String(123); // "123"
Number("42"); // 42
Boolean(0); // false
parseInt("08"); // 8
parseFloat("3.14"); // 3.14
BigInt("123"); // 123n
obj.toString(); // "[object Object]"
JSON.stringify({ a: 1 }); // '{"a":1}'
```

##### PraseInt() vs Number()

- Number()
  - Converts the entire string to a number.
  - If the string contains any non-numeric characters (besides whitespace), the result is NaN.
  - Works with floats, scientific notation, hex/binary literals, etc.
    ```js
    Number("42"); // 42
    Number("3.14"); // 3.14
    Number("42abc"); // NaN ❌
    Number(" 42 "); // 42 (trims whitespace)
    Number("0xF"); // 15 (hex ok)
    Number("08"); // 8
    isNaN("string"); // true
    Number.isNaN("string"); // false.
    ```
- ParseInt()
  - Parses up to the first invalid character and ignores the rest.
  - Always returns an integer (truncates decimals).
  - Takes a second argument = radix (base).
    ```js
    parseInt("42"); // 42
    parseInt("3.14"); // 3 (stops at ".")
    parseInt("42abc"); // 42 ✅
    parseInt(" 42 "); // 42 (ignores whitespace)
    parseInt("08"); // 8
    parseInt("0xF"); // 15
    parseInt("101", 2); // 5 (binary parsing)
    ```

#### Implicit (Coercion)

→ JS converts automatically.

- **String coercion** → `1 + "2"` → `"12"`
- **Numeric coercion** → `"5" * 2` → `10`
- **Boolean coercion** (in conditions):
  - Falsy: `0, -0, 0n, "", null, undefined, NaN, false`
  - Truthy: everything else (incl. `"0"`, `"false"`, `[]`, `{}`)

##### Coercion Types

###### Operator Hints

- Different operators ask for different “hints”:
  - String hint: Concatenation (+ with a string), template literals.
  - Number hint: Arithmetic (-, \*, <, == comparison).
  - No hint: == null / == undefined (special rule).

###### Conversion Order

- Primitives:
  - Boolean → Number (true → 1, false → 0)
  - String → Number ("123" → 123, "" → 0, "abc" → NaN)
  - Number → String (123 → "123")
- Object/Array → Primitive (via `.valueOf()` / `.toString()`)
  - Arrays use `toString()` first : `[]` → `""`
  - objects use `valueOf()` then `toString()` : `{}` → `"[object Object]"

##### Type coercion traps\*\*

- When using + with non-numbers, JS converts to Primitive
- `[] + [] → ""` ([].toString() → "", "" + "" -> "")
- `[] + {} → "[object Object]"` ([].toString() → "", {}.toString() → "[object Object]" => "[object Object]")
- `{} + [] → 0` (parsed as block + `+[]`).
  ```js
  {
  } // empty block
  +[]; // [] → "", +"" → 0
  ```
- `[] == ![]` → true (because `[] → "" → 0`, `![] → false → 0` => `0 == 0`).
- `{}` -> true.
- `[]` -> true.
- so `{} == false` → false (empty object coerces to `"[object Object]" → NAN == false  → false`).
- but `[] == false` → true (empty array coerces to `"" → 0 == false  → true`).
-     - `null + 1` → `1`.
  - `undefined + 1` → NaN.
    - `JSON.stringify(undefined)` → undefined (not `"undefined"`).
    - `JSON.stringify(NaN)` → `"null"`.

---

### Operators

- Symbols that perform operations on values.

#### Categories

1. **Arithmetic** : `+  -  *  /  %  **  ++  --`
2. **Assignment** : `=  +=  -=  *=  /=  %=  **=  <<=  >>=  >>>=  &=  ^=  |=  &&=  ||=  ??=`
3. **Comparison** : `==  !=  ===  !==  >  <  >=  <=`
4. **Logical** : `&&  ||  !`
5. **Bitwise** : `&  |  ^  ~  <<  >>  >>>`
6. **Ternary (Conditional)** : `condition ? exprIfTrue : exprIfFalse`
7. **Spread / Rest**
   - `...arr` (spread values)
   - `function f(...args)` (Rest, collect args)
8. **typeof / instanceof / in / delete / void**

- `typeof value` → string type
- `value instanceof Constructor`
- `"prop" in obj`
- `delete obj.prop`
- `void expr` → `undefined`

9. **Comma**
   `(a=1, b=2, a+b)` → evaluates left to right → returns last.

##### Examples

```js
    5 + "5"       // "55"
    5 == "5"      // true
    5 === "5"     // false
    null ?? "x"   // "x"
    a &&= b       // a = a && b
    [...arr1, ...arr2]
    "x" in {x:1}  // true
```

#### alert⚠️

1. **Arithmetic quirks**
   - `0.1 + 0.2 !== 0.3` (precision).
   - `%` is remainder, not true modulo (`-5 % 2 → -1`).
2. **Assignment traps**
   - `a = b = c = 0;` → all assigned to `0`.
   - `||=`, `&&=`, `??=` only assign if condition matches.
3. **Logical operators**
   - Return **last evaluated value**, not always boolean:
   - `let y = "hello" || "hi"` → `"hello"` // || - first truthy value else last value
   - `"ok" && 123` → `123` // && - first falsy value else last value
   - `!!value` → reliable boolean cast.
4. **Bitwise gotchas**
   - Operands converted to **32-bit signed ints**.
   - `>>>` produces unsigned 32-bit int.
   - Large numbers overflow.
5. **instanceof pitfalls**
   - Checks prototype chain, not type string.
   - Cross-frame objects fail:
     ```js
     [] instanceof Array; // false if different realm
     ```
6. **in operator**
   - `"toString" in {}` → true (inherited).
7. **delete quirks**
   - `delete obj.prop` → true if configurable.
   - `delete var,let,const,function` → false.
8. **void**
   - `void 0` → `undefined`.
   - Rarely useful outside bookmarks/minified code.
9. **Comma operator**
   - Confusing when mixed with `for` loops and function args.
10. **Precedence traps**
    - `a + b * c` → `a + (b*c)`
    - `typeof x === "string"` works b/c `typeof` has higher precedence.

---

### Control Structures

- Direct the execution flow of code via branching, looping, or exceptions.

#### Conditionals

      ```js
      if (cond) { ... }
      else if (cond) { ... }
      else { ... }
      switch (expr) {
      case val: ...; break;
      default: ...
      }
      ```

#### Loops

      ```js
      for (let i=0; i<5; i++) { ... }
      while (cond) { ... }
      do { ... } while (cond) // executes at least once, even if condition false.
      for (const val of iterable) { ... } // iterable array
      for (const key in enumerable) { ... } // enumerable object keys
      ```

#### Jumps

      ```js
      break;      // exit loop/switch
      continue;   // skip current loop iteration
      return val; // exit function
      throw err;  // raise exception
      ```

#### Exception Handling

      ```js
      try { ... }
      catch (e) { ... }
      finally { ... }
      ```

#### alert⚠️

- Assignment in condition: `if (x = 5)` → always truthy.
- Switch
  - Missing `break` → fall-through to next case.
  - `switch` uses **strict comparison (`===`)**.
  - Duplicate `case` labels silently ignored.
  - `default` can be anywhere; execution continues unless `break`.
- For loop
  - `var` in loops is function-scoped → closure trap in async:
    `js
for (var i = 0; i < 3; i++) setTimeout(() => console.log(i)); // 3,3,3
for (let i = 0; i < 3; i++) setTimeout(() => console.log(i)); // 0,1,2
`
  - Modifying loop counter inside loop can cause infinite loops.
- `while(true)` without `break` → infinite loop.
- For…in vs For…of
  - `for…in`: iterates enumerable keys (incl. inherited).
  - `for…of`: iterates values of iterable (arrays, strings, sets, maps).
  - Iteration order in `for…in` not guaranteed (esp. objects).
  - Break/Continue
    - Only affect nearest loop/switch, not outer loops.
    - To exit outer loop, need labels (bad practice).
- Try/Catch/Finally
  - `catch(e)` shadows outer `e`.
  - `finally` always runs (even after `return`), and can override return value:
    ```js
    function f() {
      try {
        return 1;
      } finally {
        return 2;
      }
    } // → 2
    ```
  - Exceptions in async functions must be caught with `try/catch` inside `async` or `.catch()`.
- Throw quirks
  - `throw` requires an argument (any value, not just Error).
  - `throw` across async boundaries won’t be caught by sync try/catch.
- Labels
  - Rarely used, can create spaghetti code:
    ```js
    outer: for(...) {
     for(...) { break outer; }
    }
    ```

---

### Strict Mode

- A stricter version of JavaScript introduced in ES5 (2009) to eliminate silent errors, improve security, and allow optimizations.

#### Syntax

```js
"use strict"; // at top of file → whole script strict
(function () {
  "use strict"; // inside function → only this function strict
})();
```

#### Behavior Changes

- Disallows **implicit globals** (must declare variables).
- `this` in functions → `undefined` (not global object).
- `delete` on variables/functions/params → SyntaxError.
- Duplicate parameter names → SyntaxError.
- Assignments to read-only properties → TypeError.
- Assignments to non-writable getters → TypeError.
- Octal literals (e.g., `0123`) → SyntaxError.
- Safer `eval` → variables declared inside `eval` don’t leak out.
- Reserved keywords (`implements`, `interface`, `let`, `package`, `private`, `protected`, `public`, `static`, `yield`) cannot be used as identifiers.

#### alert⚠️

1. Implicit globals banned
   ```js
   x = 10; // ❌ ReferenceError in strict mode
   ```
2. `this` behavior
   - Normal function: `this is undefined` instead of `window/globalThis`.
   - Arrow functions inherit `this` from lexical scope, unaffected.
3. Delete restrictions
   - `delete Object.prototype` → ❌ TypeError.
   - `delete variableName` → ❌ SyntaxError.
4. Duplicate parameter names
   ```js
   function f(a, a) {} // ❌ SyntaxError in strict
   ```
5. Octal literals
   ```js
   let x = 010; // ❌ SyntaxError
   ```
6. Assignments to non-writable
   ```js
   const obj = {};
   Object.defineProperty(obj, "x", { value: 1, writable: false });
   obj.x = 2; // ❌ TypeError
   ```
7. With statement
   - `with(obj){...}` → ❌ not allowed.
8. Eval sandboxing
   - In strict mode, vars declared inside `eval` stay inside it.
   - In sloppy mode, they leak into surrounding scope.
9. Reserved words
   - Using ES future keywords as identifiers (e.g., `let = 1`) → SyntaxError.
10. Arguments object - No dynamic link between parameters & `arguments`.
    ```js
    function f(a) {
      "use strict";
      a = 99;
      console.log(arguments[0]);
    }
    f(1); // 1 in strict, 99 in sloppy
    ```
11. Silent errors become thrown - E.g., assigning to `NaN = 5` throws TypeError.
12. Function declarations in blocks - Strict mode disallows block-scoped function declarations in older engines (now standardized with ES6).

#

#

#

#

---

## Gotchas

- ECMAScript History
  - Browser vendors lag in adoption.
  - Not every ES feature lands in Node/browser at same time → check compatibility.
  - ES versions after 2015 are **yearly** (TC39 process).
- Syntax & Statements

---

## Best Practices

- ECMAScript History
  - Check engine support: [MDN compat tables](https://developer.mozilla.org/).
  - Use `"use strict"` to avoid legacy quirks.
  - Transpile w/ Babel for older envs.
- Syntax & Statements
- Comments
  - Use **single-line** for short notes, **block** for explanations.
  - Prefer **JSDoc** for functions, APIs, libraries.
  - Don’t leave **commented-out code** in production.
  - Use clear, concise English; avoid obvious notes (“increment i by 1”).
  - Keep **legal/license comments** in `/*! … */`.
- Variables
  - Use `const` by default, `let` if reassignment needed, avoid `var`.
  - Always declare at **top of block** for clarity.
  - Never rely on hoisting; declare before use.
  - Avoid global vars; use modules/closures.
  - Use **meaningful names**; follow camelCase convention.
- Coercion
  - Default to **`const` objects/arrays**, avoid mutation when possible.
  - Use `===` instead of `==` (avoids coercion).
  - Use `Number.isNaN()` & `Number.isFinite()`.
  - Use `Array.isArray()` not `typeof` for Arrays.
  - Prefer `BigInt` only if working beyond safe integer range.
  - Use `Object.is()` for edge equality (`NaN`, `-0`).
  - Performance Aspects
  - **Primitives:** Fast (stack).
  - **References:** Slower (heap, GC involved).
  - **Optimizations:**
    - Use correct data type (`Map` faster than object for dynamic keys).
- Operators
  - Prefer `===`/`!==` over `==`/`!=`.
  - Avoid chained assignments (`a=b=c=...`).
  - Use logical operators for short-circuit (||, &&, ??), not as boolean substitutes (!!).
  - Avoid confusing comma/void/delete unless necessary.
  - Use `Number.isNaN`, `Object.is`, and explicit casts (String(), Number(), Boolean()).
  - Limit use of bitwise ops unless needed (>>>).
  - Write readable ternaries; prefer `if/else` for clarity (condition ? exprIfTrue : exprIfFalse).
- Control Structures
  - Always use `{}` with conditionals/loops.
  - Prefer `for…of` or array methods (`map`, `forEach`) and `for…in` for iterating over array with keys and objects.
  - Avoid labels.
  - Use `switch` only when many branches; otherwise prefer `if/else`.
  - Keep loop counters and termination conditions clear.
  - Always return `Error` objects, not strings, with `throw`.
  - Catch only what you can handle; don’t swallow errors silently.
- Strict Mode
  - Always use `"use strict";` in legacy scripts & Write new code assuming strict mode.
  - In ES6+ modules, strict mode is **enabled by default** (no need to declare).

## Mnemonics

- “JavaScript” = language;
- “ECMAScript” = standard.
- “3 started, 5 strict, 6 exploded, yearly drip after.”
- “var = legacy, let = change, const = default.”
- Primitives are 7 (string, number, boolean, null, undefined, symbol, bigint); Everything else = object.
- “Strict = No Globals, No Duplicates, No Octals, Safer Eval, Safer This.”

---
