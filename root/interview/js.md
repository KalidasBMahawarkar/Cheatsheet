- Strings: `'a'`, `"a"`, `` `a ${x}` ``
- Numbers: `0, 1_000, 0b1010, 0o755, 0xFF, 1.2e3, NaN, Infinity`
- Boolean: `true | false`
- Null/Undefined: `null`, `undefined`
- Symbols: `Symbol("id")`
- BigInt: `123n`, `BigInt("123")`

- Objects: `{a:1, b}`, computed key: `{[k]:v}`, shorthand method: `{f(){}}`
- Arrays: `[1,2, ...rest]`
- RegExp: `/ab+c/gi`
- JSON: `JSON.parse(str)`, `JSON.stringify(x)`

- typeof x
- Array.isArray(a)
- Instance: `x instanceof C`
- Optional chaining: `obj?.a?.()`
- Nullish coalescing: `x ?? y`
- Coalesce chain: `a ?? b ?? c`

```js
if (cond) { } else if (c) { } else { }
switch (x) { case 1: ...; break; default: ... }
for (let i=0;i<n;i++) { }
for (const k in obj) { }        // keys (enumerable)
for (const v of iterable) { }   // values
while (cond) { } do { } while (cond)
break; continue; label: for(...) { break label }
```

```js
try {
} catch (e) {
} finally {
}
```

# 6) Functions

- Declared: `function f(a,b){ return a+b }`
- Expression: `const f = function(a,b){ }`
- Arrow: `const f = (a,b) => a+b` (lexical `this`, no `arguments`)
- Generator: `function* g(){ yield 1 }`
- Async: `async function f(){ return 1 }`
- Async arrow: `const f = async () => await p`
- IIFE: `(() => { /*...*/ })()`
