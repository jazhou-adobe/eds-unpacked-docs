# Site Bootstrap & Hosting Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Finish connecting the already-created `jazhou-adobe/eds-unpacked` EDS
site to Adobe Document Authoring (da.live) as its content source, and set up
local dev tooling, so real content can start being published.

**Architecture:** EDS ("Edge Delivery Services") sites have no build/deploy
step. The `jazhou-adobe/eds-unpacked` GitHub repo holds only code (blocks,
scripts, styles); a separate content source (Document Authoring) holds the
actual pages; the AEM Code Sync GitHub App bridges pushes to `main` with
Adobe's edge infrastructure, which renders pages by fetching from whichever
content source is configured for the site.

**Tech Stack:** Adobe Edge Delivery Services (`adobe/aem-boilerplate`),
Adobe Document Authoring (da.live), AEM Code Sync GitHub App, `@adobe/aem-cli`
for local dev.

## Global Constraints

- Site name: **Edge Delivery Services Unpacked** (from Foundation spec).
- Code repo: `jazhou-adobe/eds-unpacked`. Do not put site code in
  `eds-unpacked-docs` — that repo stays specs/plans only.
- Content source: Document Authoring at `jazhou-adobe/eds-unpacked`
  (`https://content.da.live/jazhou-adobe/eds-unpacked/`), never SharePoint
  or Google Drive.
- Public-only hosting — no gated/private tier (from Foundation spec).
- Any step requiring GitHub App authorization, device-code login, or
  Adobe IMS/admin authentication must be performed by the human, not
  auto-completed by an agent — flag it and wait rather than act on
  credential prompts.

---

## Already Verified (2026-07-23, do not redo)

Confirmed directly against the live repo and URLs — skip re-checking these
unless something below fails unexpectedly:

- `jazhou-adobe/eds-unpacked` exists, is public, default branch `main`,
  and already has the full `aem-boilerplate` structure (`blocks/`,
  `scripts/`, `styles/`, `fonts/`, `icons/`, `package.json`, etc.). No
  `fstab.yaml` — expected, since current EDS ("Helix 5") uses the
  Configuration Service instead of a repo file for content source config.
- AEM Code Sync is installed and working: both
  `https://main--eds-unpacked--jazhou-adobe.aem.page/` and
  `https://main--eds-unpacked--jazhou-adobe.aem.live/` return HTTP 200 and
  render real HTML (the boilerplate's default demo page, "Congrats, you
  are ready to go!").
- The content source has **not** yet been switched to Document
  Authoring — the site is still serving the aem-boilerplate's default
  demo content, not anything from da.live.
- `https://content.da.live/jazhou-adobe/eds-unpacked/`,
  `https://admin.da.live/list/jazhou-adobe/eds-unpacked/`, and
  `https://admin.hlx.page/config/jazhou-adobe/sites/eds-unpacked.json` all
  return HTTP 401 (auth required) when checked unauthenticated — expected,
  and consistent with these needing an authenticated admin/IDP session.

---

### Task 1: Confirm or create initial content in Document Authoring

**Files:** none (da.live content, not repo files)

**Interfaces:**
- Consumes: da.live org `jazhou-adobe`, site `eds-unpacked` (from user).
- Produces: at least one document in da.live at the site's root path, so
  Task 2's content-source switch has something to render instead of a 404.

- [ ] **Step 1: Open da.live and check for existing content**

  Navigate to `https://da.live/#/jazhou-adobe/eds-unpacked` in a browser
  (this requires your own Adobe login — do this yourself, not via an
  agent). Check whether a root/index document already exists.

- [ ] **Step 2: If empty, create a minimal index document**

  In da.live, create a document at the site root (path `/index`) with at
  minimum a title matching the site name, e.g.:

  ```
  # Edge Delivery Services Unpacked

  This site is under construction.
  ```

  Save it in da.live (this creates the document in the content source;
  it does not yet need to be "published" for Task 2's verification, since
  preview reads directly from the source).

- [ ] **Step 3: Note the result**

  Record whether content already existed or was just created — needed
  context for Task 2's verification step.

---

### Task 2: Point the site's content source at Document Authoring

**Files:** none (Adobe Configuration Service, not repo files)

**Interfaces:**
- Consumes: org `jazhou-adobe`, site `eds-unpacked`, content source URL
  `https://content.da.live/jazhou-adobe/eds-unpacked/`, type `markup`.
- Produces: preview/live URLs now render da.live content instead of the
  aem-boilerplate demo page — this is what Task 4 (local dev) and all
  future content/UX work builds on.

- [ ] **Step 1: Open the Site Admin tool**

  Navigate to `https://tools.aem.live`, open the **Site Admin** tool (or
  **Simple Config Editor** if Site Admin doesn't expose content-source
  editing). Authenticate with the GitHub account that installed Code Sync
  on `jazhou-adobe/eds-unpacked` — do this yourself; do not hand
  credentials or device codes to an agent.

- [ ] **Step 2: Set the content source**

  Enter org `jazhou-adobe`, site `eds-unpacked`. Set:
  - Content source URL: `https://content.da.live/jazhou-adobe/eds-unpacked/`
  - Type: `markup`

  Save.

- [ ] **Step 3: Verify preview now serves da.live content**

  Run:
  ```bash
  curl -s https://main--eds-unpacked--jazhou-adobe.aem.page/ | grep -i "Congrats, you are ready to go"
  ```
  Expected: **no match** (empty output). A match means the content source
  switch didn't take effect yet — wait ~30s for cache/propagation and
  retry before assuming failure.

  Then run:
  ```bash
  curl -s https://main--eds-unpacked--jazhou-adobe.aem.page/ | grep -i "Edge Delivery Services Unpacked"
  ```
  Expected: a match, confirming the index document from Task 1 is now
  rendering.

- [ ] **Step 4: Verify live matches**

  Run the same two checks against
  `https://main--eds-unpacked--jazhou-adobe.aem.live/`. Note: live only
  updates after an explicit **Publish** action in da.live's Sidekick (it
  does not auto-publish on save) — if this check fails, publish the
  index document via the AEM Sidekick browser extension first, then
  retry.

