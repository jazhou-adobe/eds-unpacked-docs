# UX Design Brief — Edge Delivery Services Unpacked

A briefing document for the visual/UX design phase (Google Stitch, a
designer, or another AI design pass). Everything under "Decided" is
already committed and shouldn't be re-litigated; everything under "Open"
is exactly what this design phase needs to resolve.

Source specs (read these for full rationale, this doc only summarizes):
- `docs/superpowers/specs/2026-07-23-foundation-and-personas-design.md`
- `docs/superpowers/specs/2026-07-23-ux-site-structure-design.md`
- `docs/superpowers/specs/2026-07-23-content-workflow-design.md`
- `docs/superpowers/plans/2026-07-23-website-development.md`

---

## 1. What this site is

**Edge Delivery Services Unpacked** teaches Adobe Edge Delivery Services
(EDS) — architecture, tooling, authoring model, and adoption path —
drawing on the author's customer workshop experience in Australia.

It serves three audiences at once, through one public, ungated tier:
- **Public portfolio** — showcases the author's EDS expertise
- **Customer enablement** — a resource pointed to during/after engagements
- **Internal Adobe field enablement** — colleagues reuse the same content

**Core promise:** after engaging with the content, a visitor can have a
confident conversation about EDS with peers, push back on common myths,
and potentially become an EDS champion at their company.

Primary audience: people currently on AEM 6.5, AEM Managed Services, or
another CMS entirely, evaluating or adopting EDS.

## 2. Who the design needs to work for

Four reader concerns the content is written for (these are an *authoring*
lens, not visitor-facing filters or nav — nothing in the UI should
segment by persona), but the design should read well for all four
simultaneously on the same pages:

1. **Architect** — technical architecture, integration points, fit
   assessment.
2. **Developer** — authoring model, tooling, local dev workflow.
3. **IT Manager / Operations** — governance, hosting/ops model, security,
   support, maintenance overhead.
4. **Business Decision-Maker** — cost, risk, time-to-market, competitive
   position.

Practical implication for design: the same page needs to feel credible
to a skeptical enterprise IT manager and approachable to a hands-on
developer — lean toward clear, confident, low-hype technical-content
design, not marketing-site gloss.

## 3. Site structure (decided — see UX/Site Structure spec)

**3 top-level sections**, replacing an earlier 7-pillar draft:
1. **Architecture** — how EDS works, technical concepts, architectural
   governance (security/compliance).
2. **Migration** — moving from AEM 6.5 / AEM Managed Services / another
   CMS to EDS, plus migration-specific governance (support model,
   cutover planning).
3. **Development** — authoring experience, performance/Core Web Vitals,
   developer tooling. Carries 3 sub-topic labels: **Authoring**,
   **Performance**, **Tooling** (plain text labels, not filters).

Every content item belongs to exactly one section — no cross-listing.

**Page hierarchy and templates** (4 page types total):
- **Home** — 3 section tiles (Architecture / Migration / Development) +
  a "recently added" list (top 5, global recency, intentionally
  unbalanced across sections — Development will publish faster). No
  content list beyond that; no bio content here.
- **Section hub** (×3) — flat, reverse-chronological list of everything
  in that section. Each row: title, format indicator (article/video),
  and — Development only — a sub-topic label. No search/filter for v1.
- **Content item** — one shared template for both articles and videos
  (video is just the main content block, not a separate player-first
  layout). Myth-busting callouts can appear inline anywhere in the body.
- **About** — one-off static page, author bio/credibility content
  (workshop experience, Adobe background). Not a template, just a page.

**Global nav**: logo (→ home) + Architecture + Migration + Development +
About. Nothing else — no search, no additional utility nav for v1.

## 4. Content patterns to design for

- **Myth-busting callout** — a recurring inline component, used inside
  any content item wherever a common misconception gets addressed. No
  dedicated page, no aggregation elsewhere — needs to read as a distinct,
  noticeable-but-not-garish visual treatment. (Note: an earlier version
  used a thick colored left border for this and an automated design
  review flagged it as a recognizable "AI-generated UI" tell — avoid that
  specific pattern; the current placeholder implementation uses a plain
  tinted background block instead.)
- **Section hub list row** — title + format indicator + optional
  sub-topic label, needs to work as a fairly dense repeatable list item
  (no cards/thumbnails required, no images guaranteed to exist).
- **Home section tiles** — 3 large entry points, need to read as equally
  weighted top-level choices even though Development will contain more
  content behind it.
- **Recently added list** — lighter-weight than the section tiles,
  secondary visual priority on the homepage.

## 5. Technical constraints the design must fit inside

- Built on **Adobe Edge Delivery Services** (`aem-boilerplate`), plain
  block-based architecture — no framework, no client-side routing, no
  build step. Every distinct visual pattern becomes a `blocks/<name>/`
  JS+CSS pair.
- Existing blocks already in the code repo: `header`, `footer`, `cards`,
  `columns`, `hero`, `fragment`, `widget`, plus new ones already built:
  `myth-callout`, `section-hub`, `home-recent`.
- Existing CSS custom properties (`styles/styles.css`) — reuse rather
  than replace unless the design phase deliberately decides to rebrand:
  ```
  --background-color: white
  --light-color: #f8f8f8
  --dark-color: #505050
  --text-color: #131313
  --link-color: #3b63fb
  --link-hover-color: #1d3ecf
  --body-font-family: roboto, roboto-fallback, sans-serif
  --heading-font-family: roboto-condensed, roboto-condensed-fallback, sans-serif
  ```
- Content is authored in Adobe Document Authoring (da.live) as real DA
  pages — composed of blocks and sections in DA's native page format,
  not a custom CMS. Whatever the design produces has to be buildable as
  DA-authorable blocks, not arbitrary custom markup only a developer
  could produce.
- Public-only, no login/gating anywhere in the design.

## 6. Open — this design phase needs to decide these

None of the following are fixed yet. Treat this as the actual scope of
"the UX design job":

- **Visual/brand identity**: color palette (keep the boilerplate default
  above, or redesign?), typography (keep Roboto/Roboto Condensed, or
  change?), logo/wordmark for "Edge Delivery Services Unpacked."
- **Tone/mood**: how technical-documentation-like vs. how
  editorial/magazine-like should this feel? (Given the 3-audience mix in
  §2, probably closer to clear technical writing than marketing gloss,
  but the exact register is undecided.)
- **Imagery**: does the site use any photography/illustration, or stay
  text/diagram-first? No image assets exist yet beyond the boilerplate's
  default favicon.
- **Format indicator design**: the current implementation uses plain
  text labels ("Article"/"Video") instead of icons — deciding whether
  real icon treatment is worth it is in scope here.
- **Responsive/device priorities**: any specific device assumptions
  (e.g., workshop attendees on mobile right after a session vs. desktop
  research later)?
- **Accessibility bar**: no specific target (e.g. WCAG level) has been
  set yet.

## 7. Success criteria for the design itself

Ties back to the Foundation spec's success criteria: a visitor should be
able to quickly tell (a) what this site is, (b) which of the 3 sections
is relevant to them, and (c) come away confident enough to explain EDS
to a colleague. The design should optimize for clarity and credibility
over visual flourish, given the enterprise/technical audience mix.
