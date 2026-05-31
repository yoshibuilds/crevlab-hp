# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

**clarix-hp** is the static homepage and product portfolio for **Crevlab** (クレブラブ), a personal development studio. The site is hosted on GitHub Pages and serves as both a landing page and a hub linking to 10 standalone web tools—all in Japanese.

## Architecture

The repo has **no build toolchain**. Every file is deployed as-is from the `main` branch root via GitHub Pages.

There are two distinct implementation patterns:

### 1. Pre-built React apps (Vite output, source not in this repo)
These directories contain only the compiled output—an `index.html` shell and a bundled `assets/index-*.js`:
- `stream-picks/`, `travel-picks/`, `gift-finder/`, `kiru/`, `manga-picks/`, `fresh-box/`

Each uses **Tailwind CSS via CDN** (`cdn.tailwindcss.com`) with inline `tailwind.config` for custom tokens, and a React root at `<div id="root">`. **Do not modify the `assets/*.js` bundles directly**—they are minified build outputs.

### 2. Self-contained single-file static tools (HTML + CSS + JS)
All logic lives inside one `index.html` with inline `<style>` and `<script>`:
- `life-budget/` — budget simulator using Chart.js from CDN
- `place-log/` — outing/place diary with `localStorage`
- `room-planner/` — furniture layout tool with `localStorage`
- `group-eats/` — group restaurant finder

These use CSS custom properties (`--primary`, `--bg`, `--card`, etc.) for theming and Japanese system fonts (`Hiragino Kaku Gothic ProN`, `Hiragino Sans`, `Meiryo`).

### Homepage (`index.html`)
The root `index.html` is the Crevlab landing page—a single self-contained file with embedded CSS and vanilla JS. It lists all products as cards linking to their respective subdirectories (`./stream-picks/index.html`, etc.).

## Development Workflow

Since there is no build step, preview any page by opening it directly in a browser or using a local server:

```bash
# Serve the whole site locally
npx serve .
# or
python3 -m http.server 8080
```

Update publishing to GitHub Pages:

```bash
git add <changed-files>
git commit -m "Update: <description>"
git push -u origin main
```

GitHub Pages deploys automatically from `main` branch root within a few minutes.

## Key Conventions

- **Language**: All UI text is in Japanese. Keep new content in Japanese.
- **Single-file tools**: When editing `life-budget`, `place-log`, `room-planner`, or `group-eats`, all HTML/CSS/JS stays in that tool's single `index.html`. Do not split into separate files.
- **Back navigation**: Every sub-tool has a back link pointing to `../` (the homepage). Preserve this pattern when adding new tools.
- **CSS variables**: Each tool defines its own color palette via `:root` CSS custom properties. The homepage uses a monochrome `--black`/`--white`/`--grey` scale; sub-tools each have their own accent color system.
- **No `localStorage` key collisions**: Each tool that uses `localStorage` should use a unique key prefix (e.g., `placelog_`, `roomplan_`).
- **External dependencies via CDN only**: Chart.js, Tailwind CSS, and any other libraries are loaded from CDN. There is no local `node_modules` or lockfile.

## Adding a New Product

1. Create a new subdirectory (e.g., `my-tool/`).
2. Implement as a single `index.html` (preferred) or as a pre-built React app output.
3. Include a back link to `../` in the tool's header.
4. Add a product card to the `#products` grid in the root `index.html`, following the existing `.product-card` markup pattern (icon, badges, name, description, tags, link).
