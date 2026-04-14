# Backend Skills — Node.js / Express

## Project Structure

- Organize by feature, not by layer. Prefer `/src/tasks/`, `/src/users/` over `/src/controllers/`, `/src/models/`.
- Each feature folder contains: `router.ts`, `service.ts`, `validation.ts`, and optionally `types.ts`.
- Entry point (`app.ts`) only mounts routers and global middleware. No business logic there.

## API Design

- Follow REST conventions: `GET /tasks`, `POST /tasks`, `GET /tasks/:id`, `PATCH /tasks/:id`, `DELETE /tasks/:id`.
- Use `PATCH` for partial updates, `PUT` only for full replacement.
- Return consistent response shapes:
  - Success: `{ data: <payload> }`
  - Error: `{ error: { message: string, code?: string } }`
- Use appropriate HTTP status codes. Never return `200` for an error.

## Middleware

- Apply middleware in this order: security headers → CORS → body parser → auth → routes → error handler.
- Authentication middleware must run before any protected route handler.
- Use a single centralized async error handler. All async route handlers must be wrapped or use `express-async-errors`.

## Validation

- Validate all incoming request bodies, query params, and path params using Zod.
- Validation happens in the router layer before the service layer is called.
- Never trust client-provided IDs or roles without re-verification from the database/token.

## Service Layer

- Business logic lives exclusively in the service layer (`*.service.ts`).
- Services are pure functions or classes — they do not access `req`/`res` directly.
- Services call the database layer (repositories/ORM), not raw SQL inline.
- Services throw typed errors; routers/middleware catch and format them.

## Error Handling

- Define a custom `AppError` class with `message`, `statusCode`, and optional `code`.
- Use a global Express error handler middleware as the last middleware registered.
- Never expose internal error details (stack traces, SQL errors) to the client in production.
- Log errors server-side with context (user ID, route, timestamp).

## Async/Await

- Always use `async/await`. No raw `.then()/.catch()` chains in route handlers.
- Always `await` Promises — never fire-and-forget unless intentional (log it if so).

## Anti-Patterns

- Do not put business logic in route handlers — only orchestration (call service, return response).
- Do not return `null` from a service when "not found" — throw a typed `NotFoundError`.
- Do not use `console.log` for application logging. Use a structured logger (e.g., `pino`).
- Do not hardcode environment-specific values. All config comes from environment variables via a config module.
- Do not catch errors silently. Every `catch` block must either rethrow, log, or handle explicitly.
