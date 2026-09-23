# Recall — Roadmap

A spaced-repetition study tracker. Built file-by-file to rebuild the builder's foundation.

> **Why this app:** it is genuinely useful to a student, it justifies offline-first honestly
> (bus / train / no signal), and its hardest parts — due dates, streaks, intervals — are
> *derived state*, which is exactly the concept most skipped when AI writes the code.

---

## Working agreement

1. **One unit per turn.** One file (or one page + its CSS). Nothing else gets written.
2. **I explain what and why** — the concept, the trade-off, the trap.
3. **You rebuild it blind.** Delete the file, close the tab, write it again from memory.
4. **Then you answer one question** before we move on. If the answer is shaky, we stay.
5. **No AI in your rebuild.** Cheating here only re-creates the problem we're fixing.

Rebuilds are logged in `PROGRESS.md` — one line per unit: date, unit, confidence (1–5).

---

## Principles (the non-negotiables)

| # | Principle | Meaning |
|---|-----------|---------|
| 1 | **Minimal dependencies** | `react`, `react-dom`, `vite`, `typescript`, plugin. That's all of v1. No router lib, no state lib, no date lib, no PWA plugin. We write them. |
| 2 | **Plain CSS** | No Tailwind. You must meet the cascade, the box model, specificity and the grid head-on. |
| 3 | **Logic lives outside components** | Scheduling maths, date maths, IDs = pure functions in `src/lib/`. Components render; they don't think. |
| 4 | **Storage is a separate layer** | Components never touch IndexedDB. They go through `src/data/`, so swapping storage later touches one folder. |
| 5 | **Derived, not stored** | "What's due today" is *computed* from topic data — never saved. Storing derived data is the #1 source of bugs. |
| 6 | **Start flat** | No `features/`, no `entities/`, no `core/`. Extract into folders only when a folder gets crowded. Premature structure is a tax. |
| 7 | **PWA by hand** | `manifest.webmanifest` and `sw.js` written manually so you know what the plugin was doing. |
| 8 | **No backend in v1** | Auth, sync and APIs hide fundamentals behind network failure modes. Local-first, then decide. |

---

## Stack

- **Vite** — dev server + bundler. No Next.js: server rendering is a layer you don't need yet.
- **React 19 + TypeScript** — career-relevant; types force you to state data shapes out loud.
- **Plain CSS** — one stylesheet per component, CSS custom properties for the design tokens.
- **IndexedDB** — browser-native persistence. Not `localStorage` (strings only, sync, tiny).
- **Service Worker + Web Manifest** — the two halves of a PWA.

---

## Folder structure

```
fcf_opencode/
├── ROADMAP.md              ← this file
├── PROGRESS.md             ← your rebuild log (you write it)
├── package.json            Phase 1
├── vite.config.ts          Phase 1
├── tsconfig.json           Phase 1
├── index.html              Phase 1  ← the real entry point
├── public/
│   ├── icons/              Phase 10  ← PWA icons
│   ├── manifest.webmanifest Phase 10
│   ├── sw.js               Phase 10  ← must live at root scope
│   └── offline.html        Phase 10
└── src/
    ├── main.tsx            Phase 1   ← mounts React into the DOM
    ├── App.tsx             Phase 1   ← root component
    ├── types.ts            Phase 3   ← the data model, written first
    ├── styles/
    │   ├── tokens.css      Phase 2   ← design tokens (custom properties)
    │   └── base.css        Phase 2   ← reset + element defaults
    ├── components/         Phase 2   ← reusable, dumb, presentational
    │   ├── Button.{tsx,css}
    │   └── Shell.{tsx,css} ← header/nav/main layout
    ├── pages/              Phase 5–8 ← one folder per route
    │   ├── Today/          list of what's due
    │   ├── AddTopic/       the form
    │   └── TopicDetail/    history + edit + delete
    ├── lib/                Phase 3   ← pure functions, zero React
    │   ├── dates.ts
    │   ├── scheduling.ts   ← the spaced-repetition algorithm
    │   ├── ids.ts
    │   └── router.ts       ← hand-written, Phase 7
    ├── state/              Phase 4
    │   └── store.ts        ← reducer + context = one source of truth
    └── data/               Phase 9
        ├── db.ts           ← IndexedDB open + migrations
        └── repo.ts         ← the only code allowed to touch storage
```

---

## Phases

### Phase 0 — Mental model *(no code)*
How a page actually loads: URL → DNS → request → HTML → CSSOM + DOM → JS → paint.
What Vite does and does not do. What "static site" means.
**Done when:** you can explain why `index.html` is the entry and `main.tsx` is not.

