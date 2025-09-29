### Gotchas ⚠️ (Exhaustive)

1. **ASI (Automatic Semicolon Insertion)**

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

5. **Switch fall-through**

   - Missing `break` → executes next cases unintentionally.

6. **`for…in` vs `for…of`**

   - `for…in`: iterates enumerable keys (incl. inherited).
   - `for…of`: iterates values (requires iterable).
   - Order not guaranteed in `for…in`.

7. **Labels**

   - `break`/`continue` can jump to labels → confusing spaghetti code.

8. **Strict mode (`"use strict"`)**

   - Assignment to undeclared variable → ReferenceError.
   - `this` in functions = `undefined` (not global).
   - Silent failures become thrown errors.

9. **Imports/Exports**

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

10. **Catch block quirks**

    - `catch(e)` shadows outer `e`.
    - Errors swallowed if not re-thrown.

11. **Reserved words**

    - Keywords like `class`, `import` can’t be used as identifiers.
    - Some old reserved words (`enum`, `implements`) disallowed in strict mode.

12. **Unicode confusion**

    - Visually identical characters (e.g., Greek `Σ` vs Latin `S`) can slip into identifiers.

13. **Legacy quirks**

    - `with` is deprecated (still parses in sloppy mode).
    - `eval` can inject new variables into scope (sloppy mode).

---

### Best Practices

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

  8. **Arrays**

  - Length auto-updates on push/pop, but `delete arr[i]` leaves hole.
  - `arr.length=0` clears whole array.

9. **Functions**

   - `First-class functions` can be assigned to variables, passed as arguments, returned from functions.
   - `typeof function(){} === "function"` (only special case).

10. **WeakMap/WeakSet**

    - Keys must be objects.
    - Not iterable, no size property → can’t enumerate.

    7. **Spread/Rest**

    - Works only on **iterables Values** (Array, string, Set).
    - Spreading non-iterable (`...123`) → TypeError.
    - Object spread ignores non-enumerable props i.e.
    - hidden properties of objects( `__proto__, __defineGetter__, __defineSetter__` etc. ) and arrays(`length, push, pop` etc. these are not enumerable and are not spread in object spread).
    - Iterable → "Can I loop its values?" (Array, string, Maps, Sets), used by
    - (for...of, spread, destructuring)
    - Enumerable → "If property shown up in key listings?" (objects, blocks{a:1}), used by - (for...in, Object.keys)

    8. **Default exports in ES modules**

    - Function declarations exported as default are named, only if explicitly given; else function expressions may show as `default`.

    ```js
    export default function foo() { } // named -> foo
    export default function () { } // anonymous -> default
    ```
