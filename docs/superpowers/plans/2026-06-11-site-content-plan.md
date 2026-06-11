# Site Content Plan Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Restructure codingathlete.com so the home page tells the whole story (bio → skills → experience → athlete → contact) in a minimalist tone, with a deeper About/CV page, and blog/projects/search removed from navigation.

**Architecture:** Astro static site. Three edited files: `Header.astro` (trim nav), `index.astro` (rewrite home page sections), `about.md` (rewrite as About/CV), plus a one-line `config.ts` meta description update. No new components — reuse existing `Layout`, `Header`, `Footer`, `Socials`, `LinkButton`. No blog/projects code is deleted; those routes simply lose their nav links.

**Tech Stack:** Astro, TailwindCSS v4, TypeScript. No test suite — validation is `pnpm build` (runs `astro check`) plus a visual check on `pnpm dev`.

---

## Content reference (source of truth for copy)

Tone rules for ALL copy: minimalist, plain, factual, short sentences. No buzzwords. **No sport→work narrative** (no "discipline carries over" etc.).

Experience timeline (most recent first):

| Company | Title | Dates | Location | One-liner | Stack |
|---|---|---|---|---|---|
| DOOK | Senior Software Engineer | Aug 2025 – Present | Cracow / Remote | Building newcode.ai, a generative-AI platform for legal teams (document analysis, drafting, agentic workflows). | Python, FastAPI, Docker, Azure, Langflow, Langfuse |
| Payability | Senior Software Engineer | Jul 2024 – Jun 2025 | Remote, Cracow | Led a daily ingestion process loading data from external APIs into AWS-hosted microservices. | Python, FastAPI, Docker, AWS, PostgreSQL |
| Bitpanda | Software Engineer, Blockchain | Apr 2021 – Jun 2024 | Remote, Cracow | Built and maintained blockchain services across the Bitpanda ecosystem. | Blockchain, Python, FastAPI, Docker, PostgreSQL, Kafka, AWS, Datadog, ArgoCD, Terraform, Kubernetes |
| Credit Suisse | Data Engineer | Jan 2020 – Mar 2021 | Lausanne, Switzerland | Built and maintained ETL data pipelines and custom data-processing software. | Python, Pandas, PySpark, Git, AWS |
| Groupon | Software Development Engineer 1 | Dec 2017 – Dec 2019 | Katowice, Poland | Started in software development — best practices and contributions across teams and domains. | Python, Flask, Django, Docker, Jenkins, Splunk |

> NOTE for reviewer: the DOOK title "Senior Software Engineer" is inferred from the seniority trajectory — confirm/correct during review.

Home page shows the top 3 (DOOK, Payability, Bitpanda); the full five live on About.

---

## Task 1: Trim navigation to Home + About

**Files:**
- Modify: `src/components/Header.astro` (the `<ul id="menu-items">` block)

- [ ] **Step 1: Remove the Posts, Projects, and Search nav items**

In `src/components/Header.astro`, delete the `Posts` list item:

```astro
        <li class="col-span-2">
          <a href="/posts" class:list={{ "active-nav": isActive("/posts") }}>
            Posts
          </a>
        </li>
```

Delete the `Projects` list item:

```astro
        <li class="col-span-2">
          <a
            href="/projects"
            class:list={{ "active-nav": isActive("/projects") }}
          >
            Projects
          </a>
        </li>
```

Delete the `Search` list item:

```astro
        <li class="col-span-1 flex items-center justify-center">
          <LinkButton
            href="/search"
            class:list={[
              "focus-outline flex p-3 sm:p-1",
              { "[&>svg]:stroke-accent": isActive("/search") },
            ]}
            title="Search"
            aria-label="search"
          >
            <IconSearch />
            <span class="sr-only">Search</span>
          </LinkButton>
        </li>
```

Leave the `About` item and the theme-toggle `<li>` (gated by `SITE.lightAndDarkMode`) in place. The Archives item stays as-is — it is already gated by `SITE.showArchives`, which is `false`.