### Phase 1 — Skeleton: the 5 files that make it run
`package.json` · `vite.config.ts` · `tsconfig.json` · `index.html` · `src/main.tsx` · `src/App.tsx`
**Teaches:** dependencies vs devDependencies, scripts, what a bundler resolves, what TS actually
compiles, `createRoot`, why the browser only understands HTML/CSS/JS and never `.tsx`.
**Done when:** `npm run dev` renders a page and you can say what happened at each step.

### Phase 2 — CSS foundations
`styles/tokens.css` → `styles/base.css` → `components/Button` → `components/Shell`
**Teaches:** cascade, specificity, the box model, `*` reset vs modern reset, custom properties,
when flex beats grid, mobile-first media queries, why component CSS is scoped by class naming.
**Done when:** a responsive app shell (header / nav / main) with no framework CSS.

### Phase 3 — The data model and pure logic
`types.ts` → `lib/dates.ts` → `lib/scheduling.ts` → `lib/ids.ts`
**Teaches:** modelling before building, interfaces vs unions, the local-vs-UTC date trap,
pure functions, why testable logic can't live inside a component.
**Done when:** you can compute `dueAt` for a topic and defend every line.

### Phase 4 — State
`state/store.ts`
**Teaches:** single source of truth, reducer pattern, immutability, why mutating state silently
fails to re-render, context vs prop drilling.
**Done when:** you can add a topic in the console and see the UI update.

### Phase 5 — First real page: Today
`pages/Today/TodayPage.tsx` + `.css` → `components/TopicCard`
**Teaches:** list rendering, `key` and why index keys break, event handling, controlled updates,
empty / loading / error states (all three, every list).
**Done when:** due topics render and can be marked reviewed.

### Phase 6 — Forms
`pages/AddTopic/AddTopicPage.tsx` + `.css`
**Teaches:** controlled vs uncontrolled inputs, validation *and* error display, `required` vs real
validation, why forms are the hardest part of the web, keyboard + a11y basics.
**Done when:** invalid input can't be submitted, and errors are announced to screen readers.

### Phase 7 — Routing
`lib/router.ts`
**Teaches:** the URL *is* state; History API (`pushState` / `popstate`); path params; that an SPA
route change never hits the server; why deep links need SPA fallback when deployed.
**Done when:** `/`, `/add`, `/topic/:id` navigate with working back/forward buttons.

### Phase 8 — Detail, edit, delete
`pages/TopicDetail/TopicDetailPage.tsx` + `.css`
**Teaches:** reading params, optimistic updates, confirm-before-destroy, derived history views.
**Done when:** full CRUD works with no persistence (memory only).

### Phase 9 — Persistence
`data/db.ts` → `data/repo.ts` → hydrate the store
**Teaches:** memory vs storage, IndexedDB's async transaction model, schema versioning and
migrations, repository pattern, hydration races, and why `localStorage` is not enough.
**Done when:** reload the tab and everything survives. Then kill the tab mid-write and survive.

### Phase 10 — PWA
`manifest.webmanifest` → icons → `sw.js` → registration → offline UX
**Teaches:** what actually makes an app installable; display modes; **SW scope rules**
(why `sw.js` sits in `public/`, not `src/`); the three caching strategies — cache-first for
immutable assets, network-first for navigation, stale-while-revalidate for data; SW lifecycle and
the update trap; HTTP cache ≠ SW cache.
**Done when:** install it to a home screen, go into airplane mode, and it still works.

### Phase 11 — Ship
Accessibility pass · empty/loading/error audit · responsive audit · `npm run build` · deploy ·
Lighthouse.
**Teaches:** what a production build actually emits, why dev ≠ prod, static hosting, and how to
read an audit instead of chasing a number.
**Done when:** a URL you can open on your phone, installed, working offline.

---

## Deliberately out of scope for v1

Backend · auth · sync across devices · sharing · charts library · dark mode · i18n ·
tests (added in v1.1 once there's something worth testing) · any component library.

---

## Current status

- [x] Phase 0 — roadmap
- [x] Phase 1 — skeleton
- [ ] Phase 2 — CSS foundations
- [ ] Phase 3 — data model
- [ ] Phase 4 — state
- [ ] Phase 5 — Today page
- [ ] Phase 6 — form
- [ ] Phase 7 — router
- [ ] Phase 8 — detail page
- [ ] Phase 9 — persistence
- [ ] Phase 10 — PWA
- [ ] Phase 11 — ship
