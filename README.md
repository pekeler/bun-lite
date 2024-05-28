The DB should be closable after running migrations. This demonstrates it's not.

This project is based on https://bun.sh/guides/ecosystem/drizzle with three changes:

- the migration was generated with `bunx drizzle-kit generate --dialect sqlite --schema ./schema.ts` (`generate:sqlite` doesn't work with the current version of drizzle-kit)
- in migrate.ts `await` was removed (because `migrate` isn't an sync function)
- in migrate.ts `sqlite.close(true);` was added to the end to demonstrate the problem

```
$ bun run migrate.ts
4 | import { Database } from "bun:sqlite";
5 |
6 | const sqlite = new Database("sqlite.db");
7 | const db = drizzle(sqlite);
8 | migrate(db, { migrationsFolder: "./drizzle" });
9 | sqlite.close(true);
    ^
error: database is locked
      at /private/tmp/bun-lite/migrate.ts:9:1

Bun v1.1.10 (macOS arm64)
$
```
