# CONTEXT.md — Session handoff / reference

> **If you are an AI assistant reading this with no prior context: this file is your entire memory.**
> Read it fully before writing a single line of code. Then read `ROADMAP.md` for the full plan.
> **Do not re-plan. Do not scaffold. Do not build ahead.** Follow "Resume protocol" below.

Last updated: 2026-09-22 · Branch `main` · Repo: `fcf_opencode`

---

## 1. Mission

The user is rebuilding their **engineering foundation**. They have shipped complex projects
(Vite, Next.js, Tailwind, Supabase, MongoDB, Convex) but AI wrote most of it, so the base layer
never formed. This project exists to fix that, **not** to produce a polished product.

> "The stack does not matter, building foundations matter."

The deliverable is **the user's skill**, measured in rebuilds they can do unaided. Shipping an app
is a side effect.

---

## 2. HARD RULES for the assistant

These override every instinct you have. Breaking them ruins the exercise.

1. **ONE unit per turn.** One file, or one page + its CSS. Never two.
2. **Explain what and why** for everything you write — concept, trade-off, common trap.
   Explain at the level of someone rebuilding the mental model, not someone who wants to ship.
3. **Then stop and ask.** Hand the unit to the user to **rebuild blind** (delete file, close chat,
   rewrite from memory, no AI, no peeking). Do NOT continue to the next unit in the same turn.
4. **Ask exactly one diagnostic question** before advancing. If the answer is weak, stay on the
   unit or re-explain. Do not advance on a shaky answer.
5. **Never build ahead** — no scaffolding, no `npm create`, no generating files "to save time",
   no filling in TODOs, no lint/test/router libraries unless Principle 1 is explicitly amended.
6. **Never re-give the answer** after a blind rebuild unless the user explicitly asks. If they ask,
   say so plainly and remind them it costs them the exercise.
7. **Minimal dependencies** (Principle 1 in `ROADMAP.md`): v1 ships with only `react`,
   `react-dom`, `vite`, `typescript`, `@types/react`, `@types/react-dom`, `@vitejs/plugin-react`.
8. **Verify state with `git` before trusting any log here.** Files drift. Git does not lie.

The user chose this working loop explicitly, from three options — it was their decision, not yours.

---

## 3. Decisions locked in (do not re-litigate)

| Decision | Choice | Why |
|---|---|---|
| App | **`Recall`** — spaced-repetition study tracker | Useful to a student; offline-first is honest (bus/train/no signal); hardest parts are *derived state* (due dates, streaks, intervals) |
| Stack | **Vite + React 19 + TypeScript + plain CSS** | React keeps it career-relevant; **no Tailwind** so the cascade/box model/specificity can't be hidden |
| PWA | **Hand-written** manifest + service worker | No `vite-plugin-pwa` black box |
| Backend | **None in v1** | Auth/sync/APIs hide fundamentals behind network failures. Local-first (IndexedDB). |
| Working loop | **I build 1 unit → explain → user rebuilds blind → 1 question → advance** | Only mode that actually transfers skill |

Rebuild style was chosen over "I build, you extend", "you build, I review", and "I build + explain
only". Do not offer to switch unless the user raises it.

---

## 4. Current status

**Phase 1 (Skeleton) — Units 1–3 COMPLETE, Unit 4 delivered, awaiting user rebuild.**

```
Files on disk (verified by git):
  ROADMAP.md, CONTEXT.md, PROGRESS.md, package.json, vite.config.ts,
  tsconfig.json, .gitignore, package-lock.json  all committed
node_modules: EXISTS (npm install run during Unit 2 experiment).
Dev server verified starting (localhost 5173, blank — expected until index.html exists).
```

Phase 1 remaining after Unit 4: `src/main.tsx` → `src/App.tsx`. Gate: `npm run dev` renders a page.

### ⬅ Where we stopped

**Units 1–3 = DONE.**
- U1 `package.json` — rebuild byte-identical, confidence 4, caret/lockfile passed.
- U2 `vite.config.ts` — rebuild passed (style diff), confidence 3, rename experiment run live
  (silent fallback observed), convention question passed.
