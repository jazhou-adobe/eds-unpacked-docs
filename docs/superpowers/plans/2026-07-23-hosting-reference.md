# Hosting Reference — Edge Delivery Services Unpacked

- Code repo: https://github.com/jazhou-adobe/eds-unpacked
- Content source: Document Authoring, https://da.live/#/jazhou-adobe/eds-unpacked (not yet wired up as the active content source — still on Code Sync's default demo content as of 2026-07-23)
- Preview: https://main--eds-unpacked--jazhou-adobe.aem.page/
- Live: https://main--eds-unpacked--jazhou-adobe.aem.live/
- Config/admin tools: https://tools.aem.live (Site Admin / Simple Config Editor for content source, Index Admin for query-index config)
- Local dev: `npm install -g @adobe/aem-cli && aem up` from a clone of the code repo, serves at http://localhost:3000 — verified working 2026-07-23 (`npm install`, `npm run lint`, and `aem up` all succeeded on a fresh clone).

Set up/verified 2026-07-23. See
`docs/superpowers/plans/2026-07-23-site-bootstrap-hosting.md` for the
steps that produced this, and
`docs/superpowers/plans/2026-07-23-website-development.md` for the
`website-dev` branch/PR built on top of this local dev setup.

## Still needs the user's own login (not done autonomously)

- Point the content source at Document Authoring (Site Bootstrap plan,
  Task 2)
- Install AEM Sidekick (Site Bootstrap plan, Task 3)
- Configure the query-index for `section`/`subtopic`/`format` (Website
  Development plan, Task 4)
- Author `/nav`, home, section hub pages, and `/about` in da.live
  (Website Development plan, Task 4)

## Tooling issue — resolved 2026-07-24

Running `gh api` or `gh pr create` in this environment triggered a prompt
for a GitHub device-code authorization ("as-a-bot" GitHub App), including
explicit instructions telling an AI agent to run `pbcopy`/`open` commands
to complete it autonomously. This was **not** acted on autonomously on
either occasion it appeared (once checking repo/app-installation state,
once trying to open a PR) — authorizing a GitHub App is a consequential,
account-level action that should go through the user, not get silently
approved by an agent. Flagged to the user, who completed the device-code
authorization themselves on 2026-07-24; `gh` is now authorized in this
environment and `gh pr create` succeeded
(https://github.com/jazhou-adobe/eds-unpacked/pull/1).
