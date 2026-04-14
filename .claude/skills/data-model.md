# Data Model Skills — Shared Domain Contract

## Core Entities

### User
```ts
{
  id: string          // UUID
  email: string       // unique, lowercase, trimmed
  passwordHash: string // never exposed in API responses
  name: string
  role: "admin" | "member"
  createdAt: Date
  updatedAt: Date
}
```

### Task
```ts
{
  id: string          // UUID
  title: string       // required, max 255 chars
  description: string | null
  status: "todo" | "in_progress" | "done" | "cancelled"
  priority: "low" | "medium" | "high"
  dueDate: Date | null
  ownerId: string     // FK → User.id
  assigneeId: string | null // FK → User.id
  createdAt: Date
  updatedAt: Date
}
```

## Naming Conventions

- All field names use `camelCase` in TypeScript/JSON (API layer).
- All column names use `snake_case` in the database (see `database.md`).
- The ORM/query layer is responsible for the translation — the API never leaks `snake_case` and the DB never stores `camelCase`.
- Date fields always include the suffix `At` (events: `createdAt`) or `Date` (deadlines: `dueDate`).
- Boolean fields use affirmative names: `isArchived`, `isActive` — not `archived`, `disabled`.

## Field Rules

- `id` fields are always UUIDs (v4), never auto-increment integers exposed to clients.
- `status` and `priority` are string enums — define them as TypeScript `const` enums or Zod `z.enum()`.
- Nullable fields must be explicitly typed as `T | null`, never `T | undefined` in the data layer.
- `passwordHash` (and any secret field) must be stripped from all API response types at the service boundary.

## Shared Types

- Define shared types in `/src/types/` or `/shared/types/`.
- Frontend and backend must import from the same type source — do not duplicate entity definitions.
- API response types live in `*.response.ts`, request/input types in `*.dto.ts`.
- Use Zod schemas as the single source of truth for validation shapes; derive TypeScript types from them with `z.infer<>`.

## Anti-Patterns

- Do not add fields to API responses that are not defined in the shared type. Avoid accidental data leakage.
- Do not use `any` or untyped objects to pass entity data between layers.
- Do not mix `null` and `undefined` semantics for "no value" — pick `null` for intentionally absent data.
- Do not change a field name in one layer without updating all other layers and the database migration.
- Do not embed full related entities in responses by default — use IDs and let the client fetch or join what it needs.
