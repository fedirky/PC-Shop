# PC Shop

A lightweight front‑end for a PC components/shop demo built with **Vue 3**, **Vite**, and **TypeScript**.

> This README is based on the repository layout you provided (archive: `pc shop.rar`). If something looks off, let me know and I’ll adjust.

---

## Overview

PC Shop is a single‑page application (SPA) scaffolded with Vite and Vue 3 (TypeScript). It provides the groundwork for building an online storefront UI—product listings, detail pages, and cart/checkout flows—while keeping the stack minimal and fast for local development.

---

## Tech Stack

- **Framework:** Vue 3
- **Build Tool:** Vite
- **Language:** TypeScript
- **Styling:** CSS (`src/style.css`)

---

## Project Structure

```
pc shop/
├─ .git/                   # Git repository metadata
├─ .gitignore
├─ README.md               # (Root readme in the repo)
└─ front-end/
   ├─ .gitignore
   ├─ .vscode/             # Editor settings (optional)
   ├─ README.md            # Front-end specific readme (template/notes)
   ├─ index.html           # HTML entry
   ├─ node_modules/        # Dependencies (can be regenerated)
   ├─ package.json         # Project metadata & npm scripts
   ├─ public/              # Static assets served as-is
   ├─ src/
   │  ├─ App.vue           # Root Vue component
   │  ├─ assets/           # Project assets (images, icons, etc.)
   │  ├─ main.ts           # App bootstrap & mount
   │  └─ style.css         # Global styles
   └─ tsconfig.json        # TypeScript configuration
```

> Note: `node_modules/` is included in the archive; you may safely delete it and reinstall dependencies locally.

---

## Getting Started

### Prerequisites
- **Node.js** 18+ (LTS recommended)
- **npm** (bundled with Node) or **pnpm/yarn** if you prefer

### Installation

```bash
cd "pc shop/front-end"
npm install
```

### Development Server

```bash
npm run dev
```

This starts Vite’s dev server. Open the printed local URL (typically `http://localhost:5173/`).

### Production Build

```bash
npm run build
```

Build artifacts will be emitted to `dist/`.

### Preview Production Build

```bash
npm run preview
```

Serves the production build locally to verify the output.

> If the exact script names differ in `package.json`, use `npm run` to list available scripts.

---

## Configuration Notes

- **Entry files:** `index.html` (HTML), `src/main.ts` (bootstrap), `src/App.vue` (root component).
- **Global styles:** `src/style.css`.
- **Static assets:** Place files in `public/` to be served as-is, or import from `src/assets/` for bundling.
- **TypeScript:** Controlled via `tsconfig.json`.
