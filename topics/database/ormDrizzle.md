Drizzle ORM is a very light abstraction on top of SQL, and very simliar to it.

> If you know SQL, you know Drizzle. If not, you will have a hard time.

> Drizzle always outputs exactly 1 SQL query.

# Install

```bash
# Drizzle ORM
npm i drizzle-orm

# Database driver for postgreSQL
npm i pg

# Easier work with .env files
npm i dotenv

# -D = --save-dev

# Allows migrations
npm i -D drizzle-kit

# TypeScript Execute, a Node.js enhancement to run TypeScript. Think of tsx as an alias to node. `node file.js` -> `tsx file.ts`.
npm i -D tsx

# Typescript support for the pg driver
npm i -D @types/pg
```

# Project structure

```
drizzle/
    - node_modules/
    - src/
        - drizzle/
            - migrations/
            - migrate.ts
            - schema.ts
    - .env
    - drizzle.config.ts
    - package.json
    - tsconfig.json
```

# Config

Create a `drizzle.config.ts` file in the root directory.

```ts
import { defineConfig } from "drizzle-kit";

export default defineConfig({
    schema: "./src/drizzle/schema.ts",
    out: "./src/drizzle/migrations",
    dialect: "postgresql",
    dbCredentials: {
        url: process.env.DATABASE_URL as string,
    },
    verbose: true,
    strict: true,
});
```

# Schema

**schema.ts**

```ts
import { pgTable, varchar, uuid } from "drizzle-orm/pg-core";

export const UserTable = pgTable("user", {
    id: uuid("id").primaryKey().defaultRandom(),
    name: varchar("name", { length: 255 }).notNull(),
});
```

# Migrations

This generates all the `.sql` migration files.

```
npx drizzle-kit generate
```

Output from the schema above:

```sql
CREATE TABLE "user" (
	"id" uuid PRIMARY KEY DEFAULT gen_random_uuid() NOT NULL,
	"name" varchar(255) NOT NULL
);
```

This deletes all the migrations.

```
npm drizzle-kit drop
```

These can be added as `package.json` scripts.

```json
{
    "scripts": {
        "db:generate": "drizzle-kit generate",
        "db:drop": "drizzle-kit drop"
    }
}
```

# Connect

# Migrations
