# Edge Delivery Services Unpacked — Content Catalogue & Job Skill Design

## Context

Covers two related deliverables for the content-creation phase:

1. **The content catalogue** — the concrete list of article "jobs" to
   create, derived from the author's topic ideas mapped onto the site's
   3 sections.
2. **The idea-to-jobs skill** — a reusable skill that turns one raw idea
   into a structured job breakdown, so the catalogue can grow the same
   way over time.

Builds on and stays consistent with:
- `docs/superpowers/specs/2026-07-23-foundation-and-personas-design.md`
  (personas as author-only lens, workshop-video re-recording constraint,
  public/no-gating)
- `docs/superpowers/specs/2026-07-23-ux-site-structure-design.md`
  (3 sections, one-section-per-item rule, governance placement test, no
  visitor search/filter for v1)
- `docs/superpowers/specs/2026-07-23-content-workflow-design.md`
  (drafts in `content/`, frontmatter schema, manual Markdown → DA sync,
  Metadata block bridge)

This spec **amends** the content-workflow frontmatter schema by adding a
`tags` field (see Job Anatomy); templates in `content/templates/` will
need updating to match (implementation follow-on, not done here).

## Definitions

- **Job** = one article (the required deliverable) plus an optional
  companion video. One idea = one job. One job = one draft file = one DA
  page.
- **Done** = the article is published. The video is an optional
  enhancement: a job can publish text-first with `video_url` blank, and
  the video is added later without blocking or re-publishing.

## The Content Catalogue (17 jobs)

Each job: `slug` is the draft filename (`content/drafts/<section>/<slug>.md`)
and the DA page path (`/<section>/<slug>`). Tags are author-only
orthogonal keywords (they never repeat the section or sub-topic name —
see Tag Rules).

### Architecture (6)

| # | Title | slug | tags |
|---|---|---|---|
| A1 | Edge Delivery Services architecture walkthrough | `architecture-walkthrough` | overview, delivery-patterns |
| A2 | Why EDS is fast | `why-eds-is-fast` | edge, caching, speed |
| A3 | Martech integration & integration patterns | `martech-integration-patterns` | martech, integrations |
| A4 | Edge workers & edge functions: an introduction | `edge-workers-and-functions` | edge-workers, edge-functions |
| A5 | Environment strategy: how many non-prod environments do you need? | `environment-strategy` | environments, governance |
| A6 | Hosting authenticated content & handling authorization | `authenticated-content-authorization` | security, authentication, authorization |

### Migration (4)

| # | Title | slug | tags |
|---|---|---|---|
| M1 | The modernisation agent: your migration co-pilot | `modernisation-agent` | modernisation-agent, ai |
| M2 | Migration approaches & readiness assessment | `migration-approaches-assessment` | assessment, planning |
| M3 | From AEM 6.5 to EDS: what actually changes | `aem65-to-eds-what-changes` | aem-6-5, comparison |
| M4 | Migration governance: cutover, support model & rollback | `migration-governance-cutover` | governance, cutover, support |

### Development (7)

| # | Title | slug | sub-topic | tags |
|---|---|---|---|---|
| D1 | How we optimised our Lighthouse score | `optimising-lighthouse-score` | Performance | lighthouse, core-web-vitals |
| D2 | Image optimisation & Dynamic Media integration | `image-optimisation-dynamic-media` | Performance | images, dynamic-media |
| D3 | The new development lifecycle | `new-development-lifecycle` | Tooling | lifecycle, workflow |
| D4 | Using AI to work with AEM | `using-ai-with-aem` | Tooling | ai |
| D5 | Admin API usage | `admin-api-usage` | Tooling | admin-api |
| D6 | Multi-Site Manager with content fragments | `msm-content-fragments` | Authoring | msm, content-fragments |
| D7 | JSON to HTML & content overlay | `json-to-html-content-overlay` | Authoring | json, content-overlay |

**Placement notes (one section per item, per UX spec):**
- A2 "Why EDS is fast" (conceptual *how it works*) is Architecture; D1
  "Lighthouse" (hands-on *how to tune*) is Development/Performance —
  deliberately split to avoid overlap.
- D7 "JSON to HTML & content overlay" is borderline Architecture; placed
  in Development/Authoring as it concerns the authoring/rendering
  approach.

## Job Anatomy

Every job is one draft markdown file at
`content/drafts/<section>/<slug>.md`, following `content/templates/`,
with:

- **Frontmatter**: `title`, `section`, `subtopic` (Development only),
  `format` (`article`; video noted via `video_url` once recorded),
  `persona` (author-only planning), **`tags`** (author-only — new field,
  see Tag Rules), `status`, `source_workshop`, `video_url`.
- **Article body**: 5–10 min read (~800–1,600 words), plain prose,
  simple structure — intro, 3–6 H2 sections, optional inline
  **myth-callout** where a misconception fits, a short "what's next"
  pointer.
- **Video companion (optional)**: a ≤5-min outline/beat sheet captured in
  the job breakdown (not embedded in the article draft). The recorded,
  generic (never raw-workshop, per Foundation spec) video's URL goes into
  `video_url` when available.

## Tag Rules

- Tags are **author-only** frontmatter. They do **not** drive any
  visitor-facing search or filter in v1 (that stays deferred per the UX
  spec). They exist for the author to organize and grep a growing
  library. A visitor-facing search is a possible later enhancement.
