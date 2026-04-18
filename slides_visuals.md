# שקפים - מ-Prompt מפלצתי למוח מודולרי

---

## שקף 1: כותרת הפתיחה

**כותרת ענקית:**
מ-Prompt מפלצתי למוח מודולרי: איך לבנות "ראוטר" שמונע מ-AI להזות קוד.

**תת-כותרת:**
3 שלבים פשוטים לסוכני קוד (Cursor/Claude) שעובדים כמו סיניורים.

---

## שקף 2: הבעיה - The Context Bloat (עומס קונטקסט)

- תמונה של קובץ CLAUDE.md או cursorrules — באורך הגלות, מלא בטקסט צפוף
- לידו אימוג'י: 🤯

---

## שקף 3: הפתרון - שיטת הראוטר (JIT Context)

**איור / דיאגרמה:**

```
תיקייה מרכזית
    ├──> Frontend
    ├──> Database
    └──> Security
```

**מילה מודגשת:** Just-In-Time Context

---

## שקף 4: שלב 1 ו-2 — הארכיטקט ופס הייצור

**כותרת:** 1. הארכיטקט | 2. פס הייצור

**צילום מסך — הפרומפט לקלוד:**
> "I am starting a new project... Don't write rules yet. Just propose a list of 3-4 Markdown files we need under .claude/skills/"

---

## שקף 5: שלב 3 — מכתירים את המלך (The Router)

**צילום מסך של קובץ CLAUDE.md (Routing Table)**

**מירקור על המשפט:**
> This file is a router. It contains no rules.

---

## שקף 6: הוכחה בשטח (The Smoke Test)

**צד שמאל — המשימה:**
> "Plan the backend architecture for Forgot Password... Do not write frontend code"

**צד ימין — לוגים מהטרמינל:**
```
Reading file: .claude/skills/backend.md
Reading file: .claude/skills/security.md
Reading file: .claude/skills/database.md
```

---

## שקף 7: התוצאה הסופית (רמת סיניור)

**צילום מסך — טבלת הפלט של קלוד:**

| החלטה | עיקרון |
|---|---|
| Token stored as SHA-256 | Never store secrets in plain text |
| Wrap in transaction() | Multi-step write must be atomic |

---

## שקף 8: סיכום ו-Call to Action

**3 דברים לזכור:**
1. תפסיקו לדחוף הכל לקובץ אחד.
2. תנו ל-AI לבנות את חלוקת הקבצים.
3. צרו Router מבוסס טריגרים ברורים.

*(אופציה: QR קוד לפרומפטים / מדריך בלינקדאין/גיטהאב)*
