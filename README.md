# 🧠 AI Modular Router Wizard

Stop stuffing your `CLAUDE.md` or `.cursorrules` with hundreds of lines of rules.
It causes **Context Bloat** — and makes your AI hallucinate.

Instead, build a **Modular Router**.
This zero-code Wizard uses an AI State Machine to automatically structure your project's AI memory into focused domains (Frontend, DB, Security, etc.) and creates a central Router that tells the AI exactly which file to read and when.

---

## 🚀 How to use (30 seconds)

1. Download `modular-wizard.md` and drop it into your project root.
2. Open Claude Code (or Cursor Chat) and type:

   > "Read modular-wizard.md and start"

3. The AI will guide you step-by-step:
   - Proposes domains based on your actual stack
   - Waits for your approval before proceeding
   - Creates `.claude/skills/<domain>.md` with senior-level rules
   - Builds your `CLAUDE.md` Router

---

## 💡 How it works

This wizard forces LLMs to act as a **State Machine**.

It injects `<mental_note>` tags into the chat history at the end of every response. The AI reads its own last note before proceeding — preventing it from rushing ahead and enforcing a **human-in-the-loop approval at every step**.

No npm install. No scripts. Runs entirely on the LLM's comprehension engine.

---

## 📁 What gets created

```
your-project/
├── CLAUDE.md                    ← The Router (domain → skill file mapping)
└── .claude/
    └── skills/
        ├── frontend.md          ← Rules for React/Vue/UI work
        ├── backend.md           ← Rules for API/server work
        ├── database.md          ← Rules for schema and queries
        └── security.md          ← Rules for auth, secrets, headers
```

---

## 🧪 Example output (Router file)

```markdown
# CLAUDE.md — Agent Router

Before writing or modifying any code, identify your domain and read the corresponding skill file in full.

| Trigger | Skill File |
|---|---|
| Editing `.tsx` files, React components, hooks, forms | `.claude/skills/frontend.md` |
| Editing Express routers, services, middleware | `.claude/skills/backend.md` |
| Editing `schema.prisma`, Prisma queries, migrations | `.claude/skills/database.md` |
| Touching auth, JWT, CORS, headers, secrets | `.claude/skills/security.md` |
```

---

## ✨ Why this matters

| Monolithic `.cursorrules` | Modular Router |
|---|---|
| 500+ lines loaded on every prompt | Only the relevant domain is loaded |
| AI ignores rules it can't fit in context | Focused rules → higher compliance |
| Hard to maintain or update | Edit one file, not everything |
| Generic rules for all tasks | Task-specific, senior-level constraints |

---

## 📄 License

MIT — fork it, adapt it, use it in your own workshop.
