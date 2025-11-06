```js
import express from "express";
// const express = require("express"); // CommonJS
const app = express();
app.use(express.json());

app.get("/health", (req, res) => res.json({ ok: true }));
app.post("/echo", (req, res) => res.json(req.body));

app.use((req, res) => res.status(404).json({ error: "not found" }));
app.use((err, _req, res, _next) =>
  res.status(500).json({ error: err.message })
);

app.listen(process.env.PORT || 3000, () =>
  console.log("http://localhost:3000")
);
```

Run:

```bash
npm i && npm start
```

Quick tests:

```bash
curl http://localhost:3000/health
curl -X POST http://localhost:3000/echo -H "content-type: application/json" -d '{"hi":1}'
```
