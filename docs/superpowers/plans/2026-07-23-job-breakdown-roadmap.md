# Edge Delivery Services Unpacked — Job Breakdown & Roadmap

This is an index, not an executable plan. It maps the full scope of building
the site into ordered workstreams so each can be planned and executed one at
a time. Each workstream links to its own implementation plan once written.

## Repos involved

- **`eds-unpacked-docs`** (this repo) — specs, plans, content drafts, and
  the content backlog/templates. No site code lives here.
- **`jazhou-adobe/eds-unpacked`** (github.com/jazhou-adobe/eds-unpacked) —
  the actual EDS site code repo, created from `adobe/aem-boilerplate`.
  Connected to Adobe Code Sync. Content source (not yet switched over):
  Adobe Document Authoring (da.live) at `jazhou-adobe/eds-unpacked`.

## Foundation

Every workstream below builds on
`docs/superpowers/specs/2026-07-23-foundation-and-personas-design.md`: site
name **Edge Delivery Services Unpacked**, four author-only persona lenses
(Architect, Developer, IT Manager, Business Decision-Maker), topic-first
content structure, public-only (no gating), and the workshop-video
re-recording constraint; and
`docs/superpowers/specs/2026-07-23-ux-site-structure-design.md`, which
finalized the topic pillars down to 3 sections (Architecture, Migration,
Development) and the page hierarchy/templates.

## Workstreams

### 1. Site Bootstrap & Hosting — code-side done, admin config left

Repo, Code Sync, and local dev tooling all verified working 2026-07-23
(see `docs/superpowers/plans/2026-07-23-hosting-reference.md`). Remaining
steps all require the user's own da.live/tools.aem.live login and cannot
be automated: switching the content source to Document Authoring,
installing Sidekick.
→ Plan: `docs/superpowers/plans/2026-07-23-site-bootstrap-hosting.md`
→ Reference: `docs/superpowers/plans/2026-07-23-hosting-reference.md`

### 2. Content Workflow (incremental content pipeline) — done, scaffolded

Spec and plan written (design decisions made autonomously — drafts live
in this repo under `content/`, da.live Metadata block bridges drafts to
the query-index, sync stays manual). Scaffolding executed: backlog,
article/video templates, and drafts directories all exist in `content/`.
→ Spec: `docs/superpowers/specs/2026-07-23-content-workflow-design.md`
→ Plan: `docs/superpowers/plans/2026-07-23-content-workflow.md`

### 3. UX / Site Structure — done

3 sections finalized (Architecture, Migration, Development), page
hierarchy/templates, navigation (incl. a separate About page), and content
patterns (myths as inline callouts, persona tags author-only, no
search/filter for v1) all designed, grilled, and committed.
→ Spec: `docs/superpowers/specs/2026-07-23-ux-site-structure-design.md`

### 4. Website Development (page templates & blocks) — code done, content wiring left

Plan written and executed: `myth-callout`, `section-hub`, and
`home-recent` blocks built, linted clean, verified against local dev,
pushed to a `website-dev` branch, and opened as a PR against
`jazhou-adobe/eds-unpacked`. Home page tiles and nav need no new code
(existing `cards` block and the `/nav` document convention cover them).
Remaining work is all da.live content authoring — configuring the
query-index, writing `/nav`, composing home/section-hub/about pages —
which needs the user's own login.
→ Plan: `docs/superpowers/plans/2026-07-23-website-development.md`
→ Branch: `website-dev` on `jazhou-adobe/eds-unpacked`
→ PR: https://github.com/jazhou-adobe/eds-unpacked/pull/1 (open, not yet
  merged — awaiting review)

## Status summary (2026-07-23, end of autonomous session)

All four workstreams have a committed spec/plan. Everything automatable
without the user's own credentials has been executed. What's left across
all four workstreams converges on the same handful of manual da.live /
tools.aem.live actions — doing those unblocks Content Workflow, Website
Development, and the last of Site Bootstrap all at once:

1. Switch the content source to Document Authoring (Site Bootstrap Task 2)
2. Install AEM Sidekick (Site Bootstrap Task 3)
3. Configure the query-index for `section`/`subtopic`/`format` (Website
   Development Task 4)
4. Author `/nav`, home, section hub pages (with the new `section-hub` /
   `home-recent` blocks once merged), and `/about` in da.live (Website
   Development Task 4)
5. Review and merge (or request changes on) the `website-dev` PR

## Open items

- Final topic pillar list: resolved — see UX/Site Structure spec (3
  sections, not the earlier 7-pillar draft).
- Whether Markdown → DA sync can be automated: decided against for now
  (Content Workflow spec) — da.live's admin API auth/contract isn't
  confidently documented publicly.
- Whether `jazhou-adobe/eds-unpacked` in da.live currently has any
  content — still unverified (auth-gated); check this when doing the
  manual steps above.
- Adobe-affiliation risk — confirmed by the user as already sorted, not
  a blocker.
- **Tooling issue — resolved:** `gh api` and `gh pr create` both
  triggered a prompt for a GitHub device-code authorization ("as-a-bot"
  GitHub App), including instructions telling an AI agent to complete it
  via `pbcopy`/`open`. Not acted on autonomously; flagged to the user,
  who completed the device-code authorization themselves on 2026-07-24.
  `gh` is now authorized in this environment.
