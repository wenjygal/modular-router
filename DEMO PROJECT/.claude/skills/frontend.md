# Frontend Skills — React

## Component Structure

- Use functional components with hooks only. No class components.
- One component per file. File name matches the exported component name (PascalCase).
- Co-locate styles, tests, and sub-components in a feature folder, not a shared `components/` dumping ground.
- Keep components under ~150 lines. If longer, extract sub-components or custom hooks.

## State Management

- Use `useState` for local UI state. Use `useReducer` when state transitions are complex or interdependent.
- Lift state only as high as needed — no higher.
- For shared/global state (e.g., current user, task filters), use React Context or a lightweight store (Zustand preferred over Redux for this project size).
- Never store derived data in state. Compute it during render or with `useMemo`.

## Data Fetching

- All API calls go through a dedicated service layer (`/src/services/`), not inline in components.
- Use React Query (`@tanstack/react-query`) for server state: caching, loading, and error states.
- Never fetch data inside `useEffect` directly in a component — use a custom hook or React Query.
- Always handle loading and error states explicitly in the UI.

## Forms

- Use `react-hook-form` for all forms. No controlled inputs with manual `onChange` + `useState` unless trivial (1-2 fields).
- Validate on submit and on blur. Surface validation errors inline next to the field, not in a toast.
- Disable the submit button while submitting. Re-enable on error.

## Routing

- Use React Router v6. Define routes in a central `routes.tsx` file.
- Protect authenticated routes with a `<PrivateRoute>` wrapper component.
- Use `useNavigate` for programmatic navigation, not `window.location`.

## Styling

- Use CSS Modules or Tailwind CSS (pick one and stay consistent across the project).
- No inline `style={{}}` props except for truly dynamic values (e.g., width from a calculation).
- No global CSS side effects in component files.

## Anti-Patterns

- Do not call APIs directly from event handlers — go through the service layer.
- Do not use `any` type in TypeScript. Prefer `unknown` with a type guard if the shape is uncertain.
- Do not spread props blindly (`<Component {...props} />`). Be explicit about what is passed.
- Do not use array index as `key` in lists that can be reordered or filtered.
- Do not mutate state directly — always return a new reference.
