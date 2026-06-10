# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Personal blog/site ("CodingAthlete", deployed at https://codingathlete.com/) built on the **AstroPaper** Astro theme. Static site, TypeScript, TailwindCSS v4, Pagefind for search. No backend — output is a static `./dist/` served via nginx (Docker) or Cloudflare Pages.

## Commands

This project uses **pnpm** (the lockfile is `pnpm-lock.yaml`; `package-lock.json` is gitignored). If pnpm isn't installed, enable it via corepack: `corepack enable && corepack prepare pnpm@latest --activate`.

- `pnpm install` — install deps. `esbuild` and `sharp` need build scripts to run; they're allowlisted in `pnpm-workspace.yaml` (`allowBuilds`). pnpm blocks unlisted build scripts by default, so a new native dependency must be added there.
- `pnpm dev` — dev server at `localhost:4321`
- `pnpm build` — **full production build**: runs `astro check` (type check), `astro build`, then `pagefind --site dist` to generate the search index, then copies it into `public/`. The pagefind step is why search only works against a real build, not `dev`.
- `pnpm preview` — preview the built site
- `pnpm sync` — regenerate Astro-generated types (`astro sync`); run this after changing the content schema if types feel stale
- `pnpm lint` — ESLint
- `pnpm format` / `pnpm format:check` — Prettier write / check

There is no test suite. "Validation" is `astro check` (part of `build`) plus lint/format.

## Content model

Blog posts are markdown files in `src/data/blog/`, loaded as the `blog` content collection (`src/content.config.ts`). Schema highlights (frontmatter):

- Required: `title`, `description`, `pubDatetime` (a date)
- Optional: `author` (defaults to `SITE.author`), `modDatetime`, `featured`, `draft`, `tags` (defaults to `["others"]`), `ogImage`, `canonicalURL`, `hideEditPost`, `timezone`

Conventions enforced by the loader/utils:

- Files matching `**/[^_]*.md` are loaded — **a leading underscore excludes** a file. Directories starting with `_` (e.g. `src/data/blog/_releases`) are excluded from URL path segments by `getPath`.
- Nested directories under `blog/` become URL path segments (each segment is slugified). See `src/utils/getPath.ts` — the post URL is `/posts/<slugified-dir-segments>/<slug>`.
- `draft: true` and posts scheduled in the future are filtered out in production by `src/utils/postFilter.ts` (uses `SITE.scheduledPostMargin`).
- Slugification (`src/utils/slugify.ts`) is hybrid: `slugify` for Latin strings, `lodash.kebabcase` for strings containing non-Latin characters.

## Configuration

`src/config.ts` (`SITE` object) is the central knob for site-wide behavior: URL, author, title, OG image, light/dark toggle, posts-per-page, archives visibility, edit-post links, timezone, language. Most "change the site's X" requests resolve here.

`src/constants.ts` defines `SOCIALS` and `SHARE_LINKS` (icon + URL lists). Icons are SVGs imported from `src/assets/icons/`.

`astro.config.ts` wires up: sitemap, markdown remark plugins (`remark-toc`, `remark-collapse`), Shiki code highlighting with `min-light`/`night-owl` themes and custom transformers (filename + diff/highlight notation), Tailwind via Vite plugin, Google Fonts, and the `PUBLIC_GOOGLE_SITE_VERIFICATION` env field.

## Architecture notes

- **Path alias**: `@/*` → `src/*` (defined in `tsconfig.json`, extends `astro/tsconfigs/strict`).
- **Routing** is file-based in `src/pages/`. Dynamic post routes live in `src/pages/posts/[...slug]/` and `src/pages/posts/[...page].astro`; tag routes in `src/pages/tags/`. Non-HTML endpoints: `rss.xml.ts`, `robots.txt.ts`, `og.png.ts`, and per-post `index.png.ts`.
- **OG image generation**: dynamic OG images are rendered with `satori` + `@resvg/resvg-js` from JSX-like templates in `src/utils/og-templates/` (`post.js`, `site.js`), driven by `src/utils/generateOgImages.ts` and `loadGoogleFont.ts`. `@resvg/resvg-js` is excluded from Vite `optimizeDeps`. Controlled by `SITE.dynamicOgImage`.
- **Post querying utils** in `src/utils/`: `getSortedPosts`, `getPostsByTag`, `getPostsByGroupCondition`, `getUniqueTags`, `postFilter`, `getPath`. Reuse these rather than re-querying the collection.
- **Theme toggle** (light/dark) is handled by `src/scripts/theme.ts`; styles in `src/styles/global.css` and `typography.css`.
- **Layouts** (`src/layouts/`): `Layout.astro` (base `<head>`/shell), `Main.astro`, `PostDetails.astro`, `AboutLayout.astro`.

## Deployment

`Dockerfile` is multi-stage: enables pnpm via corepack, builds with `pnpm install --frozen-lockfile` + `pnpm run build`, then serves `dist/` via nginx. `docker-compose.yml` is for local dev (mounts the repo, runs the dev server with `--host`).
