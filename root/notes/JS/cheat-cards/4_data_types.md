# JavaScript Advanced Types & Structures

## Concepts

### Strings

- Immutable sequences of UTF-16 code units, used for text.

#### Syntax

```js
const s1 = "hello";
const s2 = `hi ${s1}`; // Interpolation: template literal
const s3 = new String("boxed"); // ❌ rarely use
// multi-line
const s4 = `Line 1
Line 2`;
```

#### Properties:

1. **Immutable** → cannot change individual chars.
   ```js
   let s = "hi";
   s[0] = "H"; // ❌ no effect
   ```
2. **Length** → `str.length`.
3. Indexed access: `str[0]`.
4. Unicode characters: `"😀".length === 2` (2 code units).
5. Grapheme clusters: `"😀".length === 1` (1 grapheme cluster).
6. Normalization: `"é".normalize() === "e\u0301".normalize()` (true).
7. Iteration: `for (const ch of "hi") console.log(ch);` (h, i).
8. Concatenation: `"hi" + "bye" === "hi bye"` (true).
9. Boxed vs primitive

   - All string methods work on primitives, but when called, JS temporarily boxes them into objects internally.

   ```js
   const x = "x"; // creates a primitive string.
   const y = new String("x"); // creates a String object (wrapper), not a primitive.

   typeof "x"; // "string"
   typeof new String("x"); // "object"

   new String("x") === "x"; // false
   ```

#### Methods

1. Character Access

   1. `at(index)` → `"abc".at(1); // "b"`
      - returns the character at the given index (supports negative index).
   2. `charAt(index)` → `"abc".charAt(1); // "b"`
      - returns the character at the given index.
   3. `charCodeAt(index)` → `"abc".charCodeAt(1); // 98"`
      - returns the UTF-16 code unit of the character at the given index.
   4. `codePointAt(index)` → `"😊".codePointAt(0); // 128522"`
      - returns the full Unicode code point value at the given index.

2. Search & Match

   1. `includes(substring)` → `"abc".includes("b"); // true"`
      - returns true if the string contains the given substring.
   2. `startsWith(substring)` → `"abc".startsWith("a"); // true"`
      - returns true if the string starts with the given substring.
   3. `endsWith(substring)` → `"abc".endsWith("c"); // true"`
      - returns true if the string ends with the given substring.
   4. `indexOf(substring)` → `"abc".indexOf("b"); // 1"`
      - returns the index of the first occurrence of the given substring.
      - case-sensitive by default.
      - returns -1 if the substring is not found.
   5. `lastIndexOf(substring)` → `"abcb".lastIndexOf("b"); // 3"`
      - returns the index of the last occurrence of the given substring.
      - case-sensitive by default.
      - returns -1 if the substring is not found.
   6. `search(regex)` → `"abc".search(/b/); // 1"`
      - returns the index of the first match for a regex pattern.
   7. `match(regex)` → `"abc".match(/b/); // ["b"]"`
      - returns an array with match info or null if not found.
   8. `matchAll(regex)` → `Array.from("abcabc".matchAll(/b/g)); // [["b"],["b"]]`
      - returns an iterator for all matches of a regex.

3. Extract / Slice

   1. `slice(start, end)` → `"abcdef".slice(1, 4); // "bcd"`
      - returns a substring from start index to end index (supports negatives).
   2. `substring(start, end)` → `"abcdef".substring(1, 4); // "bcd"`
      - returns substring between start and end indexes (no negatives).
   3. `substr(start, length)` → `"abcdef".substr(1, 3); // "bcd"`
      - returns a substring of given length (legacy, avoid).

4. Replace / Modify

   1. `replace(substring, replacement)` → `"abc".replace("b", "x"); // "axc"`
      - returns new string with first match replaced.
   2. `replaceAll(substring, replacement)` → `"abcabc".replaceAll("b", "x"); // "axcaxc"`
      - returns new string with all matches replaced.

5. Split & Join

   1. `split(separator)` → `"a,b,c".split(","); // ["a","b","c"]"`
      - returns an array of substrings divided by separator.
      - `split("")` breaks surrogate pairs:
        ```js
        "😀".split(""); // ["�","�"]
        ```
      - Use `x => [...str]` to split into individual characters.
   2. `join(separator)` → `["a","b","c"].join(","); // "a,b,c"`
      - returns a string by joining all array elements with the separator.
      - _(Note: `join()` is from Array, not String.)_

6. Case Conversion

   1. `toUpperCase()` → `"abc".toUpperCase(); // "ABC"`
      - returns string in uppercase.
   2. `toLowerCase()` → `"ABC".toLowerCase(); // "abc"`
      - returns string in lowercase.
   3. `toLocaleUpperCase()` → `"abc".toLocaleUpperCase(); // "ABC"`
      - returns string in uppercase using locale.
   4. `toLocaleLowerCase()` → `"ABC".toLocaleLowerCase(); // "abc"`
      - returns string in lowercase using locale.

7. Trim & Pad

   1. `trim()` → `"  abc  ".trim(); // "abc"`
      - removes whitespace from both ends.
   2. `trimStart()` → `"  abc  ".trimStart(); // "abc  "`
      - removes leading whitespace.
   3. `trimEnd()` → `"  abc  ".trimEnd(); // "  abc"`
      - removes trailing whitespace.
   4. `padStart(length, char)` → `"abc".padStart(5, "0"); // "00abc"`
      - pads string on the left to reach desired length.
   5. `padEnd(length, char)` → `"abc".padEnd(5, "0"); // "abc00"`
      - pads string on the right to reach desired length.

8. Repeat & Concat

   1. `repeat(count)` → `"abc".repeat(2); // "abcabc"`
      - repeats the string specified number of times.
   2. `concat(...strings)` → `"a".concat("b","c"); // "abc"`
      - concatenates multiple strings and returns a new string.
      - For large builds, use arrays + `join()`.

9. Locale & Comparison

   1. `localeCompare(otherString)` → `"a".localeCompare("b"); // -1"`
      - compares strings according to locale sorting rules.
   2. `normalize(form)` → `"é".normalize("NFD"); // "e\u0301"`
      - returns Unicode-normalized form (NFC, NFD, NFKC, NFKD).

10. Unicode Safety

    1. `isWellFormed()` → `"abc".isWellFormed(); // true"`
       - checks if string is valid UTF-16 (no lone surrogates).
    2. `toWellFormed()` → `"\uD800".toWellFormed(); // "�"`
       - replaces invalid surrogates with replacement char.

11. Conversion

    1. `toString()` → `"abc".toString(); // "abc"`
       - returns the string itself.
    2. `valueOf()` → `"abc".valueOf(); // "abc"`
       - returns the string itself.
    3. `toLocaleString()` → `"abc".toLocaleString(); // "abc"`
       - returns the string itself.

12. Static String Methods

    1. `String.fromCharCode(65,66,67); // "ABC"`
       - creates string from UTF-16 codes.
    2. `String.fromCodePoint(128512); // "😀"`
       - creates string from Unicode code points.
    3. `String.raw\`A\nB`; // "A\nB"`
       - returns raw string from template literal.

