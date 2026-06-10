# CodingAthlete

Personal site and blog of **Sebastian Wdowiarz** — a Python software engineer and athlete writing about backend engineering, sport, and the overlap between the two.

🌐 Live at [codingathlete.com](https://codingathlete.com/)

## Tech Stack

- **Framework** — [Astro](https://astro.build/)
- **Language** — [TypeScript](https://www.typescriptlang.org/)
- **Styling** — [TailwindCSS](https://tailwindcss.com/)
- **Search** — [Pagefind](https://pagefind.app/)
- **Package manager** — [pnpm](https://pnpm.io/)
- **Deployment** — [Cloudflare Pages](https://pages.cloudflare.com/)

## Getting Started

This project uses **pnpm**. If you don't have it, enable it via corepack:

```bash
corepack enable && corepack prepare pnpm@latest --activate
```

Then:

```bash
pnpm install   # install dependencies
pnpm dev       # start the dev server at http://localhost:4321
```

## Commands

| Command             | Action                                                          |
| :------------------ | :-------------------------------------------------------------- |
| `pnpm install`      | Install dependencies                                            |
| `pnpm dev`          | Start the local dev server at `localhost:4321`                  |
| `pnpm build`        | Type-check, build to `./dist/`, and generate the Pagefind index |
| `pnpm preview`      | Preview the production build locally                            |
| `pnpm sync`         | Regenerate Astro types (run after changing the content schema)  |
| `pnpm lint`         | Lint with ESLint                                                |
| `pnpm format`       | Format with Prettier                                            |
| `pnpm format:check` | Check formatting with Prettier                                  |

## Writing Posts

Blog posts are markdown files in `src/data/blog/`. Each needs frontmatter with at least `title`, `description`, and `pubDatetime`. Nested directories become URL path segments; a leading underscore (`_`) on a file or directory excludes it from the build. Set `draft: true` to keep a post out of production.

## Configuration

Site-wide settings (title, author, URL, posts-per-page, theme, etc.) live in `src/config.ts`. Social and share links live in `src/constants.ts`.

> See [`CLAUDE.md`](./CLAUDE.md) for a fuller architecture overview.

## Docker

```bash
docker build -t codingathlete .   # build the static site and serve it via nginx
docker run -p 4321:80 codingathlete
```

## Credits

Built on the [AstroPaper](https://github.com/satnaing/astro-paper) theme by [Sat Naing](https://satnaing.dev). Licensed under the MIT License.
