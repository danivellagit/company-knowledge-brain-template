---
# companies/<slug>.md
# One MD per company. The home company carries is_self: true; everyone else is external.
# Delete the comments and any field you don't need before saving.

name: <Company Name>
is_self: true                         # ONLY on the home company. DELETE this line for every external company.
aliases: []                           # MANDATORY, never empty. Every spelling/handle/domain seen in
                                      # transcripts, chat, mail. e.g. ["<ShortName>", "<domain>.com"]
linkedin: <https://www.linkedin.com/company/...>   # saved-once stable ID
crm_url: <CRM company record URL>     # MANDATORY for external (e.g. HubSpot, Salesforce, Pipedrive).
                                      # Leave empty for the home company and for competitors.
industry:                             # MANDATORY for external (optional for competitor)
  - "[[<industry>]]"                  # wiki link to industries/<slug>.md
relationship:                         # MANDATORY for external
  - "[[<relationship-type>]]"         # customer | prospect | partner | vendor | advisor | competitor
uses_stack: []                        # optional, external software they run. e.g. ["[[<system>]]"]
docs_url: <docs-url>                  # optional, the company's hub/page in your docs system (e.g. Notion, Confluence)
people: []                            # wiki-link list of people MDs at this company. e.g. ["[[jane-doe]]"]
---

<!--
BODY ASYMMETRY:
- HOME company (is_self: true) = richer body allowed (positioning + voice anchor).
- EXTERNAL company             = light body, a 1-2 sentence About and nothing else.
- COMPETITOR                   = lightest body; no crm_url; industry optional.
Keep ONLY the shape that matches this company.
-->

<!-- ============ SHAPE A: HOME company (is_self: true) ============ -->

# <COMPANY>

## About

What the company is, in one or two sentences. Stable facts.

## Positioning

Who you serve, what you sell, the category you play in. This is the canonical
statement every agent leans on when it represents the company.

## Voice

The tone every agent uses when it speaks for the company.

<!-- ============ SHAPE B: EXTERNAL company (light body) ============ -->

# <Company Name>

## About

1-2 sentences: sector, business model, geography. Stable facts only.

<!--
For EXTERNAL: stop here. Do NOT add Why we care, Notes, Context, deal info,
current status, risks, last-touch dates, or any prose about the relationship.
That lives in the CRM (via crm_url) and goes stale here in weeks.
-->

<!-- ============ SHAPE C: COMPETITOR (lightest body, no crm_url) ============ -->

# <Competitor Name>

## About

One line: what they offer and how it overlaps with what you sell.
