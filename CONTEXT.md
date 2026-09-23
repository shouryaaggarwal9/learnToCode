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

**Phase 1 (Skeleton) — Unit 1 of ~6 COMPLETE, Unit 2 delivered, awaiting user rebuild.**

```
Files on disk (verified by git):
  ROADMAP.md, CONTEXT.md, PROGRESS.md, package.json  all committed
Everything else: empty directories only.
node_modules: does not exist. `npm install` has NOT been run.
```

Phase 1 remaining after Unit 2: `tsconfig.json` → `index.html` → `src/main.tsx` →
`src/App.tsx`. Phase 1's gate is `npm run dev` rendering a page.

### ⬅ Where we stopped

**Unit 1 = `package.json` — DONE.** Blind rebuild matched byte-for-byte, rated confidence 4,
caret/lockfile question answered correctly, logged in `PROGRESS.md`, committed.
(A stray `my.package.json` duplicate was deleted, at user's instruction.)

**Unit 2 = `vite.config.ts` — delivered and explained.** The user has **not yet** confirmed their
blind rebuild. The pending question they owe an answer to:

> **Why does the config file live at the project root and get loaded automatically, and what
> breaks if you rename it or move it into `src/`?**

(Correct answer involves: Vite (and the bundler world generally) uses convention-over-configuration
— it looks for `vite.config.{ts,js,mjs,cjs}` at the root; rename/move it and Vite falls back to
zero-config defaults, silently losing the React plugin — which means JSX stops compiling / HMR
breaks — plus `root`/`base` customisations vanish. No error is thrown just for the missing config.)

After the rebuild + answer: log it in `PROGRESS.md`, commit, then **Unit 3: `tsconfig.json`**.

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
