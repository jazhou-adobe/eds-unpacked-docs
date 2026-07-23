# Website Development Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the EDS blocks needed for the page types defined in
`docs/superpowers/specs/2026-07-23-ux-site-structure-design.md` — a
myth-busting callout, a Section hub content list, and a homepage
"recently added" list — in the `jazhou-adobe/eds-unpacked` code repo.

**Architecture:** EDS blocks are plain JS/CSS pairs under `blocks/<name>/`,
decorated automatically by `scripts/scripts.js`. The Section hub and
homepage lists both fetch `/query-index.json` (Adobe's auto-generated
content index) and filter/sort it client-side — no server-side code, no
build step. This depends on page metadata (`section`, `subtopic`,
`format`) being present as a Metadata block on every published document,
per `docs/superpowers/specs/2026-07-23-content-workflow-design.md`.

**Tech Stack:** Vanilla JS/CSS (EDS block convention), no framework, no
bundler — matches the existing `jazhou-adobe/eds-unpacked` repo (stock
`aem-boilerplate` v1.3.0, blocks: `header`, `footer`, `cards`, `columns`,
`hero`, `fragment`, `widget`).

## Global Constraints

- Repo: `jazhou-adobe/eds-unpacked` (verified 2026-07-23: stock
  `aem-boilerplate`, `npm install` and `npm run lint` both pass clean,
  `aem up` serves locally at `http://localhost:3000`).
- New blocks only — do not modify `header`, `footer`, `cards`, `columns`,
  `hero`, `fragment`, or `widget`; they're unrelated to this work.
- Sections are exactly `architecture`, `migration`, `development` (UX
  spec); a hub page's section is derived from its URL path segment
  (`/architecture` → `architecture`), not hardcoded per block instance.
- No search/filter UI (UX spec, v1 decision) — lists are plain,
  unfiltered, reverse-chronological or as-indexed.
- Reuse existing CSS custom properties from `styles/styles.css`
  (`--link-color`, `--light-color`, `--dark-color`, `--text-color`,
  `--body-font-size-s`) rather than introducing new color/font values.
