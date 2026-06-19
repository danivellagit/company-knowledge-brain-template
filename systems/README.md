# systems/

One MD per **external software platform / tool** that companies run inside their own stack (commerce platforms, PIMs, payment providers, CRMs, marketplaces, …). Stable wiki-link targets so companies can declare `uses_stack: [[<system>]]` and sales / account-config agents can query "which customers run `<platform>`?" in one hop.

Not a catalogue of every tool you've heard of — only systems that **appear in real customer stacks** or that `<COMPANY>` integrates with. If you find yourself adding a system because it "might come up" — stop.

---

## When to create an MD

Whenever a recap, signal, or company brief surfaces a software platform a company runs (or a partner ecosystem `<COMPANY>` plugs into) **and** an MD does not exist yet. The first mention triggers the creation; the wiki-link in the citing file then lands on a real target. A platform that recurs (≥2 mentions or ≥2 sources) is a create candidate even if you skipped it the first time.

Do NOT create:
- Internal `<COMPANY>` products / agents → those live in [`products/`](../products/).
- Random SaaS you've heard of but no customer uses → wait for the first real signal.
- Versions / minor variants — one MD per product family.

---

## File convention

`<slug>.md`, lowercase, hyphenated. Examples: `<platform-a>.md`, `<platform-b>.md`, `<crm-tool>.md`.

### Front matter

```yaml
---
name: <System name>
slug: <system-slug>
category: <category>             # ecommerce-platform | PIM | payments | CRM | marketplace | ESP | DAM | analytics | ...
vendor: <Vendor name>            # the company behind it
website: https://<system-domain>
aliases: []                      # every other way this platform is named — keep rich, empty is a bug
companies: []                    # manual reverse of companies.uses_stack — companies that run this system
                                 #   - "[[<company-slug>]]"
---
```

`aliases` should be **rich**: product nicknames, abbreviations, sub-product names that collapse to this family, so the entity-scan matches an incoming mention onto this MD rather than creating a duplicate. Empty `aliases` on a real entity is a bug.

### Body

```markdown
# <System name>

## About

1-2 sentences: what the external software platform is, and where it's strong.
```

That's it. **No integration notes section** — integration realities differ per customer and per release; anything written here goes stale fast and nobody refreshes it. Live integration documentation lives in the dev docs / your docs system / customer-specific account brief, not here.

---

## Inverse view — who uses it

The `companies` field is a **manually-maintained reverse relation** of `companies/<X>.md → uses_stack: [[<slug>]]`. A local Markdown editor renders it as a dedicated "Companies" section on the system page. This is the reverse-edge double-write: when a company adds this system to its stack, update both files in the same change. Signals mentioning this system in `related_to_systems` still surface in the editor's generic Referenced by view.
