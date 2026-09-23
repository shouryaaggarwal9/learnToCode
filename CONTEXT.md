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

**Phase 1 — ✅ GATE PASSED (2026-09-23). Phase 2 IN PROGRESS — Unit 1 delivered, awaiting rebuild.**

```
Files on disk (verified by git, pushed to origin/main):
  Phase 1 complete set: package.json, vite.config.ts, tsconfig.json, index.html,
  src/main.tsx, src/App.tsx + .gitignore, package-lock.json, ROADMAP/CONTEXT/PROGRESS.md
node_modules: exists. `npm run dev` renders "Recall" heading + paragraph. Verified by user.
Console: favicon.ico 404 = PERMANENT & EXPLAINED (browser auto-requests /favicon.io when no
  <link rel="icon"> present; dies in Phase 10 when we ship icon + link tag). Not a bug.
```

### ⬅ Where we stopped

**Units 1–6 = DONE.** Ratings: U1=4, U2=3, U3=2, U4=3, U5=3, **U6 rating still owed — ask.**
- U6 notes: rebuild structure correct, 1 typo (repetition), self-fixed from diff. Who-calls-App
  answered correctly ONLY after assistant re-explained ("take two": Vite = printing press, not
  reader; `<App />` = instruction card, not a call; React calls App inside render(); react-dom
  creates real DOM nodes). User's final answer had right actor (render/React calls App) but
  garbled material journey ("jsx to object to js", "adds to index.html" not #root, bystander
  unnamed). ASSISTANT GAVE PRECISION CORRECTIONS as part of verdict and opened the gate —
  treated as vocabulary gaps, not model gaps. If next phase shows this wobbling, revisit.
- Favicon settled by Network tab: status 404, user observed directly (earlier "it vanished"
  report was a misread console — lesson logged: contradicted observations get re-checked, not averaged).

**Phase 2 = pure CSS, one file or one page+CSS per unit. Unit 1 = `src/styles/tokens.css`
— delivered.** NOT imported anywhere yet — defined-but-unreferenced = zero effect (deliberate
inverse of Phase 1's dangling-reference errors). Pending question:

> **Why are spacing tokens written in `rem` instead of `px`? What is `rem` anchored to, and
> what user setting does it respect that `px` ignores?**

(Correct answer involves: rem = root <html> font-size (default 16px), user browser font-size
setting changes it → whole layout scales; px is absolute, ignores user preference → a11y
failure for low-vision users. Ties to ROADMAP Phase 11 accessibility pass.)

After rebuild + answer + U6 rating: commit+push, then **Phase 2 Unit 2: `src/styles/base.css`**.
Phase 2 remaining: base.css → Button.{tsx,css} → Shell.{tsx,css}. Gate: responsive app shell,
no framework CSS.

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
- `2026-09-23` **Phase 1 GATE PASSED** — page renders, all 6 units logged (ratings 4,3,2,3,3,-).
  Favicon 404 confirmed permanent + explained. Phase 2 begins; U4/U5 rated 3 each.
- `2026-09-23` Phase 2 Unit 1 `src/styles/tokens.css` delivered; awaiting blind rebuild.

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
