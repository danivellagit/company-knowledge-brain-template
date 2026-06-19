---
# products/<slug>.md
# A thin pointer to the AI agent / automation <COMPANY> builds.
# The full spec (fit, pricing, customers, KPIs, status) lives in your docs system,
# which is the system of record. This MD is just a stable, greppable index entry.
# No relational wiki-link edges here, those relations live in the docs system.

name: <Product Name>                 # e.g. "Lead Qualifier"
slug: <product-slug>                 # matches the filename, e.g. lead-qualifier
category: <category>                  # your own taxonomy, e.g. sales | support | marketing | ops | data
docs_url: <docs-url>                  # pointer to the page in your docs system (the system of record)
---

# <Product Name>

## About

1-2 sentences: what the agent / automation does and the job it solves.

<!--
Stop here. Do NOT add pricing, customer lists, KPI targets, fit relations, or
rollout status. All of that lives in the docs page linked from `docs_url` and
goes stale in the repo within weeks.
-->
