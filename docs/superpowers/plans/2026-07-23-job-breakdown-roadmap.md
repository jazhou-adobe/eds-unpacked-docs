# Edge Delivery Services Unpacked — Job Breakdown & Roadmap

This is an index, not an executable plan. It maps the full scope of building
the site into ordered workstreams so each can be planned and executed one at
a time. Each workstream links to its own implementation plan once written.

## Repos involved

- **`eds-unpacked-docs`** (this repo) — specs, plans, and roadmap only. No
  site code lives here.
- **`jazhou-adobe/eds-unpacked`** (github.com/jazhou-adobe/eds-unpacked) —
  the actual EDS site code repo, created from `adobe/aem-boilerplate`.
  Connected to Adobe Code Sync. Content source: Adobe Document Authoring
  (da.live) at `jazhou-adobe/eds-unpacked`.

## Foundation

Every workstream below builds on
`docs/superpowers/specs/2026-07-23-foundation-and-personas-design.md`: site
name **Edge Delivery Services Unpacked**, four author-only persona lenses
(Architect, Developer, IT Manager, Business Decision-Maker), topic-first
content structure with topic pillars deliberately unfixed, public-only (no
gating), and the workshop-video re-recording constraint.

## Workstreams

### 1. Site Bootstrap & Hosting — mostly done, one config step left

Verified 2026-07-23: `jazhou-adobe/eds-unpacked` already exists, already has
the `aem-boilerplate` structure, and Code Sync is already installed and
working — both `https://main--eds-unpacked--jazhou-adobe.aem.page/` and the
`.aem.live` equivalent return HTTP 200 and serve the boilerplate's default
demo content. What's left: point the content source at Document Authoring
(`jazhou-adobe/eds-unpacked` in da.live) instead of the default demo content,
and set up local dev tooling.
→ Plan: `docs/superpowers/plans/2026-07-23-site-bootstrap-hosting.md`

### 2. Content Workflow (incremental content pipeline) — needs one more research pass before planning

Build the repeatable mechanism for turning a workshop topic into published
content: a persona/topic frontmatter schema (per Foundation spec), a
Markdown drafting template + backlog convention in `eds-unpacked-docs`, and
a Markdown → Document Authoring sync step. The sync step's exact mechanism
(da.live's admin API isn't fully documented publicly) needs verification
before it can be written as a no-placeholder plan; a manual "paste into
da.live" step is the honest fallback if automation isn't feasible.
→ Not yet written, next up.

### 3. UX / Site Structure — needs its own brainstorm first

Navigation, page templates (home, topic index, article, video), and how the
site actually presents topic-first content. No finalized design exists yet
(some exploration in Google Stitch, nothing to import) — this needs a proper
brainstorming → spec cycle before a plan can be written, per the hard gate
against planning undesigned work.
→ Not started.

### 4. Website Development (page templates & blocks) — blocked on #3

Building the actual EDS blocks/templates for home, article, video, and
topic-index pages. Depends on UX Structure decisions from #3; basic
technical scaffolding (block architecture, linting, local dev) is covered by
#1.
→ Not started, blocked on #3.

## Suggested sequencing

1. Finish Site Bootstrap & Hosting (small remaining step — unblocks
   publishing real content)
2. Content Workflow (can run in parallel with #3; doesn't depend on final UX)
3. UX / Site Structure brainstorm → spec
4. Website Development plan, built on #3's spec

## Open items

- Final topic pillar list (deferred, per Foundation spec)
- Whether Markdown → DA sync can be automated or stays a manual step
  (Content Workflow workstream)
- Whether `jazhou-adobe/eds-unpacked` in da.live currently has any content —
  could not verify (auth-gated); first Content Workflow task should check.
- The `gh` CLI in this environment prompted for a GitHub device-code
  authorization ("as-a-bot" app) mid-session — flagged to the user, not
  acted on autonomously. Worth confirming this is expected tooling.
- Adobe-affiliation risk (public site under `jazhou-adobe`, Adobe product
  content, Adobe employee) — raised during plan review; confirmed by the
  user as already sorted, not a blocker.
