# Profile

> The single source of truth for company-specific information.
>
> Every agent reads this file whenever it needs to know "who is who" or "what business we're in".
>
> **Template note:** fill the `<placeholders>` and `_TBD_` blocks. The example team set below (leadership / sales / marketing / product / engineering / customer-success) is a starting point for a scale-up — rename, add, or remove teams to match your org, then mirror the changes in [`canon/team/`](team/) and [`team/`](../team/).

---

## Company

- **Name:** `<COMPANY>`.
- **One-line pitch:** _TBD — what you do, in one sentence a customer would recognise._
- **Vertical / market:** _TBD._
- **Stage:** _TBD (e.g. Seed, Series A, profitable bootstrapped)._
- **Headcount:** _TBD._
- **Primary markets:** _TBD — geographies, in priority order._

Working language for written artifacts is English, see [`operations.md`](operations.md) → "Language". Voice and positioning are defined in [`company-rules.md`](company-rules.md) → "Voice" and "Positioning".

## Leadership

They own the cross-company files in this repo.

- **`<Full Name>`**, `<role>` — _short context (prior experience that matters)._
- **`<Full Name>`**, `<role>` — _short context._

Backed by: _TBD (investors / board), or "bootstrapped"._

## Team leads

The people who own their team's `team/<team>/` files. This table is the example team set for a scale-up — adjust to your org.

| Team | Lead | Primary concern |
|---|---|---|
| Leadership | `<Name>` | Strategy, governance, hiring, cross-team decisions |
| Sales | `<Name>` | Pipeline, deals, commercial conversations |
| Marketing | `<Name>` | Brand, content, demand generation |
| Product | `<Name>` | Roadmap, discovery, specs |
| Engineering | `<Name>` | Architecture, infrastructure, delivery, incidents |
| Customer Success | `<Name>` | Onboarding, adoption, retention, customer delivery |

> This table is a high-level index. For **who does what (and what they do NOT do)**, speaker aliases in your meeting-transcript tool / chat / email, and decision authority, see [`people/`](../people/). Every agent that writes ownership into a deliverable must read that folder first.

## Integrations in use

Sources wired for generation. Fill in the tools you actually use; the bracketed examples are common choices.

- **`<transcript tool>`** (e.g. Fireflies, Otter, Granola): call transcripts.
- **`<CRM>`** (e.g. HubSpot, Salesforce, Pipedrive): deals, pipeline, contact records, company records. **This is the canonical CRM** for this repo: every external `people/<slug>.md` and `companies/<slug>.md` must carry the CRM record URL in `crm_url`. The ID embedded in that URL is what lets agents jump straight to the record without a search step.
- **`<email>`** (e.g. Gmail, Outlook): email threads and drafts.
- **`<code host>`** (e.g. GitHub, GitLab): PRs, issues, commits.
- **`<docs system>`** (e.g. Notion, Confluence, Coda): pages, deck narrative, audience-facing docs.
- **`<file storage>`** (e.g. Google Drive, OneDrive): binary files (xlsx, pdf, docx, pptx, images).

## Example customers and partners

These names appear in examples and fixtures throughout the system. _TBD — leave blank until you have real, sign-off-cleared names._

- **Customers:** _TBD._
- **Partner ecosystem:** _TBD._

---

## Mission

_TBD — one paragraph. What changes in the world if you succeed?_

## Annual themes

The big-picture anchors the whole company advances this year. Every team's roadmap, every campaign, every deal should trace to one of these. _TBD — pick 2–4._

- **`<theme 1>`:** _one line._
- **`<theme 2>`:** _one line._

## Goals for the year

Concrete things you want to be able to say "we did" by year end. Internal ARR figures stay out of this file, they live in leadership-only docs. _TBD._

- **`<goal 1>`:** _one line._
- **`<goal 2>`:** _one line._

## Product pillars

The pillars that organize what you build. Every roadmap item should trace to one. _TBD._

- **`<pillar 1>`:** _one line._
- **`<pillar 2>`:** _one line._

## Go-to-market posture

- **Primary markets and segments:** _TBD. Full detail in [`icp.md`](icp.md)._
- **How we reach customers:** _TBD (direct sales, inbound, partner / referral, PLG)._
- **Who drives sales:** _TBD (the GTM team shape + founder involvement on which deals)._

## Operating principles

How the company operates day-to-day. Stable across years; goals shift, principles don't. _TBD — adapt the examples._

- AI-native before people-native.
- Single owner per deliverable.
- Customer transparency > internal comfort.
- Quality > speed when they conflict.

---

*This file is CEO-editable. Update it whenever the org changes (new hires in leadership, new team lead, new vertical, new market, new annual theme). The next run picks up the changes automatically.*