- [ ] **Step 2: Remove the now-unused IconSearch import**

At the top of `src/components/Header.astro`, delete this line:

```astro
import IconSearch from "@/assets/icons/IconSearch.svg";
```

Leave `IconArchive` (still used by the Archives item) and all other imports.

- [ ] **Step 3: Build to verify no type/lint errors**

Run: `pnpm build`
Expected: completes successfully through `astro check` and `astro build` with no errors. (The pagefind step still runs; that is fine.)

- [ ] **Step 4: Commit**

```bash
git add src/components/Header.astro
git commit -m "Trim site nav to Home + About"
```

---

## Task 2: Rewrite the home page

**Files:**
- Modify (full replace): `src/pages/index.astro`

- [ ] **Step 1: Replace the entire contents of `src/pages/index.astro`**

```astro
---
import Layout from "@/layouts/Layout.astro";
import Header from "@/components/Header.astro";
import Footer from "@/components/Footer.astro";
import Socials from "@/components/Socials.astro";
import LinkButton from "@/components/LinkButton.astro";
import { SITE } from "@/config";
import { SOCIALS } from "@/constants";
---

<Layout>
  <Header />
  <main id="main-content" data-layout="index" class="app-layout">
    <!-- Hero -->
    <section id="hero" class="border-y-2 border-border py-8 sm:py-10">
      <div class="flex flex-col gap-6 sm:flex-row sm:items-center sm:gap-8">
        <div class="flex-shrink-0">
          <img
            src="/seb-avatar.jpeg"
            alt={SITE.author}
            width="192"
            height="192"
            class="mx-auto h-40 w-40 rounded-full object-cover sm:mx-0 sm:h-48 sm:w-48"
          />
        </div>
        <div class="min-w-0 flex-1">
          <h1 class="my-2 text-2xl font-bold sm:my-0 sm:text-3xl">
            Hi, I'm Sebastian
          </h1>
          <p class="mt-2 text-foreground/90">
            Software engineer and athlete. I build backends in Python. I race
            HYROX Pro and run.
          </p>
          {
            SOCIALS.length > 0 && (
              <div class="mt-4 flex flex-wrap items-center gap-1">
                <Socials />
              </div>
            )
          }
          <div class="mt-5 flex flex-wrap gap-3">
            <LinkButton
              href="/about"
              class="rounded-md border border-border px-4 py-2 text-sm font-medium"
            >
              About
            </LinkButton>
            <LinkButton
              href="mailto:wdowiarzsebastian@gmail.com"
              class="rounded-md border border-border px-4 py-2 text-sm font-medium"
            >
              Get in touch
            </LinkButton>
          </div>
        </div>
      </div>
    </section>

    <!-- What I do -->
    <section id="what-i-do" class="py-10">
      <h2 class="text-2xl font-semibold tracking-wide">What I do</h2>
      <div class="mt-6 grid gap-6 sm:grid-cols-3">
        <div class="rounded-lg border border-border p-5">
          <h3 class="text-lg font-semibold">Backend &amp; APIs</h3>
          <p class="mt-2 text-sm text-foreground/70">
            Python and FastAPI. Designing and building services and APIs.
          </p>
        </div>
        <div class="rounded-lg border border-border p-5">
          <h3 class="text-lg font-semibold">Infra &amp; DevOps</h3>
          <p class="mt-2 text-sm text-foreground/70">
            Docker, AWS and Azure, Kubernetes, CI/CD.
          </p>
        </div>
        <div class="rounded-lg border border-border p-5">
          <h3 class="text-lg font-semibold">AI &amp; data</h3>
          <p class="mt-2 text-sm text-foreground/70">
            LLM apps with Langflow and Langfuse. Data pipelines.
          </p>
        </div>
      </div>
    </section>

    <!-- Experience -->
    <section id="experience" class="border-t border-border py-10">
      <h2 class="text-2xl font-semibold tracking-wide">Experience</h2>
      <ul class="mt-6 grid gap-6">
        <li class="rounded-lg border border-border p-5">
          <div class="flex flex-wrap items-baseline justify-between gap-2">
            <h3 class="text-lg font-semibold">
              Senior Software Engineer · DOOK
            </h3>
            <span class="text-sm text-foreground/60">Aug 2025 – Present</span>
          </div>
          <p class="mt-2 text-sm text-foreground/70">
            Building newcode.ai, a generative-AI platform for legal teams —
            document analysis, drafting and agentic workflows. Python, FastAPI,
            Docker, Azure, Langflow, Langfuse.
          </p>
        </li>
        <li class="rounded-lg border border-border p-5">
          <div class="flex flex-wrap items-baseline justify-between gap-2">
            <h3 class="text-lg font-semibold">
              Senior Software Engineer · Payability
            </h3>
            <span class="text-sm text-foreground/60">Jul 2024 – Jun 2025</span>
          </div>
          <p class="mt-2 text-sm text-foreground/70">
            Led a daily ingestion process loading data from external APIs into
            AWS-hosted microservices. Python, FastAPI, AWS, PostgreSQL.
          </p>
        </li>
        <li class="rounded-lg border border-border p-5">
          <div class="flex flex-wrap items-baseline justify-between gap-2">
            <h3 class="text-lg font-semibold">
              Software Engineer, Blockchain · Bitpanda
            </h3>
            <span class="text-sm text-foreground/60">Apr 2021 – Jun 2024</span>
          </div>
          <p class="mt-2 text-sm text-foreground/70">
            Built and maintained blockchain services across the Bitpanda
            ecosystem. Python, Kafka, Kubernetes, AWS, Terraform.
          </p>
        </li>
      </ul>
      <div class="mt-6">
        <LinkButton href="/about" class="text-sm font-medium text-accent">
          Full experience →
        </LinkButton>
      </div>
    </section>

    <!-- The athlete -->
    <section id="athlete" class="border-t border-border py-10">
      <h2 class="text-2xl font-semibold tracking-wide">The athlete</h2>
      <p class="mt-4 max-w-2xl text-foreground/80">
        I train CrossFit and race HYROX Pro regularly. I ran a marathon in
        3:27:50, and I cycle.
      </p>
    </section>

    <!-- Contact -->
    <section id="contact" class="border-t border-border py-10">
      <h2 class="text-2xl font-semibold tracking-wide">Contact</h2>
      <p class="mt-4 text-foreground/80">
        Open to interesting work. Reach me on
        <a
          href="https://www.linkedin.com/in/sebastian-wdowiarz-b5743483/"
          target="_blank"
          rel="noopener noreferrer"
          class="text-accent">LinkedIn</a
        >
        or by
        <a href="mailto:wdowiarzsebastian@gmail.com" class="text-accent"
          >email</a
        >.
      </p>
    </section>
  </main>
  <Footer />
</Layout>
```