13. Deprecated / Legacy (avoid)

    1. `substr(start, length)` → `"abcdef".substr(1, 3); // "bcd"`
       - returns a substring of given length (legacy, avoid).
    2. `anchor()`, `big()`, `blink()`, `bold()`, `fixed()`, `fontcolor()`, `fontsize()`, `italics()`, `link()`, `small()`, `strike()`, `sub()`, `sup()` – old HTML-formatting methods.

#### Template Literals

- (backticks `` ` ``) allow **embedded expressions, multi-line strings, and custom string processing**
  - Delimited by backticks `` ` ``.
  - Support `${expr}` for embedding values.
  - Preserve newlines/whitespace.

```js
const name = "Kalidas"; // string
`Hello, ${name}!`; // interpolation
`Hello ${`Mr. ${name}`}`; // Nesting Interpolation
`2 + 3 = ${2 + 3}`; // "2 + 3 = 5" Expression Interpolation

`Line 1
Line 2`; // Multi-line String interpolation

`\``; // literal backtick -> `\`` -> escape backtick

// Template literals don't automatically escape quotes.
const val = 'val1"val2';
`{"x": "${val}"}`; // ❌ unsafe if val contains quotes -> `{"x": "val1"val2"}`

// Tagged Templates
function customTag(strings, ...values) {
  return strings[0] + values.map((v) => v.toUpperCase()).join(" ");
}
customTag`hi ${"world"} ${"kalidas"}`; // "hi WORLD KALIDAS" Tagged Templates

// template literal uses toString() to convert the expression to a string.
const obj = { toString: () => "OBJ" };
`${obj}`; // "OBJ" -> toString() is overridden to return "OBJ"
```

#### Unicode, Graphemes & Normalization

- Unicode
  - JS strings are UTF-16 encoded.
  - **Code unit**: 16-bit chunk (what JS indexes).
  - **Code point**: actual Unicode scalar value, A number that identifies a character in Unicode.
  - **Surrogate pairs**: characters outside BMP (`U+10000+`) use 2 code units.

```js
// Code Units vs Code Points
"😀".length; // 2 (two UTF-16 code units) -> surrogate pairs
"😀".codePointAt(0); // 128512 (true Unicode code point)
```

- Graphemes
  - Characters (graphemes) may consist of one or more code points.
  - Grapheme clusters ≠ JS string “character”.
  - `"á".split("")` → `["a","́"]` (combining marks separate).
  - Use `[...str]` or libraries like Graphemer for grapheme-safe splits.
  - Normalization required - String visually same may compare unequal unless normalized e.g.
  - `"é" (U+00E9) ≠ "é" (U+0065 U+0301)`
  - `"é".normalize() === "é".normalize()` // true

```js
`🇮🇳` (India flag) // 2 code points
`á` // `a + combining acute`
[..."🇮🇳"].length; // 2 (not 1)
"🇮🇳"[0]; // "\uD83C\uDDEE" (half a char) not valid as it splits the surrogate pair
const ar = [..."🇮🇳"]; // ["🇮🇳"]
```

- Normalization
  - Ensures visually identical characters with different Unicode representations are treated equally for comparison, sorting, and storage.
  - Forms:
    - **NFC** (Normalization Form C): composed, default.
    - **NFD** (Normalization Form D): decomposed.
    - **NFKC** (Normalization Form KC): compatibility variants.
    - **NFKD** (Normalization Form KD): compatibility variants.
  - `.normalize(form)` ensures canonical form (NFC by default).

```js
const s1 = "é"; // U+00E9
const s2 = "e\u0301"; // e + combining acute
s1 === s2; // false
s1.normalize() === s2.normalize(); // true default form is NFC

// NFC: Joins things together -> "e + ´ " becomes "é" (nice and tidy)
const s = "e\u0301"; // "e" + accent -> "é"
console.log(s); // "é" (looks same as "é" but different Unicode representation)
console.log(s === "é"); // false 😕
console.log(s.normalize("NFC") === "é"); // true ✅

// NFD: Pulls things apart -> "é" becomes "e + ´" (you can see the parts).
const s = "é";
console.log(s.normalize("NFD")); // "e\u0301"

// NFKC: Makes fancy letters or symbols normal -> "①" becomes "1" (plain version).
const s = "①";
console.log(s.normalize("NFKC")); // "1"

// NFKD: Makes fancy letters normal and pulls them apart -> "①é" becomes "1e + ´" (simple pieces).
const s = "①é";
console.log(s.normalize("NFKD")); // "1e\u0301"

// Iteration -> Iterates over code points, not code units -> "😀á" -> "😀", "a", "́"
for (const ch of "😀á") console.log(ch); // "😀", "a", "́"
```

### Numbers

- Numbers in JS (except `BigInt`) are **64-bit IEEE-754 floating point** → can represent integers up to **2⁵³-1 safely**.
- **Immutable** → cannot modify numbers in place.
  - `let x = 10; x++; // creates new value, doesn’t mutate`

```js
let n1 = 42; // integer
let n2 = 3.14; // float
let n3 = 1e6; // exponential
let n4 = 0b1010; // binary (10)
let n5 = 0o755; // octal (493)
let n6 = 0xff; // hex (255)
```

#### Properties

##### Core Properties

1. Immutable → `let x = 10; x++;`
   - Numbers are primitive values; they cannot be changed in place.
2. `Number.prototype` → `Object.getOwnPropertyNames(Number.prototype);`
   - Base prototype for all number methods like `toFixed`, `toString`, etc.
3. `Number.length` → `1`
   - Indicates the Number constructor takes one argument.
4. `Number.name` → `"Number"`
   - Returns the constructor name.
5. `Number.constructor` → `Function`
   - References the built-in Function constructor.
6. `Number.prototype.constructor` → `Number`
   - Points back to the Number constructor function.

##### Static Constants

7. `Number.MAX_VALUE` → `1.7976931348623157e+308`
   - The largest finite representable number.
8. `Number.MIN_VALUE` → `5e-324`
   - The smallest positive number greater than zero.
9. `Number.MAX_SAFE_INTEGER` → `9007199254740991`
   - Largest integer that can be represented safely (2⁵³ − 1).
10. `Number.MIN_SAFE_INTEGER` → `-9007199254740991`
    - Smallest safe integer representable.
11. `Number.POSITIVE_INFINITY` → `Infinity`
    - Represents positive infinity.
12. `Number.NEGATIVE_INFINITY` → `-Infinity`
    - Represents negative infinity.
13. `Number.NaN` → `NaN`
    - Represents “Not-a-Number”.
14. `Number.EPSILON` → `2.220446049250313e-16`
    - The smallest difference between 1 and the next representable number.

##### BigInt Safe Range

15. Safe Integer Range → `Number.isSafeInteger(9007199254740992); // false`
    - Integers beyond ±(2⁵³ − 1) lose precision; use BigInt instead.

##### Instance & Type Behavior

16. Primitive Type → `typeof 42; // "number"`
    - Numbers are primitives.
17. Boxed Object → `typeof new Number(42); // "object"`
    - `new Number()` creates a Number object wrapper (avoid in practice).
18. Auto-Boxing → `(42).toFixed(2); // "42.00"`
    - JS auto-boxes primitives to use methods.
19. Immutable Behavior → `let n = 5; n + 1; n; // 5`
    - Operations return new values, don’t mutate.

##### Special Values

20. `Infinity` → `1 / 0;`
    - Represents overflow beyond numeric limit.
21. `-Infinity` → `-1 / 0;`
    - Represents underflow beyond negative limit.
22. `NaN` → `parseInt("abc");`
    - Returned for invalid numeric operations.
23. `-0` → `Object.is(+0, -0); // false`
    - `-0` is distinct from `+0`, though they behave similarly in math.

##### ES6+ Type Checking & Parsing

24. `Number.isFinite(x)` → `Number.isFinite(10 / 2);`
    - Checks if value is finite (no coercion).
25. `Number.isInteger(x)` → `Number.isInteger(10.5);`
    - Checks if value is an integer (no coercion).
26. `Number.isNaN(x)` → `Number.isNaN(NaN);`
    - Checks for NaN without coercion (unlike global `isNaN`).
27. `Number.isSafeInteger(x)` → `Number.isSafeInteger(9007199254740992);`
    - Checks if integer is within the safe range.
28. `Number.parseFloat(str)` → `Number.parseFloat("3.14abc"); // 3.14`
    - Parses string to float (same as global `parseFloat`).
29. `Number.parseInt(str)` → `Number.parseInt("42px");`
    - Parses string to integer (same as global `parseInt`).

| Category  | Examples                                 | Description                        |
| --------- | ---------------------------------------- | ---------------------------------- |
| Limits    | `MAX_VALUE`, `MIN_VALUE`                 | Range boundaries                   |
| Precision | `EPSILON`, `MAX_SAFE_INTEGER`            | Floating-point & integer precision |
| Infinity  | `POSITIVE_INFINITY`, `NEGATIVE_INFINITY` | Overflow markers                   |
| Invalid   | `NaN`, `isNaN()`                         | Non-numeric indicators             |
| Checking  | `isFinite()`, `isInteger()`              | Type-safe validators               |
| Type      | `typeof`, `instanceof`                   | Identify primitives vs objects     |

#### Methods

##### Number Prototype Methods

1. `toFixed(digits)` → `(3.14159).toFixed(2); // "3.14"`
   - Formats number using fixed decimal places (string output).
2. `toPrecision(precision)` → `(3.14159).toPrecision(3); // "3.14"`
   - Formats number to a specified total number of digits (scientific or fixed).
3. `toExponential(digits)` → `(123456).toExponential(2); // "1.23e+5"`
   - Returns string in exponential (scientific) notation with defined decimals.
4. `toString(radix)` → `(255).toString(16); // "ff"`
   - Converts number to a string in the given base (2–36).
5. `valueOf()` → `(42).valueOf(); // 42`
   - Returns the primitive numeric value of a Number object.
6. `toLocaleString(locale, options)` → `(1234567.89).toLocaleString("en-IN"); // "12,34,567.89"`
   - Formats number according to locale and formatting options (useful for currency, thousands separators).
7. `toLocaleString(locale, options)` → `(1234.56).toLocaleString("en-US", { style: "currency", currency: "USD" }); // "$1,234.56"`
   - Returns localized string representation with currency, percent, or unit formats.
8. `toLocaleString(locale, options)` → `(1234567).toLocaleString("en-IN"); // "12,34,567"`
   - Locale-specific grouping for Indian numbering system.
9. `toString()` (without radix) → `(42).toString(); // "42"`
   - Default base-10 string representation.
10. `toExponential()` (default) → `(77).toExponential(); // "7.7e+1"`
    - Converts number to exponential notation (default precision auto-calculated).
11. `toPrecision()` (without argument) → `(77.1234).toPrecision(); // "77.1234"`
    - Returns number as is, without formatting.
12. `toFixed()` (default) → `(77.1234).toFixed(); // "77"`
    - Rounds to nearest integer and returns string.
13. `Precision Loss` → `(0.1 + 0.2).toFixed(2); // "0.30"` (rounded, not exact `0.30000000000000004`).
    - Always returns **string**, not number — convert back with `parseFloat()` if needed.
14. `Chaining Example` → `(123.456).toFixed(1).toString(); // "123.5"`
    - Methods can be chained for formatting pipelines.
15. `Auto-Boxing` → `(42).toFixed(2)` works even on primitives — JS internally boxes the number into a temporary `Number` object.
16. `Default Conversion Order` → When coerced to string, JS calls `toString()` → `valueOf()` as fallback.
17. `Locale Formatting Performance` → `toLocaleString()` is **heavier** than `toFixed()` due to ICU library calls — avoid inside loops.

##### Math API

```js
Math.round(4.6); // 5
Math.floor(4.9); // 4
Math.ceil(4.1); // 5
Math.trunc(4.9); // 4
Math.random(); // [0,1)
Math.max(1, 2, 3); // 3
Math.pow(2, 3); // 8
2 ** 3; // 8 (exponentiation operator)
```

#### Gotchas ⚠️ (Exhaustive)

1. **Floating-point precision**
   - `0.1 + 0.2 !== 0.3` → `0.30000000000000004`
   - Use `Number.EPSILON` for tolerance:
     ```js
     Math.abs(a - b) < Number.EPSILON;
     ```
2. **Safe integer limits**
   - Beyond ±(2⁵³-1) → precision errors.
   - `Number.isSafeInteger(2**53); // false`
   - Use `BigInt` for huge integers.
3. **NaN quirks**
   - `NaN === NaN` → false.
   - Only reliable via `Number.isNaN()` or `Object.is()`.
4. **Infinity**
   - Dividing by 0 → `Infinity`.
   - Arithmetic with `Infinity` may produce `NaN`.
5. **parseInt pitfalls**
   - `parseInt("08")` → `8` (not octal in ES5+).
   - Always pass radix: `parseInt("08", 10)`.
6. **Loose equality coercion**
   - `"42" == 42` → true.
   - `"42px"` == 42 → false, but `parseInt("42px")` → 42.
7. **Bitwise ops**
   - Convert numbers to **32-bit signed ints**.
   - Large numbers overflow: `1<<31` → -2147483648.
8. **JSON quirks**
   - JSON.stringify drops `Infinity`/`NaN` → `null`.
9. **-0**
   - `-0` exists (distinct from `0` in `Object.is`).
   - Breaks some logic (e.g., dividing).
10. **Math.random**
    - Returns pseudo-random; not cryptographically secure.
    - Use `crypto.getRandomValues` for secure random.
11. **Number wrappers**
    - `new Number(5)` creates object, not primitive. Avoid.
12. **Trailing decimals**
    - `42.` is valid JS (same as `42.0`).
13. **Octal literals**
    - Legacy `012` is disallowed in strict mode.

### Floating-Point Precision

- All JS numbers use **IEEE-754 double-precision floating point (64-bit)**.
  - 53 bits of precision → ~15–17 decimal digits.
  - Some decimal fractions **cannot be represented exactly**.

```js
0.1 + 0.2; // 0.30000000000000004
0.3 - 0.2; // 0.09999999999999998

// fix
//Tolerant Comparison
function nearlyEqual(a, b, eps = Number.EPSILON) {
  return Math.abs(a - b) < eps;
}
nearlyEqual(0.1 + 0.2, 0.3); // true
Number.EPSILON ≈ 2.22e-16 // smallest difference detectable.

//Rounding
Number((0.1 + 0.2).toFixed(2)); // 0.3

//Integer Math (Scaling)
(0.1 * 10 + 0.2 * 10) / 10; // 0.3

// Use libraries like big.js, decimal.js, bignumber.js for financial/math-critical apps.
```

#### Gotchas ⚠️ (Exhaustive)

1. **Equality fails**
   - Direct comparison unreliable: `(0.1+0.2)===0.3` → false.
2. **Large vs small numbers**
   - Precision shrinks as numbers grow.
   - `9007199254740993 === 9007199254740992` → true (past safe int).
3. **Division quirks**
   - `1/49*49` ≠ `1`.
4. **Associativity issues**
   - `(a+b)+c !== a+(b+c)` with floats.
5. **JSON.stringify**
   - Converts numbers to shortest decimal that round-trips, may look odd.
6. **toFixed rounding**
   - `1.005.toFixed(2)` → "1.00" (binary fraction issue).
7. **Math.max precision**
   - Very large values may overflow to Infinity.
8. **-0 quirks**
   - `1/-Infinity` → -0.
   - `-0===0` → true, but `Object.is(-0,0)` → false.
9. **Bitwise ops**
   - Coerce floats to 32-bit signed ints → precision loss.

### Arrays

- Ordered, zero-indexed collections of values. Special type of object with powerful built-in methods.

#### Syntax

```js
const arr1 = [1, 2, 3];
const arr2 = new Array(3); // length = 3, empty slots
const arr3 = Array.of(1, 2, 3); // [1,2,3]
const arr4 = Array.from("abc"); // ["a","b","c"]
```

#### Properties

- **Index-based access** → `arr[0]`
- **Length** → `arr.length` (mutable)
- **Sparse arrays** allowed (`[ , , 3]`)

#### Methods (mutating - change the existing array and return)

```js
push(4), pop(); // add-remove END
shift(), unshift(0); // add-remove START
splice(1, 2, "x"); // remove-replace-insert
sort(), reverse();
fill(0), copyWithin();
```

#### Methods (non-mutating - create a new array and return a new array)

```js
concat(), slice(), join(), includes(), indexOf(), lastIndexOf(),toReversed(), toSorted(), toSpliced(), with();
```

#### Iteration / Functional Methods

```js
// Executes callback for each element (no return)
[].forEach((element, index, array) => ... undefined ),

// Transforms each element → returns new array
[].map((element, index, array) => ... new array),

// Keeps elements where callback → truthy → returns new array
[].filter((element, index, array) => ... new array),

// Maps each element, then flattens result one level → returns new array
[].flatMap((element, index, array) => ... new array),

// Returns first element that satisfies condition (or undefined)
[].find((element, index, array) => ... element ),

// Reduces array and accumulates values → single value (left → right)
// If no initialValue → first element of array used as initialValue.
[].reduce((accumulator, currentValue, currentIndex, array) => ... , initialValue ),

// Same as reduce but accumulates values from right to left
[].reduceRight((accumulator, currentValue, currentIndex, array) => ... , initialValue ),

// Returns index of first element that satisfies condition (or -1)
[].findIndex((element, index, array) => ... index ),

// Returns true if any element passes the test → boolean
// on empty array returns false
[].some((element, index, array) => ... boolean ),

// Returns true if all elements pass the test → boolean
// on empty array returns true
[].every((element, index, array) => ... boolean ),

// Flattens nested arrays up to given depth → returns new array
[].flat(depth = 1) ,

```

#### Other Methods

```js
at(-1) → last element.
```

#### Caveats

##### Sparse Arrays

- Sparse Arrays: Arrays with “holes” → indices exist in `length` count but have **no values** stored.
- Sparse arrays are slower (not optimized like dense arrays)
  - Parser doesn’t check holes.
  - Runtime marks array as holey when a missing element appears.
  - Sparse arrays are slower due to extra hole checks.
  - Dense arrays use optimized packed storage.
  - Tip: Avoid holes → use `undefined` explicitly if needed.

```js
const a = new Array(3); // [ <3 empty items> ]
const b = [1, , 3]; // [1, <empty>, 3]
b.length; // 3
b[1] = returns undefined; // [1, undefined, 3]

// Preserves hole
arr.join("-"); // "1--3"
arr.toString(); // "1,,3"

// Skips hole (forEach, map, filter, some, every, reduce)
arr.forEach((x) => console.log(x)); // 1, 3
arr.map((x) => x || 0); // [1, <empty>, 3]

// include existing indices only (`for...in`, `for...of`, `Object.keys`)
for (const index in arr) {
  console.log(index); // 0, 2
}

// turn holes into `undefined` (`Array.from`, spread)
Array.from(arr); // [1, undefined, 3]
[...arr]; // [1, undefined, 3]

// JSON.stringify converts holes to `null`
JSON.stringify([, 1, 2]); // "[null,1,2]"

// Fills holes
arr[1] = 2; // now [1,2,3]
// fill holes with a value
new Array(3).fill(0); // [0,0,0]

// delete creates a hole
delete arr[1]; // now [1,,3]
// splice removes elements and shifts indices
arr.splice(1, 1); // now [1,3]

// use `Array.fill` to initialize
new Array(3).fill(0); // [0,0,0]

// use `Array.from` to convert to array
Array.from([1,,3]); // [1,undefined,3]


//Undefined ≠ empty slot
[undefined].length; // 1
[,].length; // 1 (but hole, not undefined)
```

##### Array-like Objects

- Objects that look like arrays (indexed keys + `length`) but lack array methods.
- Examples:
  - `arguments`
  - DOM collections (`NodeList`, `HTMLCollection`)
  - Typed arrays (`Uint8Array`, etc.)
  - Custom objects `{0:"a",1:"b",length:2}`
- Properties
  - Indexed elements → numeric keys `"0"`, `"1"`, …
  - `.length` property.
  - No `map`, `filter`, `reduce`, etc.
- Copy traps
  - Spreading `[...obj]` only works if iterable. Non-iterable array-like → `TypeError`.
- Always check `Array.isArray()` when expecting real arrays.

Converting to Real Array with `Array.from` or spread

```js
const obj = { 0: "a", 1: "b", length: 2 };
Array.from(obj); // ["a","b"]
[...obj]; // ["a","b"]
Array.from(arguments);
[...arguments];
```

#### alert⚠️

- Length:
  - `arr.length=0` clears array.
  - Setting smaller length truncates.
  - Larger length creates holes.
- Sort:
  - Default `sort()` converts to strings.
  - `["10","2"].sort()` → `["10","2"]`.
  - Fix: `arr.sort((a,b)=>a-b)`.
- Delete:
  - `delete arr[0]` leaves a hole, doesn’t shrink length.
- Comparison:
  - Arrays are objects; compared by reference.
  - `[1]==[1]` → false.
- Copy:
  - Spread (`[...arr]`) = shallow copy.
  - Nested objects still reference same memory.
  - Use `structuredClone` for deep copies.
- Negative indices:
  - `arr[-1]` is not last element, it’s a property.
  - Use `.at(-1)` instead.

## Best Practices

- Use array literals `[]` instead of `new Array()`.
- Use `.at(-1)` for last element instead of hacks.
- Always provide compare function to `sort()`.
- Use `map/filter/reduce` for clarity, but avoid in performance-critical loops.
- Convert array-likes with `Array.from()`.

- Iteration

  - Use `map` for transformation, `filter` for selection.
  - Always provide `initialValue` in `reduce`.
  - Use `find` for first match, `filter` for multiple.
  - Don’t use `map` if you’re not returning a value (use `forEach`).
  - Use `some`/`every` for readability over manual loops.
  - Be cautious with sparse arrays [ , , ]; prefer initializing fully.

- Avoid creating sparse arrays intentionally.
- **“Array-like = Looks like array, but walks like object.”**

---

## Mnemonic

- **“Strings = Immutable text, UTF-16 quirks, handle with care.”**
- **“Template Literals = Backticks + ${Expressions} + Multiline + Tags.”**
- **“Code units ≠ Code points ≠ Graphemes. Normalize for equality.”**

- **“Floats Lie — Compare with EPSILON, Scale to Integers, or Use Big Decimal.”**

- “Array = Ordered list, mind the holes, always copy carefully.”
- **map = transform**
- **filter = keep**
- **reduce = fold into one**
- **find = first match**
- **some = at least one**
- **every = all pass**

- **“Holes ≠ Undefined. Holes = skipped, undefined = value.”**

---

---

---

---

---

---

---

# 🔢 JavaScript `Number.MAX_SAFE_INTEGER` & `Number.EPSILON` — Cheat Card

---

### `Number.MAX_SAFE_INTEGER`

```js
console.log(Number.MAX_SAFE_INTEGER);
// 9007199254740991  (2^53 - 1)
```

- Largest integer that can be **safely represented** in JS (`IEEE-754`).
- Past this, integers lose precision → rounding errors.
  **Check safety:**

```js
Number.isSafeInteger(9007199254740991); // true
Number.isSafeInteger(9007199254740992); // false
```

---

### `Number.EPSILON`

```js
console.log(Number.EPSILON);
// 2.220446049250313e-16
```

- The **smallest difference** between 1 and the next representable float.
- Useful for comparing floating-point numbers.
  **Example:**

```js
function nearlyEqual(a, b) {
  return Math.abs(a - b) < Number.EPSILON;
}
nearlyEqual(0.1 + 0.2, 0.3); // true
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Precision limit**
   - Beyond `MAX_SAFE_INTEGER`, not all integers are unique.
   ```js
   9007199254740992 === 9007199254740993; // true (!)
   ```
2. **BigInt fix**
   - Use `BigInt` for integers larger than ±(2^53 - 1).
   ```js
   9007199254740992n + 1n; // works fine
   ```
3. **EPSILON scale**
   - Too small for big numbers.
   - Works best when numbers are close to 1.
   ```js
   1e20 + 1 === 1e20; // true, EPSILON can’t detect
   ```
   - Use **relative epsilon**:
     ```js
     function nearlyEqualRel(a, b, eps = Number.EPSILON) {
       return Math.abs(a - b) < eps * Math.max(Math.abs(a), Math.abs(b));
     }
     ```
4. **Not absolute precision**
   - `EPSILON` is machine precision, not a universal tolerance.
   - Depends on magnitude of compared numbers.
5. **JSON/serialization**
   - Large numbers beyond safe integer may silently round in `JSON.stringify`.
6. **Math operations**
   - Repeated additions/subtractions of tiny decimals accumulate error.

---

### Best Practices

- ✅ Use `Number.isSafeInteger()` before arithmetic on large ints.
- ✅ Use `BigInt` for cryptography, counters, IDs, money.
- ✅ Use `Number.EPSILON` (or relative epsilon) for float comparisons.
- ✅ Document when you rely on float tolerance in critical systems.
- ❌ Don’t rely on equality checks for floats (`===`).

---

👉 Mnemonic:

- **MAX_SAFE_INTEGER** = integer safety limit.
- **EPSILON** = float equality tolerance.

---

Would you like me to now create a **cheat card comparing `Number` vs `BigInt`** (safe integers vs arbitrary precision), since that’s a natural follow-up to MAX_SAFE_INTEGER?
Here’s your **Concept Mastery Cheat Sheet** for **BigInt** in JavaScript — crisp, complete, and with **all gotchas grouped together**.

---

# 🔢 JavaScript BigInt — Cheat Card

**Concept:**
`BigInt` is a primitive type (ES2020) for representing **arbitrary-precision integers**, beyond the safe range of `Number`.

---

### Syntax

```js
const big1 = 123n; // literal
const big2 = BigInt(123); // constructor
```

---

### Operations

```js
10n + 20n; // 30n
100n - 50n; // 50n
5n * 2n; // 10n
7n / 2n; // 3n (integer division, no decimals)
7n % 2n; // 1n
```

- Supports `+ - * / % **`.
- Bitwise ops work (`& | ^ << >>`).
- Comparisons work with Numbers:
  ```js
  10n == 10; // true (loose)
  10n === 10; // false (strict)
  11n > 10; // true
  ```

---

### Conversions

```js
Number(10n); // 10
BigInt("9007199254740993"); // valid string
```

---

### Gotchas ⚠️ (Exhaustive)

1. **No mixing with Numbers**
   - Arithmetic between `BigInt` and `Number` throws:
     ```js
     10n + 1; // ❌ TypeError
     ```
   - Must convert explicitly.
2. **Division truncates**
   - Always integer: `7n/2n → 3n`, never decimals.
3. **JSON incompatibility**
   - `JSON.stringify(10n)` → ❌ TypeError.
   - Must convert manually (`.toString()`).
4. **Math functions unsupported**
   - `Math.sqrt(16n)` ❌ TypeError.
   - Need custom bigint math or libraries.
5. **Performance**
   - Slower than `Number` (arbitrary precision). Use only when needed.
6. **Boolean coercion**
   - `if (0n)` → false; all other BigInts → true.
7. **Bitwise ops**
   - Work fine but limited to bigint semantics.
8. **Literal suffix**
   - `123n` works; cannot write `0.5n` (no decimals).
9. **Edge case conversions**
   - `Number.MAX_SAFE_INTEGER+2 === Number.MAX_SAFE_INTEGER+1` → true
   - `BigInt(Number.MAX_SAFE_INTEGER)+2n` → correct.
10. **Web APIs**
    - Not all browser APIs support BigInt yet (e.g., IndexedDB keys).

---

### Best Practices

- ✅ Use `BigInt` for IDs, counters, cryptography, financial/large integer ops.
- ✅ Convert explicitly when mixing with `Number`.
- ✅ Use `.toString()` for serialization.
- ❌ Don’t use for decimals/floats.
- ✅ Stick to `Number` when within safe integer range (faster).

---

## 👉 Mnemonic: **“BigInt = Arbitrary precision ints, but no floats, no JSON, no Math.”**

Would you like me to prepare a **cheat card on BigInt vs Number (side-by-side comparison)** — since that’s often an interview favorite?

## Here’s your **Concept Mastery Cheat Sheet** for **Symbols** in JavaScript — precise, practical, with **all gotchas grouped together**.

# 🌀 JavaScript Symbols — Cheat Card

**Concept:**
A `Symbol` is a **unique and immutable primitive** (ES6) used mainly as **object property keys** to avoid collisions.

---

### Syntax

```js
const sym1 = Symbol("desc");
const sym2 = Symbol("desc");
sym1 === sym2; // false (always unique)
// Global symbol registry
const g1 = Symbol.for("id");
const g2 = Symbol.for("id");
g1 === g2; // true
```

---

### Use as Keys

```js
const id = Symbol("id");
const user = { [id]: 123 };
console.log(user[id]); // 123
```

- Symbols don’t collide with string keys.
- Useful for internal or “hidden” properties.

---

### Well-Known Symbols

```js
class MyCollection {
  [Symbol.iterator]() {
    return [1, 2, 3][Symbol.iterator]();
  }
}
for (const x of new MyCollection()) console.log(x); // 1,2,3
```

- `Symbol.iterator` → iteration protocol.
- `Symbol.toPrimitive` → custom type coercion.
- `Symbol.hasInstance`, `Symbol.species`, etc.

---

### Introspection

```js
Object.getOwnPropertySymbols(user); // [Symbol(id)]
Reflect.ownKeys(user); // ["name", Symbol(id)]
```

- Not returned by `Object.keys` or `for...in`.
- Returned by `Object.getOwnPropertySymbols` / `Reflect.ownKeys`.

---

### Gotchas ⚠️ (Exhaustive)

1. **Not auto-convertible to string**
   ```js
   "id:" + Symbol("x"); // ❌ TypeError
   String(Symbol("x")); // "Symbol(x)"
   ```
2. **Uniqueness**
   - Each `Symbol("desc")` is unique, regardless of description.
   - Use `Symbol.for()` to reuse across modules.
3. **Hidden, not private**
   - Symbols hide keys from normal iteration, but anyone with reference can access them.
   - Not true encapsulation (use `#private` fields in classes for that).
4. **JSON ignore**
   - `JSON.stringify({[Symbol("id")]:123})` → "{}".
   - Symbols dropped silently.
5. **Object property order**
   - Symbol keys appear after string keys in `Reflect.ownKeys`.
6. **Global registry leaks**
   - `Symbol.for()` uses global registry → can cause accidental sharing if names collide.
7. **Performance**
   - Overuse may slow down property lookups slightly (not optimized like string keys).
8. **Well-known symbols subtlety**
   - Some (like `Symbol.toPrimitive`) can cause surprising coercion behavior if misused.

---

### Best Practices

- ✅ Use symbols for **internal keys** or meta-properties.
- ✅ Use `Symbol.for` when you need **shared registry keys**.
- ✅ Use well-known symbols to customize built-in behavior (`iterator`, `toStringTag`, etc.).
- ❌ Don’t use Symbols as replacements for private fields.
- ✅ Document symbol usage clearly — they’re not visible via normal inspection.

---

## 👉 Mnemonic: **“Symbols = Unique keys, hidden from loops, but not truly private.”**

Would you like me to follow this with a **cheat card on Well-Known Symbols** (like `Symbol.iterator`, `Symbol.toStringTag`, etc.), since they’re often tested in interviews?
Here’s your **Concept Mastery Cheat Sheet** for **Well-Known Symbols** in JavaScript — focusing on the most common ones: `Symbol.iterator`, `Symbol.asyncIterator`, `Symbol.toStringTag` — with all **gotchas grouped together**.

---

# 🌀 JavaScript Well-Known Symbols — Cheat Card

**Concept:**
Special built-in `Symbol` values that let objects customize language behaviors.

---

### `Symbol.iterator`

```js
const arr = [1, 2, 3];
const it = arr[Symbol.iterator]();
console.log(it.next()); // {value:1,done:false}
for (const x of arr) console.log(x); // 1,2,3
const custom = {
  *[Symbol.iterator]() {
    yield 1;
    yield 2;
  },
};
[...custom]; // [1,2]
```

- Defines **synchronous iteration protocol**.
- Enables `for...of`, spread `...`, destructuring, etc.

---

### `Symbol.asyncIterator`

```js
const asyncIterable = {
  async *[Symbol.asyncIterator]() {
    yield "a";
    yield "b";
  },
};
for await (const x of asyncIterable) console.log(x); // a, b
```

- Defines **asynchronous iteration**.
- Enables `for await...of`.

---

### `Symbol.toStringTag`

```js
const obj = { [Symbol.toStringTag]: "Custom" };
Object.prototype.toString.call(obj); // "[object Custom]"
```

- Customizes default `Object.prototype.toString`.
- Used by many built-ins (`[object Array]`, `[object Map]`).

---

### Other Well-Known Symbols (mention)

- `Symbol.hasInstance` → customize `instanceof`.
- `Symbol.isConcatSpreadable` → control array concat.
- `Symbol.match`, `Symbol.replace`, `Symbol.search`, `Symbol.split` → regex behavior.
- `Symbol.toPrimitive` → customize type coercion.
- `Symbol.unscopables` → exclude props from `with` scope.
- `Symbol.species` → control derived constructors.

---

### Gotchas ⚠️ (Exhaustive)

1. **Not enumerable**
   - Well-known symbol props don’t show in `for...in` / `Object.keys`.
   - Must use `Object.getOwnPropertySymbols` or `Reflect.ownKeys`.
2. **Iterator protocol requirements**
   - `next()` must return `{value, done}`.
   - Forgetting `done:true` → infinite loop.
3. **Async iterator quirks**
   - Requires `for await...of`. Using `for...of` on async iterable → ❌ TypeError.
   - Works only in async functions.
4. **`toStringTag` only affects `Object.prototype.toString`**
   - Doesn’t change `typeof`.
   - Misleading if abused.
5. **Custom `[Symbol.iterator]` on non-iterables**
   - Enables `for...of` on arbitrary objects → can confuse maintainers.
6. **Cross-realm issues**
   - Different iframes/windows may not share constructors, but symbols themselves are global across realms.
7. **`Symbol.toPrimitive` vs `valueOf/toString`**
   - Precedence rules subtle. If all three defined, coercion picks in order.
8. **Overriding built-ins**
   - Can break expected behavior (e.g., setting `Symbol.isConcatSpreadable=false` on arrays).
9. **Performance**
   - Custom iterators may be slower than built-ins.

---

### Best Practices

- ✅ Use `Symbol.iterator` for custom data structures.
- ✅ Use `Symbol.asyncIterator` for streams, async data sources.
- ✅ Use `Symbol.toStringTag` only for debugging/logging clarity.
- ✅ Always implement iterator/asyncIterator correctly (return `{done:true}`).
- ❌ Don’t abuse well-known symbols to “hack” language behavior unexpectedly.
- ✅ Document when overriding built-ins with custom symbols.

---

## 👉 Mnemonic: **“iterator = sync loop, asyncIterator = async loop, toStringTag = identity label.”**

Would you like me to also prepare a **cheat card dedicated to Iterator vs Iterable vs AsyncIterable protocols** (since they’re often tested alongside these symbols)?

## Here’s your **Concept Mastery Cheat Sheet** for **Dates & the `Temporal` API** in JavaScript — covering legacy pitfalls and the new modern API, with **all gotchas grouped together**.

# 📅 JavaScript Dates & Temporal API — Cheat Card

**Concept:**

- **`Date`** (ES1) = legacy object for timestamps.
- **`Temporal`** (Stage 3 TC39) = modern, reliable API for dates/times (proposed replacement).

---

### Legacy `Date`

```js
const d1 = new Date(); // now
const d2 = new Date("2025-09-28"); // parse string (UTC vs local quirks)
const d3 = new Date(2025, 8, 28, 7, 30); // YYYY,MM(0-based),DD,HH,MM
d1.getFullYear(); // year
d1.getMonth(); // 0–11
d1.getDate(); // 1–31
d1.getDay(); // 0–6 (Sun–Sat)
d1.getTime(); // ms since epoch
d1.toISOString(); // UTC string
Date.now(); // epoch ms (fast)
```

- Stores as **ms since Jan 1 1970 UTC**.
- Methods split into local vs UTC (`getHours` vs `getUTCHours`).

---

### Temporal API (New)

```js
// Absolute times
const inst = Temporal.Now.instant(); // current exact instant
inst.toString(); // "2025-09-28T02:30:00Z"
// Calendar dates
const date = Temporal.PlainDate.from("2025-09-28");
date.add({ days: 1 }); // 2025-09-29
// Time + date
const dt = Temporal.PlainDateTime.from("2025-09-28T07:30");
dt.with({ hour: 10 }); // 2025-09-28T10:30
// Zoned
const zdt = Temporal.Now.zonedDateTimeISO();
zdt.toString(); // "2025-09-28T08:00:00+05:30[Asia/Kolkata]"
```

**Main Types:**

- `Temporal.Instant` → absolute point in time (like `Date`).
- `Temporal.PlainDate`, `PlainTime`, `PlainDateTime` → human-readable parts.
- `Temporal.ZonedDateTime` → date-time with explicit timezone.
- `Temporal.Duration` → span of time.
- `Temporal.Calendar`, `Temporal.TimeZone` → metadata.

---

### Gotchas ⚠️ (Exhaustive)

1. **`Date` quirks**
   - Months are **0-based** (`Jan = 0`).
   - Parsing inconsistent across engines/locales.
   - Timezone offsets implicit (local system).
   - Mutability: `date.setFullYear(...)` mutates object.
   - Leap seconds ignored.
   - Max safe range: ±8.64e15 ms (~±285,000 years).
2. **Temporal advantages**
   - Immutable objects (safe).
   - Timezones explicit.
   - Precise arithmetic (`date.add({days:1})`).
   - Built-in calendar support.
   - Consistent parsing (ISO 8601).
3. **Temporal not yet standard everywhere**
   - Still proposal (Stage 3). Needs polyfill (`proposal-temporal`).
   - Not available in all runtimes (as of 2025).
4. **Conversions**
   - `Date` ↔ `Temporal`:
     ```js
     Temporal.Instant.fromEpochMilliseconds(Date.now());
     new Date(Temporal.Now.instant().epochMilliseconds);
     ```
5. **Performance**
   - Temporal heavier than Date (more objects, immutability). Use `Date.now()` for perf-critical timestamps.
6. **Serialization**
   - JSON.stringify(new Date()) → ISO string.
   - Temporal not natively serialized; use `.toString()`.

---

### Best Practices

- ✅ Use `Date.now()` for quick timestamps.
- ✅ Use `Temporal` (polyfill) for new projects needing robust date/time.
- ✅ Always handle timezones explicitly (ZonedDateTime).
- ✅ Use `.toISOString()` for storage/serialization.
- ❌ Don’t rely on Date parsing of non-ISO strings.
- ❌ Don’t mutate Date objects unless necessary.

---

👉 Mnemonic:

- **Date = legacy, mutable, quirky**
- **Temporal = modern, immutable, precise**

---

Would you like me to prepare a **side-by-side “Date vs Temporal” comparison cheat card** (with use-cases, pros/cons, and migration tips)?

## Here’s your **Concept Mastery Cheat Sheet** for **Regular Expressions (RegExp)** in JavaScript — dense but clear, with **all gotchas grouped together**.

# 🔍 JavaScript Regular Expressions (RegExp) — Cheat Card

**Concept:**
RegExps are objects describing patterns for matching, searching, and replacing text.

---

### Creation

```js
const re1 = /abc/; // literal
const re2 = new RegExp("abc"); // constructor
```

---

### Flags

- `g` → global (all matches)
- `i` → case-insensitive
- `m` → multiline (`^/$` match line boundaries)
- `s` → dotAll (`.` matches newlines)
- `u` → Unicode mode (full code points)
- `y` → sticky (match from lastIndex only)

---

### Character Classes

- `.` any char (except newline unless `s`)
- `\d` digit `[0-9]`
- `\w` word `[A-Za-z0-9_]`
- `\s` whitespace
- `\D`, `\W`, `\S` = negations
- Custom: `[abc]`, ranges `[a-z]`, negated `[^a-z]`

---

### Quantifiers

- `*` 0+
- `+` 1+
- `?` 0/1
- `{n}`, `{n,}`, `{n,m}`
- Greedy by default → add `?` for lazy: `.+?`

---

### Anchors & Boundaries

- `^` start, `$` end
- `\b` word boundary, `\B` non-boundary

---

### Groups

- `(abc)` capturing
- `(?:abc)` non-capturing
- `(?<name>abc)` named capture
- Backrefs: `\1`, `\k<name>`

---

### Assertions

- Lookahead: `X(?=Y)` (followed by Y)
- Negative lookahead: `X(?!Y)`
- Lookbehind (ES2018+): `(?<=Y)X` / `(?<!Y)X`

---

### Methods

```js
/ab/.test("abc"); // true
"abc".match(/a./); // ["ab"]
"abc".match(/a./g); // ["ab"]
"abc".search(/b/); // 1
"abc".replace(/b/, "x"); // "axc"
"abc".split(/b/); // ["a","c"]
const r = /a/g;
r.exec("aab"); // {0:"a",index:0}
r.exec("aab"); // {0:"a",index:1} (g advances lastIndex)
```

---

### Gotchas ⚠️ (Exhaustive)

1. **Escaping hell**
   - In string regex: `new RegExp("\\d+")` (double escapes).
2. **lastIndex with `g`/`y`**
   - Statefulness can cause bugs if not reset.
   ```js
   const r = /a/g;
   r.test("a");
   r.test("a"); // true, false
   ```
3. **Greedy quantifiers**
   - `/.*/` matches whole string; add `?` for minimal match.
4. **Dot (`.`) limits**
   - Doesn’t match newlines unless `s` flag.
5. **Unicode quirks**
   - `"😀".length===2`, `/./.test("😀")` → true but matches half surrogate.
   - Use `u` flag: `/./u`.
6. **Lookbehind support**
   - Not in older engines.
7. **Performance traps**
   - Catastrophic backtracking with nested quantifiers:
     ```js
     /(a+)+$/ on "aaaaaaaaaaaaaaaaaaaa!"
     ```
   - Can hang program.
8. **Anchors in multiline**
   - `^` and `$` match per-line only with `m` flag.
9. **Replace pitfalls**
   - `$&` = matched substring, `$1` = group1, `$$` = literal $.
10. **Regex literals vs constructor**
    - Literal compiled once, constructor each time.
11. **JSON/stringify**
    - `JSON.stringify(/a/g)` → "{}" (not preserved).
12. **Empty regex**
    - `//` invalid. Use `new RegExp("")`.

---

### Best Practices

- ✅ Always use `u` flag for Unicode safety.
- ✅ Use named groups for readability.
- ✅ Reset `lastIndex` before reusing global/sticky regex.
- ✅ Escape user input when building dynamic regex.
- ✅ Avoid catastrophic patterns; use tools like [regex101.com](https://regex101.com/).
- ✅ Prefer non-capturing groups unless capture needed.

---

## 👉 Mnemonic: **“Regex = Pattern + Flags + State traps.”**

Would you like me to also prepare a **cheat card on RegExp vs String methods (`match`, `matchAll`, `replace`, `split`, `search`)** since many interview bugs come from mixing them up?

## Here’s your **Concept Mastery Cheat Sheet** for **RegExp Flags, Groups, and Lookarounds (Lookaheads & Lookbehinds)** in JavaScript — concise, deep, and with **all gotchas grouped together**.

# 🚩 JavaScript RegExp — Flags, Groups & Lookarounds

---

### 🔖 Flags

```js
/g  // global — find all matches
/i  // case-insensitive
/m  // multiline (^,$ = line boundaries)
/s  // dotAll (dot matches newline)
/u  // Unicode mode (full code points)
/y  // sticky (match only at lastIndex)
```

- Flags can be combined: `/abc/gi`.

---

### 🎭 Groups

```js
/(abc)/         // capturing group
/(?:abc)/       // non-capturing
/(?<name>abc)/  // named group
```

- Captures accessed in results:
  ```js
  /(a)(b)/.exec("ab"); // ["ab","a","b"]
  ```
- Named backref: `\k<name>`

---

### 🔄 Backreferences

```js
/(a)b\1/.test("aba"); // true
/(?<d>\d+)-\k<d>/.test("42-42"); // true
```

---

### 👁️ Lookaheads

```js
X(?=Y)   // positive lookahead
X(?!Y)   // negative lookahead
/\d(?=px)/.exec("10px"); // "0"
```

- Matches `X` if followed by `Y`.

---

### 👁️ Lookbehinds (ES2018+)

```js
(?<=Y)X  // positive lookbehind
(?<!Y)X  // negative lookbehind
/(?<=\$)\d+/.exec("$99"); // "99"
```

- Matches `X` if preceded by `Y`.

---

### Gotchas ⚠️ (Exhaustive)

1. **Flags**
   - `g` + `exec/test` are stateful (`lastIndex` must be reset).
   - `m` only affects `^`/`$`.
   - `s` needed for `.` to match `\n`.
   - `u` required for Unicode > BMP (e.g. emoji).
   - `y` requires match at `lastIndex` → fails otherwise.
2. **Groups**
   - Capturing groups slow regex if unused. Prefer `(?: )`.
   - Named groups not supported in very old browsers.
   - Unmatched groups return `undefined`, not `""`.
3. **Backrefs**
   - Reference numbering depends on left-to-right order of `(`.
   - Nested groups increase count; miscount = bugs.
4. **Lookaheads/Lookbehinds**
   - Zero-width → consume no characters.
   - Lookbehinds require fixed-width patterns in some engines.
   - Negative lookbehind/lookahead easy to misuse: `X(?!Y)` still matches `X`.
   - Catastrophic backtracking possible if nested inside quantifiers.
5. **Unicode quirks**
   - Without `u`, `.` and quantifiers can split surrogate pairs (emoji).
6. **Performance**
   - Heavy use of capturing/backrefs/lookarounds can degrade performance.

---

### Best Practices

- ✅ Use `u` + `s` when working with text containing Unicode & newlines.
- ✅ Prefer **named groups** for readability.
- ✅ Use `(?: )` for grouping without capture.
- ✅ Reset `lastIndex` when reusing `/g` or `/y` regex.
- ✅ Keep lookaheads/behinds simple (avoid nested quantifiers).
- ❌ Don’t rely on group numbers in large regex → use names.

---

👉 Mnemonic:

- **Flags = search mode**
- **Groups = capture or cluster**
- **Lookaround = check before/after without consuming**

---

Would you like me to also prepare a **cheat card just on “Regex Backtracking & Catastrophic Patterns”** (since interviewers often test efficiency issues with groups + lookarounds)?

## Here’s your **Concept Mastery Cheat Sheet** for **Sticky (`y`), Global (`g`), and DotAll (`s`) regex flags** in JavaScript — crisp, with **all gotchas grouped together**.

# 🚩 JavaScript RegExp Flags — Sticky, Global, DotAll

---

### 🔁 Global (`g`)

```js
const re = /a/g;
"a_a_a".match(re); // ["a","a","a"]
```

- Finds **all matches**.
- Advances `lastIndex` internally with `.exec()` or `.test()`.
- Works across whole string.

---

### 📍 Sticky (`y`)

```js
const re = /a/y;
re.lastIndex = 2;
"__a_a".match(re); // ["a"] (must match at index 2)
```

- Matches **only at `lastIndex`**.
- Fails if pattern not found exactly there.
- Good for tokenizers, incremental parsing.

---

### ⚫ DotAll (`s`)

```js
/hello.world/.test("hello\nworld"); // false
/hello.world/s.test("hello\nworld"); // true
```

- Makes `.` match **all characters**, including `\n`.
- Without `s`, dot excludes line breaks.

---

### Gotchas ⚠️ (Exhaustive)

1. **Global (`g`)**
   - `.exec()` and `.test()` are **stateful**:
     ```js
     const r = /a/g;
     r.test("a");
     r.test("a"); // true, false
     ```
   - Forgetting to reset `lastIndex` causes missed matches.
   - `String.match` with `g` → returns array of matches, **no capture groups**.
2. **Sticky (`y`)**
   - Requires `lastIndex` control. If `lastIndex` wrong → always fail.
   - Ignores matches not starting exactly at `lastIndex`.
   - Useful for step-by-step parsing but rarely needed for simple search.
   - Not widely known → less portable in older engines.
3. **DotAll (`s`)**
   - Changes meaning of `.` drastically → can make regex more expensive.
   - Without `s`, use `[^]` trick for “match anything” (but less readable).
   - Still respects Unicode if combined with `u`.
4. **Combining flags**
   - `/pattern/gs` → global + dotAll, safe for multi-line text.
   - `/pattern/my` → multiline + sticky works but tricky to debug.
5. **lastIndex pitfalls**
   - Both `g` and `y` mutate `lastIndex` when matching.
   - If you reuse regex object, reset manually: `re.lastIndex=0`.
6. **Performance**
   - Sticky can be faster for anchored matching (no backtracking before lastIndex).
   - DotAll may slow regex if pattern spans large text.

---

### Best Practices

- ✅ Use `g` when you need **all matches**, but don’t rely on capture groups with `match`.
- ✅ Use `matchAll` (`/pattern/g`) for captures:
  ```js
  [..."a1a2".matchAll(/a(\d)/g)];
  ```
- ✅ Use `y` for parsing/tokenization where position matters.
- ✅ Use `s` for multi-line text matching readability.
- ❌ Don’t forget `lastIndex` with `g`/`y`.
- ✅ Combine `gs` for “find everything, even across lines”.

---

👉 Mnemonic:

- **g = all matches**
- **y = must stick at lastIndex**
- **s = dot matches everything**

---

Would you like me to also prepare a **cheat card on `lastIndex` and stateful regex behavior** (since it’s the key confusion point with `g` and `y`)?
