---
name: spec-clearance-with-odoo-product
description: Review a client specification against standard Odoo 18 features before development starts. Use when asked to "clear a spec", "check spec against Odoo", "fit-gap analysis", "is this standard in Odoo", or to review a functional/technical specification document for an Odoo project.
---

# Spec Clearance with Odoo Product

Goal: go through every requirement in a specification and decide how Odoo covers it,
so that no custom development is planned for something Odoo already does.

## Inputs

- The specification document (file path, pasted text, or doc link).
- Odoo version (default: 18.0) and edition (Community / Enterprise).
- Installed or planned apps, if known.

## Process

1. **Split the spec into atomic requirements.** One requirement = one testable behavior.
   Number them (R1, R2, ...) and keep the client's original wording.
2. **Map each requirement to Odoo apps and models.** Name the app (Sales, Inventory,
   Accounting, ...) and the main model (`sale.order`, `stock.picking`, ...).
3. **Verify against the source code, not memory.** Search the local source
   (`community/addons`, `enterprise`) for the field, setting, or flow before claiming it exists.
   Check `res.config.settings` for feature toggles.
4. **Classify each requirement** with exactly one status:
   - `STANDARD` — works out of the box.
   - `CONFIG` — works after settings, data, or access-rights configuration.
   - `STUDIO` — needs Odoo Studio (fields, views, automations, reports). Enterprise only.
   - `CUSTOM` — needs a custom module.
   - `NOT ADVISED` — fights the Odoo design; propose a standard alternative.
   - `UNCLEAR` — spec is ambiguous; write the question for the client.
5. **For CUSTOM items**, describe the smallest change: models to inherit, fields,
   methods to override, views to extend. Flag upgrade risk (core method overrides,
   accounting/stock valuation changes, OWL patches).
6. **Cross-check impact**: multi-company, multi-currency, access rights, localization,
   accounting entries, stock valuation, and reports affected by each requirement.

## Output

Produce a fit-gap table, then a summary.

| # | Requirement | App / Model | Status | How Odoo covers it / Proposed change | Effort | Notes / Questions |
|---|-------------|-------------|--------|--------------------------------------|--------|-------------------|

Effort: `S` (< 1 day), `M` (1–3 days), `L` (> 3 days).

Summary must include:

- Count per status.
- List of `UNCLEAR` questions to send to the client.
- List of `NOT ADVISED` items with the standard alternative.
- Main risks (upgrade, performance, data migration).

## Rules

- Prefer standard > configuration > Studio > custom. Always state why a lower option is not enough.
- Never mark a requirement `STANDARD` without pointing to the feature, setting, or code location.
- Note edition limits: say when a feature is Enterprise-only.
- Do not invent features. If unsure, mark `UNCLEAR` and say what to test.
- Keep the client's wording in the Requirement column; put interpretation in Notes.
