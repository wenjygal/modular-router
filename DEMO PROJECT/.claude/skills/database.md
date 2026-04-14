# Database Skills

## Technology Choices

- Use PostgreSQL as the primary database.
- Use Prisma as the ORM for schema definition, migrations, and type-safe queries.
- Never write raw SQL unless Prisma cannot express the query — and document why when you do.

## Schema Design

- All tables use `snake_case` column names (Prisma maps these to `camelCase` in TypeScript).
- Every table has: `id UUID PRIMARY KEY DEFAULT gen_random_uuid()`, `created_at TIMESTAMPTZ NOT NULL DEFAULT now()`, `updated_at TIMESTAMPTZ NOT NULL DEFAULT now()`.
- Use foreign key constraints on all relational columns. Do not rely on application-level integrity alone.
- Use `NOT NULL` by default. Allow `NULL` only when absence of a value is genuinely meaningful.
- Avoid storing arrays or JSON blobs for data that will be queried or filtered — normalize it.

## Migrations

- Every schema change requires a migration file. Never modify the database schema by hand in any environment.
- Migration filenames are descriptive: `20240101_add_task_priority_column.sql` or via `prisma migrate dev --name add_task_priority`.
- Migrations must be reversible where possible. If a migration is destructive (drop column, drop table), add a comment explaining the rollback strategy.
- Never edit an already-applied migration. Create a new one.
- Run migrations in CI before tests. Never deploy code that requires a migration that hasn't been applied.

## Indexing

- Add an index on every foreign key column.
- Add an index on columns used in `WHERE` clauses in frequent queries (e.g., `status`, `owner_id`, `due_date`).
- Add a unique index instead of a unique constraint when you need a partial unique index.
- Do not add indexes speculatively — index based on actual query patterns.

## Queries

- Use Prisma's `select` to fetch only the columns you need. Never `findMany()` with no field selection on large tables.
- Use `findUniqueOrThrow` / `findFirstOrThrow` instead of null-checking the result manually.
- Paginate all list queries. Never return unbounded result sets. Use cursor-based pagination for large datasets.
- Wrap multi-step writes that must succeed or fail together in a `prisma.$transaction()`.

## Connection Management

- Use a single Prisma client instance (singleton pattern) across the application.
- In serverless or test environments, handle client instantiation to avoid connection pool exhaustion.
- Set explicit connection pool limits via the `DATABASE_URL` connection string parameters.

## Anti-Patterns

- Do not run migrations inside the application startup code in production.
- Do not use `prisma.executeRaw` with string interpolation — use tagged template literals (`Prisma.sql`).
- Do not delete or truncate tables in a migration that is applied to production without a backup and a tested rollback plan.
- Do not store secrets, tokens, or passwords in plain text columns.
- Do not use `updated_at` as a concurrency control mechanism — use optimistic locking with a `version` integer column if needed.
- Do not ignore Prisma query warnings about N+1 patterns — resolve them with `include` or batch queries.