- **Not covered by this plan** (flagged, needs the user's own login):
  configuring the query-index itself via `https://tools.aem.live/tools/index-admin/`,
  authoring the `/nav` document, composing the home page and section hub
  pages in da.live, and creating the About page. These are da.live content
  actions, not code — see Task 4.

---

### Task 1: Build the `myth-callout` block

**Files:**
- Create: `blocks/myth-callout/myth-callout.js`
- Create: `blocks/myth-callout/myth-callout.css`

**Interfaces:**
- Consumes: standard EDS block decoration contract — `decorate(block)`
  receives the block's DOM element, called by `scripts/scripts.js`'s
  existing `decorateBlocks`/`loadBlock` pipeline (no changes needed
  there).
- Produces: a `.myth-callout` element other blocks/pages don't depend on
  (self-contained, no shared state).

- [ ] **Step 1: Write the block JS**

  ```javascript
  // blocks/myth-callout/myth-callout.js
  export default function decorate(block) {
    block.classList.add('myth-callout');
    const label = document.createElement('p');
    label.className = 'myth-callout-label';
    label.textContent = 'Myth';
    block.prepend(label);
  }
  ```

- [ ] **Step 2: Write the block CSS**

  ```css
  /* blocks/myth-callout/myth-callout.css */
  .myth-callout {
    background-color: var(--light-color);
    border-radius: 8px;
    padding: 16px 20px;
    margin: 24px 0;
  }

  .myth-callout-label {
    font-family: var(--heading-font-family);
    font-size: var(--body-font-size-s);
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    color: var(--link-color);
    margin: 0 0 8px;
  }

  .myth-callout p:not(.myth-callout-label) {
    margin: 0;
    color: var(--text-color);
  }
  ```

- [ ] **Step 3: Lint**

  ```bash
  npm run lint
  ```
  Expected: 0 errors.

- [ ] **Step 4: Verify in local dev**

  ```bash
  aem up --no-open &
  sleep 3
  curl -s http://localhost:3000/ -o /dev/null -w "%{http_code}\n"
  ```
  Expected: `200`. (The default demo page won't contain a myth-callout
  block yet — this just confirms the dev server still boots cleanly with
  the new files present. Stop the server after: `pkill -f "aem up"`.)

- [ ] **Step 5: Commit**

  ```bash
  git add blocks/myth-callout
  git commit -m "Add myth-callout block"
  ```

---

### Task 2: Build the `section-hub` block

**Files:**
- Create: `blocks/section-hub/section-hub.js`
- Create: `blocks/section-hub/section-hub.css`

**Interfaces:**
- Consumes: `/query-index.json`, shaped
  `{ total, offset, limit, data: [{ path, title, section, subtopic, format, lastModified, ... }] }`
  (Adobe's standard query-index response shape) — depends on the
  query-index being configured with `section`, `subtopic`, `format` as
  indexed properties (Task 4, manual, not yet done).
- Produces: a `.section-hub-list` rendered into the block; no other code
  depends on its internals.

- [ ] **Step 1: Write the block JS**

  ```javascript
  // blocks/section-hub/section-hub.js
  function currentSection() {
    return window.location.pathname.split('/').filter(Boolean)[0] || '';
  }

  function renderItem(item) {
    const li = document.createElement('li');
    li.className = 'section-hub-item';

    const link = document.createElement('a');
    link.href = item.path;
    link.textContent = item.title || item.path;
    li.append(link);

    const meta = document.createElement('span');
    meta.className = 'section-hub-item-meta';
    const parts = [item.format === 'video' ? 'Video' : 'Article'];
    if (item.subtopic) parts.push(item.subtopic);
    meta.textContent = parts.join(' · ');
    li.append(meta);

    return li;
  }

  export default async function decorate(block) {
    block.textContent = '';
    const section = currentSection();
    const ul = document.createElement('ul');
    ul.className = 'section-hub-list';

    try {
      const resp = await fetch('/query-index.json');
      if (!resp.ok) throw new Error(`query-index request failed: ${resp.status}`);
      const json = await resp.json();
      const items = (json.data || [])
        .filter((item) => item.section === section)
        .sort((a, b) => (b.lastModified || 0) - (a.lastModified || 0));

      if (items.length === 0) {
        const empty = document.createElement('p');
        empty.textContent = 'No content published in this section yet.';
        block.append(empty);
        return;
      }

      items.forEach((item) => ul.append(renderItem(item)));
      block.append(ul);
    } catch (e) {
      const error = document.createElement('p');
      error.textContent = 'Unable to load content list right now.';
      block.append(error);
    }
  }
  ```

- [ ] **Step 2: Write the block CSS**

  ```css
  /* blocks/section-hub/section-hub.css */
  .section-hub-list {
    list-style: none;
    margin: 0;
    padding: 0;
  }

  .section-hub-item {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    gap: 16px;
    padding: 16px 0;
    border-bottom: 1px solid var(--light-color);
  }

  .section-hub-item a {
    font-size: var(--body-font-size-m);
    color: var(--text-color);
    text-decoration: none;
  }

  .section-hub-item a:hover {
    color: var(--link-hover-color);
  }

  .section-hub-item-meta {
    font-size: var(--body-font-size-xs);
    color: var(--dark-color);
    white-space: nowrap;
  }
  ```

- [ ] **Step 3: Lint**

  ```bash
  npm run lint
  ```
  Expected: 0 errors.

- [ ] **Step 4: Commit**

  ```bash
  git add blocks/section-hub
  git commit -m "Add section-hub block"
  ```

---

### Task 3: Build the `home-recent` block

**Files:**
- Create: `blocks/home-recent/home-recent.js`
- Create: `blocks/home-recent/home-recent.css`

**Interfaces:**
- Consumes: `/query-index.json`, same shape as Task 2.
- Produces: a `.home-recent-list` of up to 5 items, globally sorted —
  deliberately unbalanced across sections (per the UX spec's grilled
  and accepted decision).

- [ ] **Step 1: Write the block JS**

  ```javascript
  // blocks/home-recent/home-recent.js
  const MAX_ITEMS = 5;

  function renderItem(item) {
    const li = document.createElement('li');
    li.className = 'home-recent-item';

    const link = document.createElement('a');
    link.href = item.path;
    link.textContent = item.title || item.path;
    li.append(link);

    const meta = document.createElement('span');
    meta.className = 'home-recent-item-meta';
    const sectionLabel = item.section
      ? item.section.charAt(0).toUpperCase() + item.section.slice(1)
      : '';
    const formatLabel = item.format === 'video' ? 'Video' : 'Article';
    meta.textContent = [sectionLabel, formatLabel].filter(Boolean).join(' · ');
    li.append(meta);

    return li;
  }

  export default async function decorate(block) {
    block.textContent = '';
    const ul = document.createElement('ul');
    ul.className = 'home-recent-list';

    try {
      const resp = await fetch('/query-index.json');
      if (!resp.ok) throw new Error(`query-index request failed: ${resp.status}`);
      const json = await resp.json();
      const items = (json.data || [])
        .sort((a, b) => (b.lastModified || 0) - (a.lastModified || 0))
        .slice(0, MAX_ITEMS);

      if (items.length === 0) {
        const empty = document.createElement('p');
        empty.textContent = 'No content published yet.';
        block.append(empty);
        return;
      }

      items.forEach((item) => ul.append(renderItem(item)));
      block.append(ul);
    } catch (e) {
      const error = document.createElement('p');
      error.textContent = 'Unable to load recent content right now.';
      block.append(error);
    }
  }
  ```

- [ ] **Step 2: Write the block CSS**

  ```css
  /* blocks/home-recent/home-recent.css */
  .home-recent-list {
    list-style: none;
    margin: 0;
    padding: 0;
    display: grid;
    gap: 12px;
  }

  .home-recent-item {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    gap: 16px;
    padding: 12px 16px;
    background-color: var(--light-color);
  }

  .home-recent-item a {
    font-size: var(--body-font-size-s);
    color: var(--text-color);
    text-decoration: none;
    font-weight: 700;
  }

  .home-recent-item a:hover {
    color: var(--link-hover-color);
  }

  .home-recent-item-meta {
    font-size: var(--body-font-size-xs);
    color: var(--dark-color);
    white-space: nowrap;
  }
  ```

- [ ] **Step 3: Lint**

  ```bash
  npm run lint
  ```
  Expected: 0 errors.

- [ ] **Step 4: Commit**

  ```bash
  git add blocks/home-recent
  git commit -m "Add home-recent block"
  ```

---

### Task 4: Manual/content follow-up (not executable by an agent — needs the user's own da.live/tools.aem.live login)

**Files:** none (da.live content + Adobe admin tooling, no repo files)

This task is documentation, not code — it records what's still needed
before Tasks 1–3's blocks actually render real content. None of it can be
done without the user's own authenticated session.

- [ ] Configure the query-index at `https://tools.aem.live/tools/index-admin/`
  for `jazhou-adobe/eds-unpacked`, indexing `section`, `subtopic`,
  `format` from each page's Metadata block (see the Content Workflow
  spec's Metadata-block decision). Verify by requesting
  `https://main--eds-unpacked--jazhou-adobe.aem.page/query-index.json`
  and confirming it returns indexed items once at least one page is
  published with those metadata keys.
- [ ] Author the `/nav` document in da.live with links to Architecture,
  Migration, Development, and About (per the UX spec's nav decision) —
  `header.js` already fetches this path automatically, no code change
  needed.
- [ ] Compose the home page in da.live: 3 section tiles using the
  existing `cards` block (no new code needed — Architecture, Migration,
  Development as three cards linking to their hub pages), followed by a
  `home-recent` block (built in Task 3).
- [ ] Add a `section-hub` block (Task 2) to the `/architecture`,
  `/migration`, and `/development` hub pages in da.live.
- [ ] Create the `/about` page in da.live with author bio content — plain
  content, no block needed.

---

## Self-Review Notes

- Spec coverage: Section hub (UX spec's Section hub template), Content
  item myth callouts (UX spec's Content Patterns), homepage recency list
  (UX spec's Home template) are all covered by Tasks 1–3. Home tiles and
  nav reuse existing blocks/conventions — correctly identified as content
  tasks, not new code, in Task 4.
- Known gap, not fabricated: "format icon" from the UX spec is
  implemented as a text label ("Article"/"Video"), not a graphical icon —
  no icon assets exist in the repo for this yet. Flagged here rather than
  invented.
- Design-hook fix: the original `myth-callout` CSS used a thick colored
  left border, flagged by an automated design-quality check as a
  recognizable AI-generated-UI pattern. Replaced with a plain tinted
  background + rounded corners, no side accent.
