# Edge Delivery Services Unpacked — Foundation & Personas Design

## Context

**Site name: Edge Delivery Services Unpacked.**

This is the first of four planned sub-projects for the site:

1. **Foundation & Personas** (this document)
2. Content Architecture (article/video organization and tagging)
3. UX / Site Structure (navigation, page templates)
4. Skills/Playbooks (distilled shareable takeaways)

Each sub-project gets its own design → spec → plan cycle. This document covers only #1.

**Topic correction:** the site is about **Edge Delivery Services (EDS)**, not Azure. Earlier framing of this idea used "Azure services" as a placeholder/error — every mention of Azure in prior discussion should be read as EDS.

## Purpose & Positioning

Edge Delivery Services Unpacked teaches Edge Delivery Services (EDS) — architecture, tooling, authoring model, and adoption path — drawing on the author's customer workshop experience in Australia.

It serves three audiences simultaneously, with a single public content tier (see Scope Boundaries):

- **Public portfolio / thought leadership** — showcases the author's EDS expertise.
- **Customer enablement** — a resource to point specific customers to during/after engagements.
- **Internal Adobe field enablement** — colleagues reuse the same public content in their own customer conversations; there is no separate internal-only tier.

Primary audience framing: visitors are currently on AEM 6.5, AEM Managed Services, or another CMS entirely, and are evaluating or adopting EDS.

**Core promise:** after engaging with the site's content, a visitor should be able to have a confident conversation about EDS with peers, push back on common myths/misconceptions, and potentially become an EDS champion inside their own company.

## Personas

Personas are an **internal authoring lens for the site owner** — a tool for deciding what to create and how to frame it. They are not a visitor-facing construct: visitors are anonymous, are never asked to self-identify, and can browse and consume any content freely with no persona-based gating, filtering, or navigation.

Four personas, each a first-class lens the author writes/records from:

1. **Architect** — evaluates EDS's technical architecture, integration points, and how it compares to their current AEM/CMS setup. Content written from this lens should let that reader assess fit and sketch an adoption/migration architecture.
2. **Developer** — wants the authoring model (document-based authoring), tooling, local dev workflow. Content written from this lens should leave that reader comfortable building or contributing to an EDS project.
3. **IT Manager / Operations** — cares about governance, hosting/ops model, security, support model, maintenance overhead. Content written from this lens should leave that reader confident EDS fits their operational and compliance requirements.
4. **Business Decision-Maker** — cares about outcomes: content velocity, cost, risk, time-to-market, competitive position. Content written from this lens should let that reader justify or approve an investment in EDS adoption.

"Equal first-class lenses" means each persona is designed for from day one when the author is deciding what to write next — **not** that launch requires equal content volume per persona. Workshop source material skews technical (Architect/Developer); IT Manager and Business content will likely lag initially. This imbalance is an accepted, explicit trade-off, not a launch blocker.

## Content Organization Structure

- **Primary nav = EDS topic/capability.** The exact set of topic pillars (e.g., architecture, authoring, performance, tooling, migration, governance, myths) is intentionally left open, to be resolved in the Content Architecture sub-project.
- **Persona = internal authoring metadata, not a visitor-facing feature.** Content may carry a persona tag in its frontmatter purely so the author can track coverage across the four lenses over time (e.g., "have I written enough for IT Managers?"). This tag is never surfaced to visitors as a filter, landing page, or curated journey — there is no persona-based UI in this phase.
- **Format types:** articles (written intros/deep-dives) and videos, both organized under the same topic taxonomy and persona-tagged the same way.
- **Video content constraint:** raw customer workshop recordings are **never published directly** — they show customer names, environments, and internal discussion. Workshop recordings serve only as private source material/inspiration. All published video content is re-recorded in a generic form with no customer specifics.
- **Skills/playbooks** (distilled cheat-sheets/checklists) are explicitly out of scope for this spec — their own sub-project, designed once the content taxonomy exists to hang them off of.

## Success Criteria

Success is qualitative for this phase: a visitor can articulate what EDS is and why it matters to their organization, can correct at least one common myth about it, and knows where to go next based on the topic structure.

No analytics or feedback mechanism (e.g., "was this helpful," contact link) is in scope for this spec — deliberately deferred until there's real content and traffic to measure.

## Scope Boundaries (explicitly out of scope for this spec)

- The final list of topic pillars — Content Architecture sub-project.
- Any visitor-facing persona UI (filters, curated paths, landing pages) — personas remain an author-only planning tool unless revisited.
- UX/visual site structure and navigation design — UX Structure sub-project.
- Skills/playbooks format — Skills sub-project.
- Hosting/platform decision — author has existing ideas; not addressed here.
- Any gated/internal-only content tier — everything published is public by default.
- Analytics/feedback measurement — deferred entirely.

## Open Items

- Final topic pillar list (deferred by choice, to be resolved when scoping Content Architecture).
