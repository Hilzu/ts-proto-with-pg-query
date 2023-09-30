# ts-proto-with-pg-query

Two issues that I have encountered with ts-proto and [pg_query.proto][pg_query]
from [libpg_query][libpg_query].

## ts-proto generated code causes a heap overflow with tsc

Latest typescript fails to compile the generated code with a heap overflow.
Using the `--generateTrace` option and
[@typescript/analyze-trace][analyze-trace] tool this seems to be caused by the
`fromPartial` function for `Node` message. Using
`--ts_proto_opt=outputPartialMethods=false` fixes the heap overflow.

```sh
npm run build:overflow

FATAL ERROR: Ineffective mark-compacts near heap limit Allocation failed - JavaScript heap out of memory
```

## ts-proto creates invalid code for pg_query.proto

pg_query contains messages called String and Boolean. ts-proto creates objects
with the same names in the generated code and this causes the `String()` and
`Boolean()` global functions to be shadowed. This breaks the conversion to these
types that ts-proto generates.

```sh
npm run build:invalid

pg_query.ts:13254:41 - error TS2349: This expression is not callable.
  Type '{ encode(message: String, writer?: Writer): Writer; decode(input: Uint8Array | Reader, length?: number | undefined): String; fromJSON(object: any): String; toJSON(message: String): unknown; }' has no call signatures.

13254     return { fval: isSet(object.fval) ? String(object.fval) : "" };
                                              ~~~~~~

pg_query.ts:13302:47 - error TS2349: This expression is not callable.
  Type '{ encode(message: Boolean, writer?: Writer): Writer; decode(input: Uint8Array | Reader, length?: number | undefined): Boolean; fromJSON(object: any): Boolean; toJSON(message: Boolean): unknown; }' has no call signatures.

13302     return { boolval: isSet(object.boolval) ? Boolean(object.boolval) : false };
                                                    ~~~~~~~

```

[pg_query]:
  https://github.com/pganalyze/libpg_query/blob/15-latest/protobuf/pg_query.proto
[libpg_query]: https://github.com/pganalyze/libpg_query
[analyze-trace]: https://github.com/microsoft/typescript-analyze-trace
