# protoc-gen-ts-with-pg-query

Issue that I have encountered with protoc-gen-ts and [pg_query.proto][pg_query]
from [libpg_query][libpg_query].

## toObject() inferred type too long

This seems to happen when `declaration` is set to `true` in `tsconfig.json`.

```shell
npm run build:inferred

src/protoc-gen-ts/pg_query.ts:76418:9 - error TS7056: The inferred type of this node exceeds the maximum length the compiler will serialize. An explicit type annotation is needed.

76418         toObject() {
              ~~~~~~~~


Found 1 error in src/protoc-gen-ts/pg_query.ts:76418
```


[pg_query]:
  https://github.com/pganalyze/libpg_query/blob/15-latest/protobuf/pg_query.proto
[libpg_query]: https://github.com/pganalyze/libpg_query
[analyze-trace]: https://github.com/microsoft/typescript-analyze-trace
