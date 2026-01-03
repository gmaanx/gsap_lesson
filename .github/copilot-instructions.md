## Purpose
Help AI coding agents get productive quickly in this repo (React + Vite + GSAP + Tailwind).

## Big picture
- This is a single-page React app built with Vite ([vite.config.js](vite.config.js)).
- Animations are implemented with GSAP and the React wrapper `@gsap/react`. Plugins used: `ScrollTrigger` and `SplitText` (registered in `src/App.jsx`).
- Styling is Tailwind-based with custom utilities and fonts declared in `src/index.css` (many component styles rely on the utility classes defined there).
- Static assets live in the `public/` folder (images under `public/images`, video `public/videos/input.mp4`).
- App-level data (nav links, lists, mock data) is in `constants/index.js`; components import from there rather than hardcoding content.

## Key files to inspect
- [package.json](package.json) — scripts (`dev`, `build`, `preview`, `lint`) and dependencies. Note `vite` is aliased to `rolldown-vite` via `overrides`.
- [vite.config.js](vite.config.js) — Vite plugins: React + Tailwind plugin.
- [src/App.jsx](src/App.jsx) — `gsap.registerPlugin(...)` and top-level layout (includes `Navbar` and `Hero`).
- [src/components/Hero.jsx](src/components/Hero.jsx) and [src/components/Navbar.jsx](src/components/Navbar.jsx) — examples of `useGSAP` usage and selector-based animations.
- [src/index.css](src/index.css) — Tailwind imports, font faces, and custom utilities like `text-gradient`, `radial-gradient`, and component styles.
- [constants/index.js](constants/index.js) — data used across UI (navLinks, lists, etc.).

## Developer workflows (how to run / build / lint)
- Install & run dev server:

  npm install
  npm run dev

- Build and preview:

  npm run build
  npm run preview

- Lint code:

  npm run lint

Notes: `package.json` uses an override that maps `vite` to `rolldown-vite@7.2.5`. Avoid blind upgrades to `vite` without confirming compatibility.

## Project-specific patterns & conventions
- GSAP patterns:
  - Plugins are registered once in `src/App.jsx` (do not re-register in child components).
  - Components use `useGSAP` from `@gsap/react` and frequently select elements with class selectors (e.g., `.title`, `.subtitle`) rather than React refs. Follow existing selector-based approach when adding animations for consistency.
  - Examples: `Hero.jsx` uses `new SplitText('.title', ...)` and then `gsap.from(heroSplit.chars, ...)`.
- Styling:
  - Tailwind is used together with custom utilities in `src/index.css`. When adding classes, prefer the existing utilities (e.g., `text-gradient`, `radial-gradient`).
  - Global fonts and custom CSS components are defined in `src/index.css` — inspect it before adding new layout styles.
- Data & content:
  - Centralized in `constants/index.js`. To add a site nav item, update `navLinks` there; components map over that array.

## Integration points & external dependencies
- `@gsap/react` integration — look for `useGSAP` usage in `src/components/*`.
- GSAP plugins imported from `gsap/all` — ensure `SplitText` availability; some SplitText builds may be non-free in official GSAP packaging; this project uses the plugin directly via `gsap/all` import.
- Tailwind + `@tailwindcss/vite` plugin — styling changes often require restart of dev server if plugin config changes.

## Common pitfalls and notes for AI edits
- Watch for duplicate imports or accidental re-declarations (e.g., `Hero.jsx` currently imports `useRef` twice). Preserve existing import styles unless refactoring deliberately.
- When modifying animations, prefer updating selector strings and GSAP timelines instead of switching the app to refs unless you update all similar components for consistency.
- Static assets: reference paths using the same `/images/...` and `/videos/...` paths used in the code (served from `public/`).
- Linting: `eslint.config.js` enforces `no-unused-vars` but ignores uppercase-prefixed globals; run `npm run lint` to validate changes.

## If you need to change architecture
- Discuss before: changing how animations are wired (selectors → refs), replacing the GSAP build, or swapping Vite for another dev server. These are cross-cutting and affect many files.

## Quick examples
- Add new nav item: update [constants/index.js](constants/index.js) `navLinks` array — components will automatically pick it up.
- Add a simple fade animation to `.subtitle`: modify `src/components/Hero.jsx` `useGSAP` callback and add a `gsap.from('.subtitle', {opacity:0, y:20, duration:1})` call.

---
If anything above is unclear or you want a different focus (tests, CI, or deeper GSAP/plugin guidance), tell me which area to expand or if you'd like me to merge this into an existing AI instruction file.
