# Site Content Plan — codingathlete.com

Date: 2026-06-11

## Goal

Restructure the site so it works on two levels at once:

- A recruiter who lands on the page can see who Sebastian is and what he has
  done within ~30 seconds.
- A casual visitor stays curious — the athlete identity (`@coding_athlete`)
  is a deliberate hook.

Sebastian is passively open to offers (not actively job-hunting), so the site
should make a strong impression when someone finds it, without reading as an
aggressive "hire me" page.

## Constraints & decisions

- **Structure:** Approach C — a rich home page that tells the whole story,
  plus one deeper page (About/CV). No multi-page sprawl.
- **Tone (important):** Minimalist, plain, factual. Short sentences. No
  buzzwords ("scalable life-changing systems"), no marketing voice. Reference
  example: *"Software engineer and athlete. I build software, race HYROX Pro,
  and run."*
- **No sport→work narrative:** Do NOT link sport to engineering ("the
  discipline carries over", "consistency in training = consistency in code").
  State sport as plain fact only.
- **Blog:** Minimal/none. Removed from navigation and home page. Code/routing
  stays in the repo (not linked) so it can return later.
- **Projects:** Removed for now — current projects are not worth showcasing.
  Projects page removed from navigation; code kept. Will be re-added when there
  are stronger projects.
- **No PDF CV** link.
- **Search (Pagefind):** Removed from navigation (nothing to search without
  blog/projects).
- **Tags:** Removed from navigation (only relevant to blog).
- **Final navigation:** Home, About.

## Home page — section blueprint

Scroll order:

1. **Hero** (refine existing)
   - Avatar + "Hi, I'm Sebastian".
   - One plain positioning line in the minimalist tone, e.g.
     *"Software engineer and athlete. I build backends in Python. I race
     HYROX Pro and run."*
   - Social links (GitHub, LinkedIn, X, email).
   - CTAs: "About" / "Get in touch". (No "View Projects", no "Download CV".)

2. **What I do** (new)
   - Three compact tiles/columns, factual:
     - Backend & APIs — Python, FastAPI.
     - Infra & DevOps — Docker, cloud, CI/CD.
     - Exploring — Rust, blockchain.
   - Purpose: recruiter sees stack and specialization at a glance.

3. **Experience** (new — primary recruiter proof)
   - Concise timeline: DO OK (current) → Groupon (SDE) → earlier if worth it.
   - Each entry: role, years, 1–2 plain sentences (concrete impact/numbers
     where available).
   - Link "Full experience →" to About/CV page.

4. **The Athlete** (new — the differentiator, prominently placed)
   - Plain statement of what he does: CrossFit, HYROX, races HYROX Pro, runs,
     cycles. Optional concrete results/distances. Optional 1–2 photos.
   - No motivational framing, no sport→work connection.

5. **Contact / CTA** (new — closing)
   - "Open to interesting work." + email / LinkedIn.

Removed from home: Featured posts, Recent posts, Featured projects.

## About / CV page (rework existing about.md)

Rewrite existing copy in the minimalist tone and add CV detail:

- **Intro** — short, factual (who/where/what).
- **Experience** — full timeline (DO OK, Groupon, earlier): role, years,
  1–2 sentences each.
- **Skills** — grouped tech stack (Languages / Backend / Infra / Exploring).
- **Contact** — links (GitHub, X, LinkedIn, email).

No PDF CV link.

## Pages removed from navigation (code retained, unlinked)

- Blog / posts
- Projects
- Search
- Tags

## Out of scope (for now)

- Writing blog posts.
- Adding/curating a projects showcase (revisit when stronger projects exist).
- Visual/design overhaul beyond what the new sections require.
