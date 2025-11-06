## Timers

```js
setTimeout(fn, ms);
setInterval(fn, ms);
setImmediate(fn);
await sleep(100);
```

## FS (callbacks → promises)

```js
import fs from "fs";
import { readFile, writeFile, mkdir } from "fs/promises";

fs.readFile("a.txt", "utf8", (error, data) => {});

const d = await readFile("a.txt", "utf8");
await writeFile("b.json", JSON.stringify(obj));

fs.createReadStream("in").pipe(fs.createWriteStream("out"));
```

## Streams

- Types: Readable, Writable, Duplex, Transform

```js
import { pipeline } from "stream/promises";
await pipeline(
  fs.createReadStream("in"),
  zlib.createGzip(),
  fs.createWriteStream("out.gz")
);
for await (const chunk of readable) {
  /* handle each line */
}
```

## Buffer

Each byte = an integer from 0–255.

```js
const b = Buffer.from("hi");
b.toString("utf8"); // "hi"

Buffer.alloc(10); // 10 bytes of 0
b.length; // 2 bytes
```
