# CoMark: Build Plan

---

## Phase 1 — Project Setup

### 1.1 — Initialize the Next.js App
Run the official Next.js scaffolder in the repo root. Choose the following options when prompted:

```bash
npx create-next-app@latest . --typescript --tailwind --eslint --app --src-dir --import-alias "@/*"
```

| Prompt | Choice |
| :--- | :--- |
| TypeScript | Yes |
| Tailwind CSS | Yes |
| ESLint | Yes |
| App Router | Yes |
| `src/` directory | Yes |
| Import alias | `@/*` |

This produces the base folder structure:
```
comark/
├── src/
│   └── app/
│       ├── layout.tsx
│       ├── page.tsx
│       └── globals.css
├── public/
├── next.config.ts
├── tailwind.config.ts
├── tsconfig.json
└── package.json
```

### 1.2 — Install Core Dependencies
Install all third-party packages needed for the project up front:

```bash
npm install @supabase/supabase-js @supabase/ssr tldraw
npm install -D prettier prettier-plugin-tailwindcss
```

| Package | Purpose |
| :--- | :--- |
| `@supabase/supabase-js` | Supabase client (DB, Auth, Realtime) |
| `@supabase/ssr` | Supabase helpers for Next.js App Router (cookie-based auth) |
| `tldraw` | Canvas engine |
| `prettier` + `prettier-plugin-tailwindcss` | Auto-sorts Tailwind classes on save |

### 1.3 — Configure Prettier
Create `.prettierrc` in the project root:

```json
{
  "semi": false,
  "singleQuote": true,
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

Add a `.prettierignore`:
```
.next
node_modules
```

### 1.4 — Set Up Folder Structure
Manually create the following directories inside `src/` to keep the project organized before writing any code:

```
src/
├── app/                  # Next.js App Router pages and layouts
├── components/           # Shared UI components
│   ├── canvas/           # tldraw wrappers and canvas-specific components
│   └── ui/               # Generic UI (buttons, inputs, etc.)
├── lib/                  # Utility modules
│   ├── supabase/         # Supabase client factory functions
│   └── types/            # Shared TypeScript types and interfaces
└── hooks/                # Custom React hooks
```

### 1.5 — Environment Variables
Create a `.env.local` file in the project root (this is gitignored by default):

```bash
NEXT_PUBLIC_SUPABASE_URL=your-supabase-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-supabase-anon-key
```

These values come from your Supabase project dashboard (Phase 2). The `NEXT_PUBLIC_` prefix exposes them to the browser, which is correct — the anon key is safe to be public (Row Level Security enforces access control).

Also create a `.env.example` (committed to Git) as documentation:
```bash
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
```

Ensure `.env.local` is in `.gitignore` (Next.js adds this by default).

### 1.6 — Push to GitHub
If the repo isn't already linked to GitHub:

```bash
git add .
git commit -m "chore: initial project setup"
git remote add origin https://github.com/dylanebaker/CoMark.git
git push -u origin main
```

### 1.7 — Connect to Vercel
1. Go to [vercel.com](https://vercel.com) and click **Add New Project**
2. Import the `CoMark` GitHub repository
3. Vercel auto-detects Next.js — no build config changes needed
4. Add the two environment variables from `.env.local` in the Vercel project settings under **Environment Variables**
5. Deploy

Every push to `main` will now trigger an automatic production deployment.

## Phase 2 — Supabase Setup
- Create a Supabase project
- Design and apply the database schema (rooms, strokes/elements, users)
- Enable Realtime on relevant tables
- Configure environment variables locally and in Vercel

## Phase 3 — Authentication
- Implement anonymous auth via Supabase Auth (no sign-up required to test)
- Optionally add OAuth (GitHub/Google) for persistent identity
- Assign a display name and cursor color to each session

## Phase 4 — Room System
- Build room creation and joining logic (unique room IDs/slugs)
- Persist room state to Supabase
- Build a landing page with create/join room UI

## Phase 5 — Canvas Integration
- Integrate tldraw as the core canvas engine
- Wire tldraw's store to Supabase Realtime for live sync
- Implement conflict-free state merging for concurrent edits

## Phase 6 — Live Cursor Tracking
- Broadcast cursor position in real-time via Supabase Realtime Presence
- Render remote cursors with display names and colors on the canvas

## Phase 7 — Persistence & State Recovery
- Persist canvas snapshots to Supabase on change (debounced)
- Load existing room state when a user joins a room in progress
- Handle late-joiners getting full current state

## Phase 8 — UI / UX Polish
- Build a minimal toolbar (tools, colors, undo/redo)
- Add room sharing (copy link button)
- Add participant list / presence indicators
- Mobile/touch support

## Phase 9 — Performance & Reliability
- Audit and optimize Realtime message frequency (throttle/debounce)
- Set up Supabase keep-alive ping to prevent free-tier project pausing
- Add error handling for dropped connections and reconnection logic

## Phase 10 — Deployment & Testing
- Final environment variable audit
- Deploy to Vercel production
- End-to-end testing with multiple simultaneous users
- Performance testing (latency, sync accuracy)
