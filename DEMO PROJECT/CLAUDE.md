# CLAUDE.md — Agent Router

This file is a router. It contains no rules. All rules live in `.claude/skills/`.

**Before writing or modifying any code, you must:**
1. Identify the domain of your task (see table below).
2. Read the corresponding skill file in full.
3. Follow its rules for the duration of the task.

If your task spans multiple domains, read all relevant skill files before starting.

---

## Domain Routing Table

### Frontend → [.claude/skills/frontend.md](.claude/skills/frontend.md)
Read this file when touching anything inside `src/` on the client side:
- Creating or editing a React component (`.tsx` files)
- Managing state (`useState`, `useReducer`, Zustand store)
- Writing a custom hook (`use*.ts`)
- Fetching data from the API (React Query, service layer)
- Building or modifying a form (`react-hook-form`)
- Configuring routes (`routes.tsx`, `<PrivateRoute>`)
- Applying styles (Tailwind classes, CSS Modules)

### Backend → [.claude/skills/backend.md](.claude/skills/backend.md)
Read this file when touching anything inside the server `src/` directory:
- Creating or editing an Express router (`*.router.ts`)
- Writing a service function (`*.service.ts`)
- Adding or changing middleware (`auth`, `errorHandler`, `validate`)
- Designing a new API endpoint (method, path, response shape)
- Handling errors (`AppError`, global error handler)
- Configuring the Express app (`app.ts`)

### Security → [.claude/skills/security.md](.claude/skills/security.md)
Read this file whenever your task touches trust boundaries:
- Anything involving login, logout, registration, or session handling
- JWT creation, verification, or refresh token logic
- Password hashing or comparison
- CORS configuration
- Setting HTTP response headers (`helmet`, CSP, `X-Frame-Options`)
- Rate limiting any endpoint
- Handling file uploads
- Storing or accessing secrets or environment variables

### Data Model → [.claude/skills/data-model.md](.claude/skills/data-model.md)
Read this file when defining or consuming the shape of any entity:
- Adding or renaming a field on `User`, `Task`, or any future entity
- Writing a Zod schema for request validation or response typing
- Creating a DTO (`*.dto.ts`) or response type (`*.response.ts`)
- Sharing a type between the frontend and backend
- Deciding whether a field should be `null`, `undefined`, or required

### Database → [.claude/skills/database.md](.claude/skills/database.md)
Read this file when touching the data persistence layer:
- Editing `schema.prisma`
- Creating or modifying a migration file
- Writing a Prisma query (`findMany`, `create`, `update`, `delete`, `transaction`)
- Adding or removing a database index
- Changing a foreign key relationship
- Managing the Prisma client instance

---

## Rules for Using This Router

- Do not rely on memory or assumptions about project conventions. Always read the skill file.
- Do not skip the skill file because the task seems small. One-line changes must also follow the rules.
- Do not modify skill files during a task. If a rule seems wrong, flag it to the user — do not silently work around it.
