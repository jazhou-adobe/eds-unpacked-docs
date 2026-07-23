# Edge Delivery Services Unpacked — UX / Site Structure Design

## Context

Third of four planned sub-projects (see
`docs/superpowers/plans/2026-07-23-job-breakdown-roadmap.md`):

1. Foundation & Personas (done —
   `docs/superpowers/specs/2026-07-23-foundation-and-personas-design.md`)
2. Content Architecture / Content Workflow (not yet designed)
3. **UX / Site Structure** (this document)
4. Website Development (blocked on this document)

This document also resolves the topic pillars that the Foundation spec
deliberately left open.

## Page Hierarchy

Three levels:

- **Home** — directory/landing page, not a content list by default.
- **Section hub** — one per top-level section, lists all content in that
  section.
- **Content item** — an individual article or video page.

## Top-Level Sections (topic pillars, now finalized)

Three sections, replacing the earlier 7-pillar draft:

1. **Architecture** — how EDS works, technical concepts, and the parts of
   enterprise governance that are architectural (e.g., security/compliance
   model).
2. **Migration** — moving from AEM 6.5 / AEM Managed Services / another CMS
   to EDS, including the migration-specific governance concerns (support
   model, cutover planning).
3. **Development** — hands-on content: authoring experience, performance /
   Core Web Vitals, developer tooling and workflow. Carries three
   sub-topics as labels (not separate nav items): **Authoring**,
   **Performance**, **Tooling**.

Myths & misconceptions are **not** a section — see Content Patterns below.

Development is expected to grow faster than Architecture or Migration
since it absorbs three sub-topics; this is an accepted, known imbalance,
not a problem to solve now (see Open Items).

## Page Templates

Three reusable templates, one per hierarchy level (Home is singular,
Section hub repeats 3x, Content item repeats per article/video). The
About page (see Navigation) is a fourth, one-off static page outside this
hierarchy — it's never repeated, so it doesn't need a reusable template,
just its own page.

- **Home template**: 3 section tiles (Architecture, Migration,
  Development) and a "recently added" list (3-5 most recent items across
  all sections, reverse-chronological). No bio content here — that lives
  on the separate About page (see Navigation).
- **Section hub template**: flat, reverse-chronological list of all
  content items in that section. Each row shows: title, a format icon
  (article vs. video), and — for Development only — a sub-topic label
  (Authoring/Performance/Tooling). Labels are plain text, not clickable
  filters.
- **Content item template**: shared by articles and videos — one layout,
  not two. The main content block is either article prose or a video
  embed. Myth-busting callouts can appear inline anywhere in the body.

## Navigation

Global nav: logo (links home) + Architecture + Migration + Development +
About. Nothing else. The About page carries the author bio/credibility
content (workshop experience, Adobe background) as a full page in its own
right, since a homepage-only blurb was judged insufficient for the
portfolio audience from the Foundation spec.

## Content Patterns

- **Myths & misconceptions**: a reusable inline callout component/pattern,
  used within any content item's body wherever relevant. No dedicated
  page, no cross-section aggregation, no separate tagging system for it.
- **Persona tags**: per the Foundation spec, these remain author-only
  frontmatter metadata. They never appear in this UX — no visitor-facing
  filter, badge, or path. The sub-topic labels under Development are the
  only tags visitors actually see, and they are plain labels, not filters.
- **Discovery**: no search, no filtering for v1. Section hubs are simple
  flat lists. This is a deliberate, open-ended deferral (see Open Items),
  not a permanent constraint.

## Scope Boundaries (explicitly out of scope for this spec)

- Visual design (typography, color, spacing) — not addressed here.
- Search/filtering implementation — deferred, no trigger threshold set.
- The Markdown → Document Authoring content workflow — separate
  sub-project (Content Architecture / Content Workflow).
- Actual block/component implementation — Website Development plan,
  built on this spec.

## Open Items

- No concrete trigger for revisiting search/filtering as content grows
  (deliberately left open-ended rather than a fixed item-count threshold).
- Development's faster growth relative to Architecture/Migration is
  accepted as-is; no rebalancing plan exists if it becomes very lopsided.