- U3 `tsconfig.json` — rebuild initially missing `target`/`lib`/`module`; user reasoned the
  three back without peeking, file now matches. Type-erasure question passed ("erased; not
  runtime data"). **User's confidence rating (1–5) for Unit 3 still owed** — ask.

**Unit 4 = `index.html` — delivered and explained.** Pending question the user owes:

> **Why is `index.html` the entry point of the app, and `src/main.tsx` is not? What does the
> browser actually request when you open the dev-server URL?**

(Correct answer involves: the browser only understands HTML/CSS/JS — it doesn't know React,
Vite, or modules-bundled-by-a-dev-server; it requests a URL and expects HTML back; `index.html`
is that document, and it *reaches* JS via a `<script>` tag. Vite inverts the classic bundler
model: HTML is the entry graph root, not JS. `main.tsx` only runs because index.html loaded it.
Bonus: dev server URL `/` maps to index.html by convention, like the vite.config naming lesson.)

After rebuild + answer: fill Unit 3 rating, commit, then **Unit 5: `src/main.tsx`**.
Expect the page to STILL be blank-ish after Unit 4 (empty #root div) — first visible render
comes with Unit 5/6. Don't panic; gate is at end of Phase 1.

**Housekeeping:** `.gitignore` (node_modules/, dist/) + `package-lock.json` committed.
User owes 2 numbers: Unit 3 confidence rating.

---

## 5. Versions in use (pulled from npm registry on 2026-09-22 — re-verify, don't assume)

| Package | Version |
|---|---|
| react / react-dom | 19.3.0 |
| vite | 8.3.0 |
| @vitejs/plugin-react | 6.1.1 |
| typescript | 7.0.2 |
| @types/react / @types/react-dom | 19.3.0 |

Note: TypeScript 7 is the native (Go) rewrite. If it causes tooling friction, pin down to 6.x and
record the change in §7.

---

## 6. Folder structure (exists now)

```
fcf_opencode/
├── ROADMAP.md          full plan: principles, phases, "done when" gates
├── CONTEXT.md          this file
├── PROGRESS.md         USER's rebuild log — user writes it, assistant reminds
├── package.json        Phase 1, Unit 1 ✅
├── public/icons/       Phase 10
├── src/
│   ├── styles/         Phase 2  tokens.css, base.css
│   ├── components/     Phase 2  Button, Shell
│   ├── pages/Today/    Phase 5
│   ├── pages/AddTopic/ Phase 6
│   ├── pages/TopicDetail/ Phase 8
│   ├── lib/            Phase 3+7  dates, scheduling, ids, router
│   ├── data/           Phase 9  db.ts, repo.ts
│   └── state/          Phase 4  store.ts
```

Full structure, all 12 phases, and every "done when" gate live in **`ROADMAP.md`**.

---

## 7. Decision / change log

Append here whenever a decision changes. Newest last.

- `2026-09-22` Project start. Repo initialised by user, `ROADMAP.md` committed.
- `2026-09-22` Locked: app = Recall, stack = Vite+React+TS+plain CSS, loop = rebuild blind.
- `2026-09-22` Build script set to `tsc --noEmit && vite build` (type-check as a pure gate) rather
  than the Vite template's `tsc -b`.
- `2026-09-23` Unit 1 complete: rebuild verified identical, confidence 4, diagnostic passed.
  `my.package.json` duplicate removed by user request.
- `2026-09-23` Unit 2 `vite.config.ts` delivered; awaiting blind rebuild.
- `2026-09-23` Unit 2 complete: rebuild passed (style-only diff), rename experiment run live by
  user (silent fallback observed), convention question passed. `.gitignore` + `package-lock.json`
  committed. `npm install` has now been run.
- `2026-09-23` Unit 3 `tsconfig.json` delivered; awaiting blind rebuild.
- `2026-09-23` Unit 3 complete: rebuild repaired from reasoning (3 missing fields restored,
  no peeking), type-erasure question passed. User rated U2 = 3.
- `2026-09-23` Unit 4 `index.html` delivered; awaiting blind rebuild.

---

## 8. Resume protocol (do this every new session)

1. Read this file, then `ROADMAP.md`.
2. Run `git status --short && git ls-files` and compare against §4. **Trust git over this file.**
3. Ask the user, briefly: *"Where are you — did you finish the Unit N rebuild and answer the
   question?"* Do not assume.
4. If they're mid-rebuild: wait, answer questions, do not write files.
5. If they're done: check `PROGRESS.md`, commit their work if uncommitted, ask for the answer,
   then deliver **exactly one** next unit.
6. Update §4 and append to §7 before the session ends, so the next one can resume cleanly.

**Tone:** the user opens messages with *"Radhe Radhe"* — greet in kind. Be warm, concise, and
teach with substance. No filler, no sycophancy, no dumping code the rules above say to withhold.
