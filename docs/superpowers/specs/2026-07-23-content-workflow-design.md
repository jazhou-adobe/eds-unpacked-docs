# Edge Delivery Services Unpacked — Content Workflow Design

## Context

Second of four planned sub-projects (see
`docs/superpowers/plans/2026-07-23-job-breakdown-roadmap.md`). Written
autonomously — the user asked to proceed without being asked further
questions for a 3-hour window, so every decision below that would
normally have gone through the brainstorming Q&A was made by the author
(Claude) directly and is flagged as such. **Treat every "Decision (made
autonomously)" below as reversible — flag anything you'd have answered
differently.**

Builds on:
- `docs/superpowers/specs/2026-07-23-foundation-and-personas-design.md`
  (site name, four author-only persona lenses, workshop-video
  re-recording constraint, no gating)
- `docs/superpowers/specs/2026-07-23-ux-site-structure-design.md` (3
  sections — Architecture, Migration, Development — page hierarchy,
  one-section-per-item rule, governance placement test)

## Purpose

A repeatable mechanism for turning a workshop topic into a published
piece of content, usable incrementally with no fixed content calendar or
pre-committed list of articles/videos.

## Decision (made autonomously): where drafts live

Markdown content drafts live in **this repo** (`eds-unpacked-docs`), under
a new top-level `content/` directory — not in the `jazhou-adobe/eds-unpacked`
code repo, and not in a third repo. Rationale: `eds-unpacked-docs` was
already designated the non-code home for this project (specs, plans); a
Markdown content draft is not site code, so it fits here rather than
mixing authoring material into the blocks/scripts/styles repo. If this
turns out wrong (e.g., you'd rather drafts live alongside the code, or in
their own repo), it's a low-cost thing to move later — nothing else
depends on the exact repo, only on the frontmatter schema below.

```
content/
  backlog.md              # ideas not yet drafted
  templates/
    article.md
    video.md
  drafts/
    architecture/
    migration/
    development/
```

## Decision (made autonomously): frontmatter schema

Every draft starts with this frontmatter block:

```yaml
---
title: ""
section: architecture   # architecture | migration | development
subtopic: ""            # only set when section: development — authoring | performance | tooling
format: article          # article | video
persona: []              # author-only planning tag(s): architect | developer | it-manager | business-decision-maker
status: draft             # draft | ready-for-da | published
source_workshop: ""       # private note only — which workshop inspired this; never copied into da.live
video_url: ""             # only set when format: video, once the re-recorded video is hosted somewhere
---
```

`persona` and `source_workshop` are planning aids only, per the Foundation
spec's decision that personas are author-only metadata — neither field is
ever copied into the published da.live document or exposed to visitors.

## Decision (made autonomously): the section/subtopic/format values become a da.live Metadata block, not just draft frontmatter

EDS pages expose page metadata (title, description, and custom keys) via
a **Metadata block** — a two-column key/value table placed at the end of
the document — which Edge Delivery's indexing (query-index) reads to
produce a machine-readable list of pages. For a Section hub page (per the
UX spec) to list "all content in that section" without hand-maintained
links, each published document needs a Metadata block carrying at least
`section`, `subtopic` (Development only), and `format` — **not**
`persona` or `source_workshop`, which stay private to the Markdown draft
and are never carried into da.live.

**Flag for the Website Development plan:** the exact current mechanism for
configuring a query-index (still a repo-level `helix-query.yaml`, or also
moved to the Configuration Service the way content-source config was) was
not verified during this session — Website Development's plan must verify
this before building the Section hub block, since it's what that block
will query against.

## Workflow (repeatable, one piece of content at a time)

1. **Capture the idea.** Add a row to `content/backlog.md`: topic, target
   section, format, one-line description. No commitment to when it gets
   written.
2. **Draft.** Copy the matching template from `content/templates/` into
   `content/drafts/<section>/<slug>.md`. Fill in frontmatter and body.
   For video: the raw workshop recording is never used directly (per the
   Foundation spec's video constraint) — this step is where the generic,
   re-recorded version gets made; `video_url` stays blank until that
   exists.
3. **Mark ready.** Once the draft is complete, set `status: ready-for-da`
   and commit it to `eds-unpacked-docs` (keeps full history of every
   draft, independent of da.live).
4. **Move into Document Authoring.** Manually create the matching
   document in da.live at `jazhou-adobe/eds-unpacked`, at the path
   matching its section (see path convention below), paste the body, and
   add a Metadata block with `section` / `subtopic` / `format`.
5. **Preview and publish.** Use the AEM Sidekick extension (installed per
   the Site Bootstrap plan) to Preview, verify it renders correctly, then
   Publish.
6. **Close the loop.** Set `status: published` in the original Markdown
   draft and commit. The draft file becomes a historical record — da.live
   is the live source of truth from this point on, per the Foundation
   spec's decision.

## Decision (made autonomously): URL path convention

Matches the UX spec's page hierarchy directly:

- `/architecture/<slug>`
- `/migration/<slug>`
- `/development/<slug>`
- `/about` (the one-off About page, not part of this workflow)
- `/` (home)

## Decision (made autonomously): sync stays manual, not automated

Adobe Document Authoring's admin API (`admin.da.live`) exists but its
auth mechanism and exact endpoint contract are not confidently documented
in public sources (verified during the Site Bootstrap research pass).
Rather than script an integration against a reverse-engineered API,
Step 4 above stays a manual copy-into-da.live action. Automating this is
a legitimate future improvement, but building it now risks depending on
undocumented behavior that could change without notice.

## Scope Boundaries (explicitly out of scope for this spec)

- Automating the Markdown → da.live sync (see decision above).
- The exact query-index configuration mechanism — deferred to Website
  Development, flagged above.
- Visual design of the backlog/template files themselves — plain
  Markdown, no tooling beyond a text editor needed.
- A fixed content calendar or list of committed articles/videos — this
  workflow is deliberately calendar-agnostic, per the original request.

## Open Items

- Query-index configuration mechanism unverified (flagged above, blocks
  Website Development's Section hub block).
- Whether to eventually automate the da.live sync once its API is better
  understood — not attempted in this session.