- [ ] **Step 2: Build to verify no errors**

Run: `pnpm build`
Expected: completes successfully. `astro check` reports no unused-import or type errors (note this file no longer imports `getSortedPosts`, `Card`, `IconRss`, or `IconArrowRight`, and no longer calls `getCollection`).

- [ ] **Step 3: Visual check**

Run: `pnpm dev`, open `http://localhost:4321/`.
Expected: hero with avatar + new one-liner + socials + two buttons; "What I do" three tiles; "Experience" three cards with a "Full experience →" link; "The athlete" line; "Contact" line. No "Featured"/"Recent posts" sections anywhere.

- [ ] **Step 4: Commit**

```bash
git add src/pages/index.astro
git commit -m "Rewrite home page: skills, experience, athlete, contact"
```

---

## Task 3: Rewrite the About / CV page

**Files:**
- Modify (full replace): `src/pages/about.md`

- [ ] **Step 1: Replace the entire contents of `src/pages/about.md`**

```markdown
---
layout: ../layouts/AboutLayout.astro
title: "About"
---

I'm Sebastian Wdowiarz, a software engineer based in Cracow, Poland, with 8+ years building backend systems. I work in Python — APIs, services, data pipelines, and lately LLM applications. Outside work I train CrossFit and race HYROX Pro regularly. I ran a marathon in 3:27:50, and I cycle.

## Experience

### Senior Software Engineer · DOOK
*Aug 2025 – Present · Cracow / Remote*

Building newcode.ai, a generative-AI platform for legal teams — document analysis, drafting, and agentic workflows. Python, FastAPI, Docker, Azure, Langflow, Langfuse.

### Senior Software Engineer · Payability
*Jul 2024 – Jun 2025 · Remote, Cracow*

Led a daily ingestion process that transformed and loaded data from external APIs into AWS-hosted microservices orchestrating data from multiple sources. Python, FastAPI, Docker, AWS, PostgreSQL.

### Software Engineer, Blockchain · Bitpanda
*Apr 2021 – Jun 2024 · Remote, Cracow*

Built and maintained blockchain technology and applications across the Bitpanda ecosystem. Python, FastAPI, Docker, PostgreSQL, Kafka, AWS, Datadog, ArgoCD, Terraform, Kubernetes.

### Data Engineer · Credit Suisse
*Jan 2020 – Mar 2021 · Lausanne, Switzerland*

Built and maintained ETL data pipelines and custom software for data processing. Python, Pandas, PySpark, Git, AWS.

### Software Development Engineer 1 · Groupon
*Dec 2017 – Dec 2019 · Katowice, Poland*

Started in software development — best practices and contributions across teams and domains. Python, Flask, Django, Docker, Jenkins, Splunk.

## Skills

- **Languages:** Python, Rust
- **Backend:** FastAPI, Flask, Django, PostgreSQL, Redis, Kafka
- **Infra & DevOps:** Docker, Kubernetes, AWS, Azure, Terraform, ArgoCD, CI/CD
- **AI & data:** Langflow, Langfuse, Pandas, PySpark

## Contact

Find me on [GitHub](https://github.com/sebastianw93), [X](https://x.com/coding_athlete), [LinkedIn](https://www.linkedin.com/in/sebastian-wdowiarz-b5743483/), or [email](mailto:wdowiarzsebastian@gmail.com).
```

