## Prisma Client メソッド集

```
node_modules/prisma/prisma-client/runtime/index.js

ModelAction2["findUnique"] = "findUnique";
ModelAction2["findFirst"] = "findFirst";
ModelAction2["findMany"] = "findMany";
ModelAction2["create"] = "create";
ModelAction2["createMany"] = "createMany";
ModelAction2["update"] = "update";
ModelAction2["updateMany"] = "updateMany";
ModelAction2["upsert"] = "upsert";
ModelAction2["delete"] = "delete";
ModelAction2["deleteMany"] = "deleteMany";
ModelAction2["groupBy"] = "groupBy";
ModelAction2["count"] = "count";
ModelAction2["aggregate"] = "aggregate";


var actionOperationMap = {
  findUnique: "query",
  findFirst: "query",
  findMany: "query",
  count: "query",
  create: "mutation",
  createMany: "mutation",
  update: "mutation",
  updateMany: "mutation",
  upsert: "mutation",
  delete: "mutation",
  deleteMany: "mutation",
  executeRaw: "mutation",
  queryRaw: "mutation",
  aggregate: "query",
  groupBy: "query"
};
```
