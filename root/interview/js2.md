```js
const a = [1,2,3];
a.push(x);
a.pop();
a.shift() removes first element;
a.unshift(); adds to first;
a.slice(s,e); creates a new array;s
a.splice(s,c,...ins); removes and replaces elements;
a.map(f); a.filter(f); a.reduce(f, init);
a.find(f); a.findIndex(f); a.some(f); a.every(f);
a.includes(x); a.indexOf(x);
a.sort((x,y)=>x-y); a.reverse(); creates a new array;
a.entries(); a.keys(); a.values();
Array.from(iter|len, map?); Array.of(...xs); creates a new array;
```

- String core:

```js
"s".length;
s.includes("x");
s.indexOf("x");
s.startsWith(p);
s.endsWith(p);
s.slice(i, j);
s.substring(i, j);
s.replace(pat, rep);
s.replaceAll(a, b);
s.split(sep);
s.trim();
s.padStart(n, pad);
s.padEnd(n, pad);
s.toUpperCase();
s.toLowerCase();
[...s];
s.normalize();
```

- Map/Set:

```js
m = new Map([["k", 1]]);
m.set(k, v);
m.get(k);
m.has(k);
m.delete(k);
m.size;
m.clear();
for (const [k, v] of m) {
}
const s = new Set([1, 2]);
s.add(v);
s.has(v);
s.delete(v);
s.size;
for (const v of s) {
}
```