- [ ] **Step 2: Build to verify no errors**

Run: `pnpm build`
Expected: completes successfully.

- [ ] **Step 3: Visual check**

Run: `pnpm dev`, open `http://localhost:4321/about`.
Expected: intro paragraph, full five-entry Experience list, grouped Skills, Contact links.

- [ ] **Step 4: Commit**

```bash
git add src/pages/about.md
git commit -m "Rewrite About as minimalist About/CV with full experience"
```

---

## Task 4: Update site meta description

**Files:**
- Modify: `src/config.ts:5` (the `desc` field)

- [ ] **Step 1: Replace the `desc` value**

In `src/config.ts`, change:

```ts
  desc: "Python software engineer and athlete building scalable backend systems. CrossFit, Hyrox, running, cycling.",
```

to:

```ts
  desc: "Software engineer and athlete. Python backends, APIs and data. CrossFit, HYROX, running, cycling.",
```

- [ ] **Step 2: Build to verify no errors**

Run: `pnpm build`
Expected: completes successfully.

- [ ] **Step 3: Commit**

```bash
git add src/config.ts
git commit -m "Update site meta description to match new tone"
```

---

## Self-review notes

- **Spec coverage:** Nav trim (Task 1) covers "remove blog/projects/search/tags from nav" — Tags lives under `/posts` tag routes and had no standalone nav item, so no separate removal is needed. Home blueprint (Task 2) covers Hero/What I do/Experience/Athlete/Contact and removes featured/recent posts. About/CV (Task 3) covers intro + full experience + grouped skills + contact, no PDF link. Meta description (Task 4) extends the tone change to SEO copy.
- **Out of scope (per spec):** no blog posts written; projects page left in code but unlinked; no design overhaul.
- **Tone check:** all copy is plain and factual; the athlete section states facts only with no sport→work narrative.
