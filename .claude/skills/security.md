# Security Skills

## Authentication

- Use JWT for stateless auth. Store tokens in `httpOnly`, `Secure`, `SameSite=Strict` cookies — never in `localStorage`.
- Access tokens should be short-lived (15 minutes). Use refresh tokens with rotation for session persistence.
- Always verify the JWT signature server-side on every protected request. Never trust the payload without verification.
- Hash passwords with `bcrypt` (minimum 12 rounds). Never store or log plaintext passwords.

## Authorization

- Enforce authorization in the service layer, not just at the route level.
- Every task/resource operation must verify that the requesting user owns or has permission for that resource.
- Do not expose other users' data based on sequential IDs — use UUIDs for resource identifiers.
- Apply the principle of least privilege: each role gets only the permissions it needs.

## Input Validation & Injection Prevention

- Validate and sanitize all user input before it reaches the database or business logic (see `backend.md`).
- Use parameterized queries or ORM methods exclusively. Never concatenate user input into SQL strings.
- Escape HTML output on the frontend to prevent stored XSS. Do not use `dangerouslySetInnerHTML` with user content.
- Validate file uploads: check MIME type, file size, and extension. Never execute uploaded content.

## HTTP Security Headers

- Use `helmet` middleware on all Express responses.
- Set a strict `Content-Security-Policy` that disallows inline scripts.
- Enable `X-Frame-Options: DENY` to prevent clickjacking.
- Use `Referrer-Policy: no-referrer` or `strict-origin-when-cross-origin`.

## CORS

- Whitelist only known origins. Never use `origin: '*'` in production.
- Only allow the HTTP methods the API actually uses.
- Credentials (`withCredentials`) must be paired with a specific, non-wildcard origin.

## Rate Limiting & Abuse Prevention

- Apply rate limiting to all auth endpoints (`/login`, `/register`, `/refresh`).
- Apply a general rate limit to all API routes.
- Use `express-rate-limit` with a Redis store in production (not in-memory).
- Return `429 Too Many Requests` with a `Retry-After` header on limit breach.

## Sensitive Data

- Never log passwords, tokens, credit card numbers, or PII.
- Never include sensitive fields in API responses unless explicitly required.
- Use environment variables for all secrets. Never commit `.env` files or secrets to git.
- Rotate secrets if they are ever accidentally exposed.

## Dependencies

- Pin dependency versions in `package.json`. Run `npm audit` in CI.
- Do not install packages without reviewing their purpose and reputation.
- Keep dependencies up to date. Address high/critical CVEs within one sprint.

## Anti-Patterns

- Do not trust `req.body.role` or `req.body.userId` — derive identity from the verified JWT only.
- Do not use `eval()`, `new Function()`, or `child_process.exec()` with user-supplied input.
- Do not return stack traces or internal error messages to the client in production.
- Do not disable HTTPS for "convenience" in staging environments that share production data.
- Do not roll your own crypto. Use established libraries (`bcrypt`, `jsonwebtoken`, `crypto`).