- Tags **exclude** the section names (`architecture`, `migration`,
  `development`) and the sub-topic names (`authoring`, `performance`,
  `tooling`) — those are already captured by the `section`/`subtopic`
  fields. Tags carry only orthogonal topic keywords.
- Tags are **never** copied into the DA Metadata block (only
  `section`/`subtopic`/`format` cross into da.live, per the content-workflow
  spec).
- **Controlled starter vocabulary** (reuse-first; new tags allowed but
  prefer existing): overview, delivery-patterns, edge, caching, speed,
  martech, integrations, edge-workers, edge-functions, environments,
  governance, security, authentication, authorization,
  modernisation-agent, ai, assessment, planning, aem-6-5, comparison,
  cutover, support, lighthouse, core-web-vitals, images, dynamic-media,
  lifecycle, workflow, admin-api, msm, content-fragments, json,
  content-overlay.

## The Idea-to-Jobs Skill

A reusable skill (invocable via the Skill tool) that turns one topic,
tag, or raw idea into a **research-backed content-preparation package**
plus the executable steps to get it published. By default it prepares the
plan and content scaffolding, not finished prose; on explicit
instruction it can also draft the full video script and/or full article
prose.

**Input:** a topic, one or more tags, or a rough idea — a sentence,
paragraph, or a catalogue title.

### Step 1 — Research (always runs first)

The skill researches the topic before planning, against current,
high-trust sources (official EDS/AEM documentation first — `aem.live`,
`da.live`, `adobe.com` — over blogs/forums):

- **Topic facts:** verify the technical content is accurate and current
  (EDS moves fast; do not rely on stale model knowledge). Cite the
  sources used and explicitly flag anything that can't be confidently
  verified rather than stating it as fact.
- **Framing/phrasing:** suggest how to phrase and position the topic for
  the audience (clear, confident, low-hype technical register per the UX
  brief), noting common misconceptions worth a myth-callout.

Requires web access (WebSearch/WebFetch).

### Step 2 — Content-preparation package (default output)

1. **Skeleton / abstract** — a short abstract (2–4 sentences) capturing
   the article's angle and takeaway.
2. **Key focus points** — the bulleted list of the most important things
   the article must cover (and, where useful, what to deliberately leave
   out).
3. **Target persona** — which of the four author-only lenses (Architect,
   Developer, IT Manager, Business Decision-Maker) benefits most from
   this topic, with a one-line why. This sets the `persona` frontmatter
   value; it stays author-only and is never surfaced to visitors.
4. **Section breakdown** — the article's H2 structure: 3–6 headings with
   a one-line note on each, where a myth-callout fits, and a "what's
   next" pointer. Target 800–1,600 words (5–10 min read).
5. **Video beat sheet** — a ≤5-min outline (3–6 beats with rough
   timings).

Plus the routing metadata:
- Proposed **title + slug**.
- **Section assignment** (Architecture / Migration / Development, +
  sub-topic if Development), applying the one-section rule and governance
  placement test from the UX spec.
- **Suggested tags** — reuse-first from the controlled vocabulary,
  flagging any genuinely new tag, excluding section/sub-topic names.

### Step 3 — On-demand drafting (only when explicitly instructed)

- **Full video script** — expands the beat sheet into a ≤5-min spoken
  script.
- **Full article prose** — writes the article body to the section
  breakdown, at target length, in the researched framing/register.

Default runs (Steps 1–2) never produce finished prose; Step 3 is opt-in
per invocation.

### Step 4 — Execution checklist (the ordered "jobs")

Drawn from the content-workflow spec, so the output converts directly
into steps to execute:
a. create draft from template at `content/drafts/<section>/<slug>.md`,
   frontmatter filled (title, section, subtopic, format, persona, tags,
   status, source_workshop);
b. write/paste the article prose (from Step 3 if generated, else authored
   by hand to the section breakdown);
c. (optional) record the ≤5-min generic video — never raw workshop
   footage;
d. (optional) host video, set `video_url`;
e. set `status: ready-for-da`, commit;
f. create the DA page at the matching path, compose blocks/sections,
   paste body **[human/da.live login required]**;
g. add Metadata block — `section`/`subtopic`/`format` only, never
   `persona`/`tags`/`source_workshop` **[human]**;
h. Preview via Sidekick, verify, Publish **[human]**;
i. set `status: published`, commit.

**Behavior notes:**
- The skill reads the committed specs for its rules (section definitions,
  governance test, tag vocabulary, template, video constraint), so it
  stays consistent as those evolve.
- Article-required / video-optional: steps c–d are optional; publishing
  (step h) does not depend on them.
- Run once per idea; running it across the 17 catalogue items is how the
  backlog gets seeded.
- Steps f–h are inherently human (da.live/Sidekick login) — the skill
  plans them but cannot execute them.

## Scope Boundaries (out of scope for this spec)

- This spec (the document) does not itself produce any article prose or
  video. The skill, once built, can draft prose/scripts on demand
  (Step 3) — but that is a runtime action of the skill, not part of
  authoring this spec.
- Actually building the skill (implementation follow-on after review).
- Updating `content/templates/` to add the `tags` field (implementation
  follow-on).
- Any visitor-facing search/filter (deferred per UX spec).
- Automating the DA sync (decided against per content-workflow spec).

## Open Items

- Section balance: Architecture 6 / Migration 4 / Development 7 —
  reasonably balanced for launch; Development still expected to grow
  fastest (accepted per UX spec).
- `content/templates/` must gain a `tags` field to match this spec before
  the first draft is created.
