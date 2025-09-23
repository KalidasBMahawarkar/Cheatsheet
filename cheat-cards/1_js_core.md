Got it. For a **cheat card on “History of JavaScript & ECMAScript”**, we’ll keep it _ultra-compact_, but still cover all milestones.

---

# 📜 JS + ECMAScript History Cheat Card

**Concept:** History & Standards
**Why:** Know version timeline, naming, and key feature drops.

---

### Origins

- **1995**: _Brendan Eich_ creates JS in 10 days @ Netscape (Mocha → LiveScript → JavaScript).
- **1996**: Microsoft clones as _JScript_ in IE.
- **1997**: ECMA adopts, defines **ECMAScript (ES)** standard.

---

### Major ES Versions

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

### Gotchas

- “JavaScript” = language; “ECMAScript” = standard.
- Browser vendors lag in adoption.
- Not every ES feature lands in Node/browser at same time → check compat.
- ES versions after 2015 are **yearly** (TC39 process).

---

### Debug Tips

- Check engine support: [MDN compat tables](https://developer.mozilla.org/).
- Use `"use strict"` to avoid legacy quirks.
- Transpile w/ Babel for older envs.

---

👉 Quick mnemonic:
**“3 started, 5 strict, 6 exploded, yearly drip after.”**

---

# 📝 JavaScript Syntax & Statements — Cheat Card

**Concept:** Rules & building blocks of JS programs

---

### Syntax Rules

- **Case-sensitive**: `var foo` ≠ `var Foo`
- **Semicolons**: optional (ASI = Automatic Semicolon Insertion), but safer to include
- **Identifiers**: start with letter, `_`, `$`; can’t start with digit; Unicode allowed
- **Comments**: `//` (single-line), `/* */` (multi-line)
- **Blocks**: `{ ... }` → scope boundary
- **Whitespace**: ignored except inside strings/regex

---

### Statements

- **Statements vs Expressions:**
  - Statement → does something.
  - Expression → produces value.
- **Declarations**: `var`, `let`, `const`, `function`, `class`, `import`, `export`
- **Expressions**: assignments, calls, operators, literals
- **Control Flow**:

  - Conditionals: `if`, `else`, `switch`
  - Loops: `for`, `while`, `do…while`, `for…in`, `for…of`
  - Exception handling: `try…catch…finally`, `throw`

- **Jumps**: `break`, `continue`, `return`
- **Empty statement**: `;` (no-op)
- **Labelled statement**: `label: statement` (rare)

---

### Gotchas ⚠️ (Exhaustive)

1. **ASI (Automatic Semicolon Insertion)**

   - `return` newline → returns `undefined` instead of code on newline.
   - Expressions split across lines misparsed (e.g., `a = b + c \n (d+e)`).
   - `++`/`--` can break without semicolon.

2. **Hoisting quirks**

   - `var`: hoisted + initialized as `undefined`.
   - `let`/`const`: hoisted but in **TDZ** until declaration.
   - `TDZ`: Temporary Dead Zone is the period of time between the variable declaration and its initialization.
   - `Hoisting`: is the process of moving declarations to the top of their scope during the compilation phase.
   - `function and variables` are hoisted.
   - `Function expressions/arrow functions` **not hoisted** in function expression only variables are hoisted but not the function assigned to variable.

3. **`var` vs `let/const`**

   - `var` ignores block scope.
   - `let/const` are block-scoped.
   - `const` requires initializer.

4. **Block vs object literal**

   - `{}` is dual-purpose — block in statement context, object literal in expression context.

   ```js
   // block+label not object
   {
     a: 1;
   } // block statement with a label a and expression 1,

   // object literal
   let obj = { a: 1 }; // because it’s after =, the parser knows {} must be an expression, so it’s an object
   ({ a: 1 }); // force it to be an object
   ```

5. **Empty statement traps**

   ```js
   while (cond);
   {
     doStuff();
   } // semicolon ; immediately after while (cond) is treated as an empty statement.
   // That means the loop’s body is just… nothingblock so after while the code sequentially runs once, not looped
   ```

6. **Switch fall-through**

   - Missing `break` → executes next cases unintentionally.

7. **`for…in` vs `for…of`**

   - `for…in`: iterates enumerable keys (incl. inherited).
   - `for…of`: iterates values (requires iterable).
   - Order not guaranteed in `for…in`.

8. **Labels**

   - `break`/`continue` can jump to labels → confusing spaghetti code.

9. **Strict mode (`"use strict"`)**

   - Assignment to undeclared variable → ReferenceError.
   - `this` in functions = `undefined` (not global).
   - Silent failures become thrown errors.

10. **Imports/Exports**

    - Must be top-level (not inside functions/blocks). // gives syntax errors -> coz they are not hoisted, compiler needs to know dependencies before execution

    ```js
    if (cond) {
      import { x } from "./lib.js"; // SyntaxError
    }

    if (cond) {
      const { default: devTool } = await import("./dev.js"); // ✅ dynamic import() allows conditional imports and is also hoisted as it is a function expression
    }
    ```

    - Are **static** → names fixed at parse time.

    ```js
    import { value } from "./lib.js";
    value = 5; // ❌ TypeError, read-only binding, but if the content can be changed from the imported file, then it will be allowed, as values referenced not copied
    ```

11. **Catch block quirks**

    - `catch(e)` shadows outer `e`.
    - Errors swallowed if not re-thrown.

12. **Reserved words**

    - Keywords like `class`, `import` can’t be used as identifiers.
    - Some old reserved words (`enum`, `implements`) disallowed in strict mode.

13. **Unicode confusion**

    - Visually identical characters (e.g., Greek `Σ` vs Latin `S`) can slip into identifiers.

14. **Legacy quirks**

    - `with` is deprecated (still parses in sloppy mode).
    - `eval` can inject new variables into scope (sloppy mode).

---

### Best Practices

- Always use `;` → avoid ASI traps.
- Prefer `const` → `let`; avoid `var`.
- Wrap all blocks in `{}` (no single-line shortcuts).
- Use `for…of` or array methods (`map`, `forEach`) over `for…in`.
- Enable `"use strict"` or default to ES modules.
- Avoid labels, `with`, `eval`.

### Scope & Lifetime

- **Validity:**

  - Statements live in function, block, or global scope.

- **Creation moment:**

  - Declared variables are hoisted at parse-time.

- **Destruction:**

  - Block scope → end of block.
  - Function scope → when function returns.
  - GC removes unreachable objects.

- **Syntax errors show up at parse time.**

---

👉 Mnemonic: **“ASI, Hoist, var, Block, Empty, Switch, Loops, Labels, Strict, Imports, Catch, Reserved, Unicode, Legacy.”**

---

# 💬 JavaScript Comments — Cheat Card

**Concept:** Inline annotations that do not affect execution

---

### Syntax

```js
// single-line comment
```

```js
/* 
  multi-line comment 
  */
```

```js
/**
 * Validates email format
 * @param {string} email
 * @returns {boolean}
 */
function validateEmail(email) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}
```

```js
/*! 
  …preserved by some minifiers (important for legal headers)… 
  */
```

---

### Usage

- **Single-line**: quick notes, disable one line
- **Multi-line**: block notes, temporary code removal
- **JSDoc**: structured docs for functions, classes, APIs (`@param`, `@returns`)

- **Use comments to explain why or how, not what.**
  ```js
  // WHY: API returns prices in cents, but UI needs dollars.
  // Multiplying by 0.01 is not obvious at first glance.
  function formatPrice(cents) {
    return (cents * 0.01).toFixed(2);
  }
  ```

---

### Gotchas ⚠️ (Exhaustive)

1. **Nested block comments** ❌ not allowed

   ```js
   /* outer /* inner */ still outer */
   // SyntaxError
   ```

2. **Inside strings/regex**

   - `"// not a comment"` → treated as string
   - `/\/\//` → regex for `//`

3. **Trailing comments**

   ```js
   let a = 5(
     // no semicolon
     function () {}
   )(); // ASI confusion
   ```

4. **Commented code drift**

   - Old/unused code left inside `/* */` can mislead readers.

5. **Conditional compilation (IE legacy)**

   - `/*@cc_on ... @*/` only worked in old JScript. Deprecated.

6. **Source maps / build tools**

   - `//# sourceMappingURL=...` must be last line, else breaks tools.

7. **Minification stripping**

   - Build tools often strip comments → don’t store metadata there unless JSDoc.

8. **Performance myth**

   - Comments don’t slow execution, but excessive ones bloat file size (important in legacy non-gzip serving).

9. **License comments**

   - `/*! … */` preserved by some minifiers (important for legal headers).

10. **Comments inside JSON not allowed**

---

### Best Practices

- Use **single-line** for short notes, **block** for explanations.
- Prefer **JSDoc** for functions, APIs, libraries.
- Don’t leave **commented-out code** in production.
- Use clear, concise English; avoid obvious notes (“increment i by 1”).
- Keep **legal/license comments** in `/*! … */`.

---

### Scope & Lifetime

- **Validity:** Only within source file; stripped before runtime.
- **Creation moment:** Written in source code.
- **Destruction moment:** Removed during parsing/tokenization.

---

👉 Mnemonic: **“Single, Block, Doc — No Nest, No Drift.”**

---

Here’s your **Concept Mastery Cheat Sheet** for **JavaScript Variables**, compact but fully loaded with details + every gotcha grouped in one block.

---

# 📦 JavaScript Variables — Cheat Card

**Concept:** Named storage for values in memory.

---

### Syntax

```js
var name = value; // function/global scoped
let name = value; // block scoped
const name = value; // block scoped, immutable binding
```

---

### Types

- **var** → function/global scope, hoisted, redeclarable.
- **let** → block scope, hoisted but TDZ, re-assignable.
- **const** → block scope, hoisted but TDZ, must initialize, immutable binding (but value can be mutable object).

---

### Behavior

- **Hoisting**:

  - `var` → hoists + sets `undefined`.
  - `let/const` → hoists but in TDZ until declaration.

- **Scope**:

  - `var` ignores `{}`, respects function/global.
  - `let/const` → respect block `{}`.

- **Re-declaration**:

  - `var` allows in same scope.
  - `let/const` throw error.

- **Assignment**:

  - `let` can be updated.
  - `const` cannot be reassigned, but objects/arrays inside can be mutated.

- **CaseSensitive**:

  - `var foo` ≠ `var Foo`
  - `let foo` ≠ `let Foo`
  - `const foo` ≠ `const Foo`

- **Multiple declarations**:
  - `let x = 1, y = 2;`

---

### Gotchas ⚠️ (Exhaustive)

1. **Hoisting quirks**

```js
console.log(a); // undefined
var a = 5;
console.log(b); // ReferenceError (TDZ)
let b = 5;
```

2. **Redeclaration rules**

   - `var a=1; var a=2;` → ✅ allowed.
   - `let a=1; let a=2;` → ❌ SyntaxError.
   - `let` + `var` conflict → ❌ SyntaxError in same scope.

3. **Shadowing**

   - Inner `let/const` shadows outer vars (even `var`).
   - Can lead to confusing bugs.

4. **Const mutability**

   ```js
   const arr = [1];
   arr.push(2); // ✅ allowed
   arr = [2]; // ❌ TypeError
   ```

5. **Global leakage**

   - Assigning undeclared var in sloppy mode → creates global.
   - Global `var` adds property to `window`.

   ```js
   var x = 10; // global (if declared at top level)
   x = 7; // ❌ implicitly becomes global (bad practice)
   ```

   - `"use strict"` prevents this (ReferenceError).

   ```js
   "use strict";
   x = 7; // ReferenceError (cannot create implicit global)
   ```

6. **Block vs function scope**

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

7. **Loop quirks**

   - `var` in `for` loop shares one variable across iterations.
   - `let` creates new binding per iteration.

   ```js
   for (var i = 0; i < 3; i++) setTimeout(() => console.log(i)); // 3,3,3
   for (let i = 0; i < 3; i++) setTimeout(() => console.log(i)); // 0,1,2
   ```

8. **TDZ (Temporal Dead Zone)**

   - Accessing `let/const` before declaration → ReferenceError.
   - Even `typeof` fails (`typeof x; let x=5;` ❌).

9. **Global object binding**

   - `var x=1;` → attaches to `window/globalThis`.
   - `let/const` do not.

10. **Destructuring const**

    ```js
    const { a } = obj; // must initialize immediately
    ```

11. **Eval/with quirks**

    - In sloppy mode, `eval("var a=5")` creates function/global var.
    - `with` can shadow existing vars unpredictably (deprecated).

12. **Re-declaring parameters**

    - Function params can collide with `var`, but not with `let/const`.

13. **Dynamic typing**

- variable’s type is determined at runtime (when the code runs), not at compile time.
- Use TypeScript / JSDoc if you need static typing.

  ```js
  let x = 10; // number
  x = "hello"; // now a string
  x = true; // now a boolean
  ```

---

### Best Practices

- Use **`const` by default**, `let` if reassignment needed, avoid `var`.
- Always declare at **top of block** for clarity.
- Never rely on hoisting; declare before use.
- Avoid global vars; use modules/closures.
- Use **meaningful names**; follow camelCase convention.

---

👉 Mnemonic: **“var = legacy, let = change, const = default.”**

---

Here’s your **Concept Mastery Cheat Sheet** for **JavaScript Data Types**, ultra-compact but complete, with all **gotchas grouped together** so nothing is missed.

---

# 🧩 JavaScript Data Types — Cheat Card

**Concept:** Categories of values that JS can handle.

---

### Primitives (immutable, copied by value)

- **string** → `"hello"`
- **number** → `42`, `3.14`, `NaN`, `Infinity`
- **boolean** → `true`, `false`
- **null** → intentional empty value
- **undefined** → uninitialized / missing value
- **symbol** → unique identifiers (`Symbol("id")`)
- **bigint** → arbitrary precision integers (`123n`)

---

### Reference (mutable, copied by reference)

- **Object** → `{ key: "value" }`
- **Array** → `[1,2,3]`
- **Function** → `function(){}`
- **Date, RegExp, Map, Set, WeakMap, WeakSet**
- **Typed Arrays, ArrayBuffer, DataView**
- **Error types** → `Error`, `TypeError`, `RangeError`, etc.

---

### Type Checking

```js
typeof "hi"; // "string"
typeof null; // "object"  ⚠️ bug
typeof []; // "object"
Array.isArray([]); // true
typeof NaN; // "number"
typeof 123n; // "bigint"
typeof Symbol(); // "symbol"
```

---

### Types

- **Explicit (Casting)** → You convert manually.
- **Implicit (Coercion)** → JS converts automatically.

---

### Explicit Conversion

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

---

### Implicit Conversion

- **String coercion** → `1 + "2"` → `"12"`
- **Numeric coercion** → `"5" * 2` → `10`
- **Boolean coercion** (in conditions):

  - Falsy: `0, -0, 0n, "", null, undefined, NaN, false`
  - Truthy: everything else (incl. `"0"`, `"false"`, `[]`, `{}`)

---

### Equality

- **Loose equality (`==`)**: allows coercion.
- **Strict equality (`===`)**: no coercion.

  ```js
  0 == false; // true
  0 === false; // false
  "" == false; // true
  null == undefined; // true
  ```

---

### PraseInt() vs Number()

#### Number()

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
```

#### parseInt()

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

---

### Gotchas ⚠️ (Exhaustive)

1. **`typeof null` bug** → returns `"object"` (legacy bug).
2. **`NaN`**

   - `typeof NaN === "number"`.
   - `NaN !== NaN` (only value not equal to itself).
   - Use `Number.isNaN()` not `isNaN()`.

3. **Numbers**

   - All are 64-bit floats.
   - `0.1 + 0.2 !== 0.3` (floating precision).
   - Safe integers only up to `2^53 - 1` (`Number.MAX_SAFE_INTEGER`).

4. **BigInt quirks**

   - Not interchangeable with `number`.
   - `10n + 5` ❌ TypeError. Must use same type.

5. **Undefined vs Null**

   - `undefined` = missing/uninitialized.
   - `null` = intentional absence.
   - `undefined == null` → true (loose).
   - `undefined === null` → false (strict).

6. **Objects**

   - Copied by reference, not by value.
   - Comparing objects compares reference, not contents.

     ```js
     {} === {} // false
     ```

7. **Symbols**

   - Unique always: `Symbol("a") !== Symbol("a")`.
   - Use `Symbol.for("a")` for global reuse.

8. **Arrays**

   - `typeof [] === "object"`.
   - Length auto-updates on push/pop, but `delete arr[i]` leaves hole.
   - `arr.length=0` clears whole array.

9. **Functions**

   - `First-class functions` can be assigned to variables, passed as arguments, returned from functions.
   - `typeof function(){} === "function"` (only special case).

10. **Type coercion traps**

    - When using + with non-numbers, JS converts to Primitive
    - `[] + [] → ""` ([].toString() → "", "" + "" -> "")
    - `[] + {} → "[object Object]"` ([].toString() → "", {}.toString() → "[object Object]" => "[object Object]")
    - `{} + [] → 0` (parsed as block + `+[]`).
      ```js
      {
      } // empty block
      +[]; // [] → "", +"" → 0
      ```

11. **Boxing of primitives**

    - `"hi".length` works bcoz JS wraps string in `String` object temporarily.

12. **WeakMap/WeakSet**

    - Keys must be objects.
    - Not iterable, no size property → can’t enumerate.

13. **Falsy confusion**

    - `[] == false` → true (empty array coerces to `"" → 0 → false`).
    - `{}` is truthy, but `{} == false` → false.

14. **String concatenation vs numeric addition**

    - `"2" + 1` → `"21"`
    - `"2" - 1` → `1`

15. **Boolean coercion weirdness**

    - `!!"false"` → true (non-empty string).
    - `!![]` → true, `!!{}` → true.

16. **Equality pitfalls**

    - `null == undefined` → true.
    - `null == 0` → false.
    - `[] == ![]` → true 😵 (because `![] → false → 0`, `[] → "" → 0`).
    - `0 == "0"` → true.

17. **NaN coercion**

    - `Number("abc")` → NaN (not an error).
    - `NaN == NaN` → false.
    - `null + 1` → `1`.
    - `undefined + 1` → NaN.

18. **parseInt quirks**

    - `parseInt("08")` → 8 (ignores leading zero).
    - `parseInt("08abc")` → 8.
    - `parseInt("Infinity")` → Infinity.
    - `Number("08abc")` → NaN.

19. **+ operator**

    - `+true` → 1, `+false` → 0.
    - `+"123"` → 123.
    - `+""` → 0.
    - `+[]` → 0, `+[1,2]` → NaN.

20. **Objects coercion**

    - Object → string via `toString()` or `valueOf()`.

    ```js
    [] + [] // "" (both → "")
    [] + {} // "[object Object]"
    {} + [] // 0 (block + +[])
    ```

21. **BigInt + Number**

    - Mixing throws: `10n + 5` ❌ TypeError.

22. **JSON coercion**

    - `JSON.stringify(undefined)` → undefined (not `"undefined"`).
    - `JSON.stringify(NaN)` → `"null"`.

23. **Underlying principles:**

    - JS prefers **string concatenation** in ambiguous `+` operations.
    - Other operators (`-`, `*`, `/`) coerce values to **numbers**.

24. **Weird coercion:**

    ```js
    [] == ![]; // true
    null == undefined; // true
    0 == false; // true
    ```

---

### Best Practices

- Default to **`const` objects/arrays**, avoid mutation when possible.
- Use `===` instead of `==` (avoids coercion).
- Use `Number.isNaN()` & `Number.isFinite()`.
- Use `Array.isArray()` not `typeof`.
- Prefer `BigInt` only if working beyond safe integer range.
- Use `Object.is()` for edge equality (`NaN`, `-0`).
- Performance Aspects

  - **Primitives:** Fast (stack).
  - **References:** Slower (heap, GC involved).
  - **Optimizations:**
    - Use correct data type (`Map` faster than object for dynamic keys).

---

👉 Mnemonic: **Primitives are 7 (string, number, boolean, null, undefined, symbol, bigint); Everything else = object.**

---

Here’s your **Concept Mastery Cheat Sheet** for **JavaScript Operators**, complete, compact, and with **all gotchas grouped together**.

---

# ⚙️ JavaScript Operators — Cheat Card

**Concept:** Symbols that perform operations on values.

---

### Categories & Syntax

1. **Arithmetic**
`+  -  *  /  %  **  ++  --`

2. **Assignment** 
`=  +=  -=  *=  /=  %=  **=  <<=  >>=  >>>=  &=  ^=  |=  &&=  ||=  ??=`

3. **Comparison**
`==  !=  ===  !==  >  <  >=  <=`

4. **Logical**
`&&  ||  !`

5. **Bitwise**
`&  |  ^  ~  <<  >>  >>>`

6. **Ternary (Conditional)**
`condition ? exprIfTrue : exprIfFalse`

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
`(a=1, b=2, a+b)` → evaluates left → returns last.

  #### Examples
  ```js
      5 + "5"       // "55"
      5 == "5"      // true
      5 === "5"     // false
      null ?? "x"   // "x"
      a &&= b       // a = a && b
      [...arr1, ...arr2]
      "x" in {x:1}  // true
  ```

---

### Gotchas ⚠️ (Exhaustive)

1. **Arithmetic quirks**
   - `0.1 + 0.2 !== 0.3` (precision).
   - `%` is remainder, not true modulo (`-5 % 2 → -1`).
   - `++a` vs `a++` → different return values.

2. **Assignment traps**
   - `a = b = c = 0;` → all assigned, returns `0`.
   - `||=`, `&&=`, `??=` only assign if condition matches.

3. **Comparison pitfalls**
   - `==` does coercion: `"0" == 0` → true.
   - `[] == ![]` → true.
   - `NaN == NaN` → false; use `Number.isNaN`.
   - `0 == -0` → true, but `Object.is(0,-0)` → false.
   - `null == undefined` → true, but strict equals false.

4. **Logical operators**
   - Return **last evaluated value**, not always boolean:
   - `"hello" || "hi"` → `"hello"` // || - first truthy value else last value
   - `"ok" && 123` → `123` // && - first falsy value else last value
   - `!!value` → reliable boolean cast.

5. **Bitwise gotchas**
   - Operands converted to **32-bit signed ints**.
   - `>>>` produces unsigned 32-bit int.
   - Large numbers overflow.

6. **Ternary**
   - Nested ternaries unreadable: `a?b:c?d:e`.

7. **Spread/Rest**
   - Works only on **iterables Values** (Array, string, Set).
   - Spreading non-iterable (`...123`) → TypeError.
   - Object spread ignores non-enumerable props i.e.
   - hidden properties of objects( `__proto__, __defineGetter__, __defineSetter__` etc. ) and arrays(`length, push, pop` etc. these are not enumerable and are not spread in object spread).
   - Iterable → "Can I loop its values?" (Array, string, Maps, Sets), used by 
   - (for...of, spread, destructuring)
   - Enumerable → "If property shown up in key listings?" (objects, blocks{a:1}), used by - (for...in, Object.keys)

8. **typeof quirks**

   - `typeof null` → `"object"`.
   - `typeof NaN` → `"number"`.
   - `typeof function(){}` → `"function"` (special case).
   - `typeof undeclaredVar` → `"undefined"` (no error).

9. **instanceof pitfalls**

   - Checks prototype chain, not type string.
   - Cross-frame objects fail:

    ```js
    [] instanceof Array; // false if different realm
    ```

10. **in operator**

    - `"toString" in {}` → true (inherited).

11. **delete quirks**

    - `delete obj.prop` → true if configurable.
    - `delete letVar` or `delete function` → false.

12. **void**

    - `void 0` → `undefined`.
    - Rarely useful outside bookmarks/minified code.

13. **Comma operator**

    - Confusing when mixed with `for` loops and function args.

14. **Precedence traps**

    - `a + b * c` → `a + (b*c)`
    - `typeof x === "string"` works b/c `typeof` has higher precedence.

---

### Best Practices

- Prefer `===`/`!==` over `==`/`!=`.
- Avoid chained assignments (`a=b=c=...`).
- Use logical operators for short-circuit, not as boolean substitutes.
- Avoid confusing comma/void/delete unless necessary.
- Use `Number.isNaN`, `Object.is`, and explicit casts.
- Limit use of bitwise ops unless needed.
- Write readable ternaries; prefer `if/else` for clarity.

---

👉 Mnemonic: **"Arith, Assign, Compare, Logic, Bits, Ternary, Spread, Type, Delete."**

---

Here’s your **Concept Mastery Cheat Sheet** for **JavaScript Control Structures**, compact but exhaustive, with **all gotchas grouped in one block**.

---

# 🔀 JavaScript Control Structures — Cheat Card

**Concept:** Direct the execution flow of code via branching, looping, or exceptions.

---

### Conditionals

```js
if (cond) { ... } 
else if (cond) { ... } 
else { ... }

switch (expr) {
  case val: ...; break;
  default: ...
}
```

---

### Loops

```js
for (let i=0; i<5; i++) { ... }
while (cond) { ... }
do { ... } while (cond)

for (const val of iterable) { ... } // iterable array
for (const key in enumerable) { ... } // enumerable object keys
```

---

### Jumps

```js
break;      // exit loop/switch
continue;   // skip current loop iteration
return val; // exit function
throw err;  // raise exception
```

---

### Exception Handling

```js
try { ... } 
catch (e) { ... } 
finally { ... }
```

---

### Gotchas ⚠️ (Exhaustive)

1. **If/Else pitfalls**

   * Omitting braces `{}` can cause logic errors:

     ```js
     if(x) console.log("a"); console.log("b"); // b always runs
     ```
   * Assignment vs comparison: `if (x = 5)` → always truthy.
2. **Switch quirks**

   * Missing `break` → fall-through to next case.
   * `switch` uses **strict comparison (`===`)**.
   * Duplicate `case` labels silently ignored.
   * `default` can be anywhere; execution continues unless `break`.
3. **For loop quirks**

   * `var` in loops is function-scoped → closure trap in async:

     ```js
     for(var i=0;i<3;i++) setTimeout(()=>console.log(i)); // 3,3,3
     for(let i=0;i<3;i++) setTimeout(()=>console.log(i)); // 0,1,2
     ```
   * Modifying loop counter inside loop can cause infinite loops.
4. **While/Do…While**

   * `do...while` executes at least once, even if condition false.
   * `while(true)` without `break` → infinite loop.
5. **For…in vs For…of**

   * `for…in`: iterates enumerable keys (incl. inherited).
   * `for…of`: iterates values of iterable (arrays, strings, sets, maps).
   * Iteration order in `for…in` not guaranteed (esp. objects).
6. **Break/Continue**

   * Only affect nearest loop/switch, not outer loops.
   * To exit outer loop, need labels (bad practice).
7. **Try/Catch/Finally**

   * `catch(e)` shadows outer `e`.
   * `finally` always runs (even after `return`), and can override return value:

     ```js
     function f(){ try {return 1} finally {return 2} } // → 2
     ```
   * Exceptions in async functions must be caught with `try/catch` inside `async` or `.catch()`.
8. **Throw quirks**

   * `throw` requires an argument (any value, not just Error).
   * `throw` across async boundaries won’t be caught by sync try/catch.
9. **Labels**

   * Rarely used, can create spaghetti code:

     ```js
     outer: for(...) { 
      for(...) { break outer; } 
     }
     ```
10. **Short-circuit as control flow**

    * Logical operators (`&&`, `||`, `??`) sometimes abused for flow → can reduce clarity.

---

### Best Practices

* Always use `{}` with conditionals/loops.
* Prefer `for…of` or array methods (`map`, `forEach`) over `for…in`.
* Avoid labels.
* Use `switch` only when many branches; otherwise prefer `if/else`.
* Keep loop counters and termination conditions clear.
* Always return `Error` objects, not strings, with `throw`.
* Catch only what you can handle; don’t swallow errors silently.

---

👉 Mnemonic: **“If → Switch → Loops → Jumps → Try.”**

---

Here’s your **Concept Mastery Cheat Sheet** for **JavaScript Strict Mode**, with all essential details + every gotcha grouped into one block.

---

# ⚡ JavaScript Strict Mode — Cheat Card

**Concept:** A stricter version of JavaScript introduced in ES5 (2009) to eliminate silent errors, improve security, and allow optimizations.

---

### Syntax

```js
"use strict";       // at top of file → whole script strict
(function(){
  "use strict";     // inside function → only this function strict
})();
```

---

### Behavior Changes

* Disallows **implicit globals** (must declare variables).
* `this` in functions → `undefined` (not global object).
* `delete` on variables/functions/params → SyntaxError.
* Duplicate parameter names → SyntaxError.
* Assignments to read-only properties → TypeError.
* Assignments to non-writable getters → TypeError.
* Octal literals (e.g., `0123`) → SyntaxError.
* Safer `eval` → variables declared inside `eval` don’t leak out.
* Reserved keywords (`implements`, `interface`, `let`, `package`, `private`, `protected`, `public`, `static`, `yield`) cannot be used as identifiers.

---

### Gotchas ⚠️ (Exhaustive)

1. **Implicit globals banned**

   ```js
   x = 10; // ❌ ReferenceError in strict mode
   ```
2. **`this` behavior**

   * Normal function: `this === undefined` instead of `window/globalThis`.
   * Arrow functions inherit `this` from lexical scope, unaffected.
3. **Delete restrictions**

   * `delete Object.prototype` → ❌ TypeError.
   * `delete variableName` → ❌ SyntaxError.
4. **Duplicate parameter names**

   ```js
   function f(a, a) {} // ❌ SyntaxError in strict
   ```
5. **Octal literals**

   ```js
   let x = 010; // ❌ SyntaxError
   ```
6. **Assignments to non-writable**

   ```js
   const obj = {}; Object.defineProperty(obj,"x",{value:1,writable:false});
   obj.x = 2; // ❌ TypeError
   ```
7. **With statement**

   * `with(obj){...}` → ❌ not allowed.
8. **Eval sandboxing**

   * In strict mode, vars declared inside `eval` stay inside it.
   * In sloppy mode, they leak into surrounding scope.
9. **Reserved words**

   * Using ES future keywords as identifiers (e.g., `let = 1`) → SyntaxError.
10. **Arguments object**

* No dynamic link between parameters & `arguments`.

```js
function f(a){ "use strict"; a=99; console.log(arguments[0]); }
f(1); // 1 in strict, 99 in sloppy
```

11. **Silent errors become thrown**

* E.g., assigning to `NaN = 5` throws TypeError.

12. **Function declarations in blocks**

* Strict mode disallows block-scoped function declarations in older engines (now standardized with ES6).

---

### Best Practices

* Always use `"use strict";` in legacy scripts.
* In ES6+ modules, strict mode is **enabled by default** (no need to declare).
* Write new code assuming strict mode.
* Use strict mode to catch sloppy mistakes early.

---

👉 Mnemonic: **“Strict = No Globals, No Duplicates, No Octals, Safer Eval, Safer This.”**

---

