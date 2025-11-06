16 mb
explain("executionStats")

```js
// driver
import { MongoClient } from "mongodb";
const client = new MongoClient(process.env.MONGO_URI);
await client.connect();
const db = client.db("app"); // choose DB
const users = db.collection("users"); // choose collection
```

Data Types (BSON → JS)

```js
await users
insertOne({ name: "A", });
insertMany([{...}, {...}], { ordered: false });
findOne({ _id: new ObjectId(id) });
const cursor = users.find(
  { age: { $gte: 18 } },
  { projection: { name: 1 }, sort: { age: -1 }, limit: 10, skip: 20 }
);
const rows = await cursor.toArray();
updateOne( {},{ $set: {} });
updateMany({}, { $unset: {} });

await users
  .aggregate([
    { $match: { } },
    { $project: { name: 1, year: { $year: "$createdAt" } } },
    { $group: { _id: "$year", count: { $sum: 1 } } },
    { $sort: { count: -1 } },
    { $limit: 5 },
      {
  $lookup: {
    from: "orders",           // target collection to join
    localField: "_id",        // field in current collection
    foreignField: "userId",   // field in target collection
    as: "orders"              // new array field name
  }
  $unwind: "$orders"
}
  ])
  .toArray();


bulkWrite([{ insertOne: { document: { name: "A" } } }]);
```
