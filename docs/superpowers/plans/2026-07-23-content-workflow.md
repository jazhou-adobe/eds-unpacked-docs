# Content Workflow Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Scaffold the incremental content workflow described in
`docs/superpowers/specs/2026-07-23-content-workflow-design.md` — backlog
file, article/video templates, and drafts directories — so new content
ideas can be captured and drafted immediately, with no fixed content list
required.

**Architecture:** Plain Markdown files in `eds-unpacked-docs/content/`.
No tooling, scripts, or build step — this is authoring scaffolding, not
code.

**Tech Stack:** Markdown, git.

## Global Constraints

- Drafts live in `eds-unpacked-docs` (this repo), not the
  `jazhou-adobe/eds-unpacked` code repo.
- Frontmatter schema is exactly as defined in the Content Workflow spec:
  `title`, `section`, `subtopic`, `format`, `persona`, `status`,
  `source_workshop`, `video_url`.
- `persona` and `source_workshop` must never be copied into da.live —
  templates must carry a comment saying so, to prevent future mistakes.
- Sections are exactly: `architecture`, `migration`, `development` (per
  the UX spec) — subtopic only applies within `development`
  (`authoring` | `performance` | `tooling`).

---

### Task 1: Create the content backlog file

**Files:**
- Create: `content/backlog.md`

- [ ] **Step 1: Write the file**

  ```markdown
  # Content Backlog — Edge Delivery Services Unpacked

  Ideas not yet drafted. Add a row any time a workshop question or topic
  seems worth writing up — no commitment to when it gets drafted.

  | Idea | Section | Format | Notes |
  |---|---|---|---|
  | Why EDS has no build step | architecture | article | (example — delete once you have real ideas) |

  Once you start drafting an idea, remove its row here and create the
  draft file instead (see `content/templates/`).
  ```

- [ ] **Step 2: Verify**

  ```bash
  test -f content/backlog.md && echo "exists"
  ```
  Expected: `exists`

- [ ] **Step 3: Commit**

  ```bash
  git add content/backlog.md
  git commit -m "Add content backlog file"
  ```

---

### Task 2: Create the article template

**Files:**
- Create: `content/templates/article.md`

- [ ] **Step 1: Write the file**

  ```markdown
  ---
  title: ""
  section: architecture   # architecture | migration | development
  subtopic: ""            # only if section: development — authoring | performance | tooling
  format: article
  persona: []              # author-only planning tag(s) — architect | developer | it-manager | business-decision-maker. NEVER copy this into da.live.
  status: draft             # draft | ready-for-da | published
  source_workshop: ""       # private note only — NEVER copy this into da.live
  ---

  <!--
    Write the article body below. When status becomes ready-for-da,
    copy this body into a new document in da.live at
    jazhou-adobe/eds-unpacked, at the path matching the section
    (e.g. /architecture/<slug>), and add a Metadata block there with
    section / subtopic / format only (not persona or source_workshop).
    See docs/superpowers/specs/2026-07-23-content-workflow-design.md
    for the full workflow.
  -->

  # Title goes here

  Body goes here.
  ```

- [ ] **Step 2: Verify**

  ```bash
  test -f content/templates/article.md && grep -q "status: draft" content/templates/article.md && echo "ok"
  ```
  Expected: `ok`

- [ ] **Step 3: Commit**

  ```bash
  git add content/templates/article.md
  git commit -m "Add article content template"
  ```

---

### Task 3: Create the video template

**Files:**
- Create: `content/templates/video.md`

- [ ] **Step 1: Write the file**

  ```markdown
  ---
  title: ""
  section: architecture   # architecture | migration | development
  subtopic: ""            # only if section: development — authoring | performance | tooling
  format: video
  persona: []              # author-only planning tag(s) — architect | developer | it-manager | business-decision-maker. NEVER copy this into da.live.
  status: draft             # draft | ready-for-da | published
  source_workshop: ""       # private note only — which workshop inspired this. NEVER copy this into da.live.
  video_url: ""             # only fill in once the RE-RECORDED generic version is hosted somewhere. Never link a raw customer workshop recording.
  ---

  <!--
    IMPORTANT: per the Foundation spec's video constraint, raw customer
    workshop recordings are never published. Re-record a generic version
    with no customer specifics first, host it, and only then fill in
    video_url above.

    When status becomes ready-for-da, create a new document in da.live
    at jazhou-adobe/eds-unpacked, at the path matching the section
    (e.g. /development/<slug>), embed the video, and add a Metadata
    block with section / subtopic / format only.
  -->

  # Title goes here

  Short written intro/summary goes here (videos still get a title and a
  couple of sentences of context, per the shared content-item template
  in the UX spec).
  ```

- [ ] **Step 2: Verify**

  ```bash
  test -f content/templates/video.md && grep -q "video_url" content/templates/video.md && echo "ok"
  ```
  Expected: `ok`

- [ ] **Step 3: Commit**

  ```bash
  git add content/templates/video.md
  git commit -m "Add video content template"
  ```

---

### Task 4: Create the drafts directory structure

**Files:**
- Create: `content/drafts/architecture/.gitkeep`
- Create: `content/drafts/migration/.gitkeep`
- Create: `content/drafts/development/.gitkeep`

- [ ] **Step 1: Create the directories with placeholder files**

  ```bash
  mkdir -p content/drafts/architecture content/drafts/migration content/drafts/development
  touch content/drafts/architecture/.gitkeep content/drafts/migration/.gitkeep content/drafts/development/.gitkeep
  ```

- [ ] **Step 2: Verify**

  ```bash
  ls content/drafts/architecture content/drafts/migration content/drafts/development
  ```
  Expected: each lists `.gitkeep`

- [ ] **Step 3: Commit**

  ```bash
  git add content/drafts
  git commit -m "Add drafts directory structure for content workflow"
  ```
