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

**Phase 1 (Skeleton) — Units 1–4 COMPLETE, Unit 5 delivered, awaiting user rebuild.**

```
Files on disk (verified by git):
  ROADMAP.md, CONTEXT.md, PROGRESS.md, package.json, vite.config.ts,
  tsconfig.json, index.html, .gitignore, package-lock.json  all committed + pushed (origin/main)
node_modules: EXISTS. Dev server verified working.
Console (observed by user): 404 for /src/main.tsx (dangling script ref — resolves at Unit 5),
  404 for favicon.ico (harmless; Phase 10 loose end).
```

Phase 1 remaining after Unit 5: `src/App.tsx`. Gate: `npm run dev` renders a page.

### ⬅ Where we stopped

**Units 1–4 = DONE.** Ratings: U1=4, U2=3, U3=2, **U4 rating still owed — ask.**
- U4 note: first answer attempt was copy-pasted from assistant's own explanation — called out
  plainly. Assistant re-explained ("the relay, take two" — browser speaks one language, Vite
  translates). Second attempt, user's own words, passed. Lesson logged: when user can't answer,
  the fix is re-explain, not push. User pushed back correctly: "I have never read about it" —
  true, and the loop assumes explanation-first.

**Unit 5 = `src/main.tsx` — delivered and explained.** Imports `./App` (which doesn't exist
yet — same intentional dangling reference as U4's script tag; resolves in Unit 6). Pending question:

> **`getElementById('root')` can return `null`. What does the `!` after it *claim*, what does
> `strict: true` from tsconfig (your U3 unit) do with that claim, and what happens at runtime
> if the claim is false?**

(Correct answer involves: `!` = non-null assertion, a compile-time promise that erases to
nothing at runtime — same erasure story as U3; strictNullChecks forces you to face `|null`;
if the claim is false, `createRoot(null)` throws at runtime, page stays blank. Compile-time
guarantees are only as good as their enforcement, and `!` is manually bypassing it.)

After rebuild + answer: fill U4 rating, commit+push, then **Unit 6: `src/App.tsx`** — that's
the unit where the 404 dies and text finally renders. Phase 1 gate closes at Unit 6.

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
