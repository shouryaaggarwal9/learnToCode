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

**Phase 1 (Skeleton) — Units 1–5 COMPLETE, Unit 6 delivered, Unit 6 = PHASE 1 GATE unit.**

```
Files on disk (verified by git):
  ROADMAP.md, CONTEXT.md, PROGRESS.md, package.json, vite.config.ts, tsconfig.json,
  index.html, src/main.tsx, .gitignore, package-lock.json  all committed + pushed (origin/main)
node_modules: EXISTS. Dev server working.
Console: 500 on /src/main.tsx = vite:import-analysis can't resolve './App' (intentional
  dangling import; dies when Unit 6 lands). favicon.ico 404 (Phase 10 loose end).
```

### ⬅ Where we stopped

**Units 1–5 = DONE.** Ratings: U1=4, U2=3, U3=2, **U4 + U5 ratings still owed — ask.**
- U5 note: user ran the id-rename experiment honestly; it was BLOCKED (500 — code never
  executed, chain died at unresolved import first). Used to teach: you can't observe a runtime
  failure in code that never reaches runtime. Non-null assertion Q passed on 2nd attempt
  (a: `!` silences strict at that spot; b: no check-time error, crash at run-time).
  Nit corrected: tsc type-checks, never "compiles" (noEmit).

**Unit 6 = `src/App.tsx` — delivered. THIS UNIT CLOSES PHASE 1'S GATE** — after user's blind
rebuild, `npm run dev` should visibly render (text on screen, console clean except favicon).
Pending question:

> **You never write `App()`. Who calls it, when, and what does its return value physically
> become on the page?**

(Correct answer involves: React calls it — element tree walk, render/commit phases; JSX was
transformed by the Unit 2 plugin into function calls/objects, NOT strings; React builds/updates
real DOM nodes via its renderer; component returns a *description*, React is the one that
materialises it. Ties Units 2+3+5 together.)

After rebuild + answer + gate verified: fill U4/U5 ratings, log "Phase 1 gate PASSED" in
CONTEXT §4/§7 + ROADMAP checkbox, commit+push. Then **Phase 2, Unit 1: `src/styles/tokens.css`**.
Phase 2 = pure CSS grind, one file per turn, expect slower pace.

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
- `2026-09-23` Unit 4 complete: rebuild byte-identical, rating owed. First answer was verbatim
  copy of assistant's explanation — flagged; re-explained; second attempt in user's words passed.
  Commits now pushed to origin/main (user caught two "committed" claims without git proof).
- `2026-09-23` Unit 5 `src/main.tsx` delivered; awaiting blind rebuild.
- `2026-09-23` Unit 5 complete: rebuild confirmed via live console; `!`/strict question passed
  on 2nd attempt (a: `!` silences strict, b: crash at runtime, nothing at check time).
- `2026-09-23` Unit 6 `src/App.tsx` delivered — closes Phase 1 gate.

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