---

### Task 3: Install the AEM Sidekick browser extension

**Files:** none (browser extension, human action)

- [ ] **Step 1: Install Sidekick**

  Install the "AEM Sidekick" extension for your browser (Chrome/Edge Web
  Store — search "AEM Sidekick"). This is what future authors use to
  Preview and Publish documents edited in da.live.

- [ ] **Step 2: Verify it recognizes the site**

  Open `https://da.live/#/jazhou-adobe/eds-unpacked`, open the index
  document, confirm the Sidekick extension activates and shows
  Preview/Publish buttons for `jazhou-adobe/eds-unpacked`.

---

### Task 4: Set up local dev tooling for future block development

**Files:**
- Modify: none yet (this task only sets up tooling, no code changes)

**Interfaces:**
- Consumes: `jazhou-adobe/eds-unpacked` repo cloned locally.
- Produces: a working local dev server, needed by the future Website
  Development plan (Workstream 4 in the roadmap) before any block code is
  written.

- [ ] **Step 1: Clone the repo locally**

  ```bash
  gh repo clone jazhou-adobe/eds-unpacked
  cd eds-unpacked
  ```

- [ ] **Step 2: Install dependencies**

  ```bash
  npm install
  ```
  Expected: completes with no errors.

- [ ] **Step 3: Run lint**

  ```bash
  npm run lint
  ```
  Expected: passes (0 errors) on the untouched boilerplate.

- [ ] **Step 4: Install the AEM CLI and start local dev**

  ```bash
  npm install -g @adobe/aem-cli
  aem up
  ```
  Expected: prints a local URL (typically `http://localhost:3000`).

- [ ] **Step 5: Verify local preview**

  ```bash
  curl -s -o /dev/null -w "%{http_code}\n" http://localhost:3000/
  ```
  Expected: `200`. Stop the dev server (Ctrl+C) once confirmed.

---

### Task 5: Record the hosting setup for future reference

**Files:**
- Create: `docs/superpowers/plans/2026-07-23-hosting-reference.md`

**Interfaces:**
- Consumes: results from Tasks 1–4.
- Produces: a short reference doc future plans (Content Workflow, Website
  Development) can link to instead of re-deriving these facts.

- [ ] **Step 1: Write the reference doc**

  ```markdown
  # Hosting Reference — Edge Delivery Services Unpacked

  - Code repo: https://github.com/jazhou-adobe/eds-unpacked
  - Content source: Document Authoring, https://da.live/#/jazhou-adobe/eds-unpacked
  - Preview: https://main--eds-unpacked--jazhou-adobe.aem.page/
  - Live: https://main--eds-unpacked--jazhou-adobe.aem.live/
  - Config/admin tools: https://tools.aem.live (Site Admin / Simple Config Editor)
  - Local dev: `npm install -g @adobe/aem-cli && aem up` from a clone of
    the code repo, serves at http://localhost:3000

  Set up 2026-07-23. See
  `docs/superpowers/plans/2026-07-23-site-bootstrap-hosting.md` for the
  steps that produced this.
  ```

- [ ] **Step 2: Commit**

  ```bash
  git add docs/superpowers/plans/2026-07-23-hosting-reference.md
  git commit -m "Add hosting reference doc for Edge Delivery Services Unpacked"
  ```
