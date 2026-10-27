# Rev Rec AI Skills

Two sanitized AI-agent runbooks for spreadsheet-based revenue-recognition workflows: a monthly workbook roll-forward and an incremental daily shipment append.

This public demo is adapted from project skill files supplied by Daksh Chauhan. It keeps the workbook-processing techniques while removing private business configuration, actual rates, customer/account exceptions and production incident details.

## Included

| Skill | Purpose | Output model |
|---|---|---|
| [Monthly snapshot](skills/revrec-monthly-snapshot/SKILL.md) | Archive a completed working copy and prepare a new monthly tab | Full multi-sheet workbook copy |
| [Daily append](skills/revrec-daily-append/SKILL.md) | Process dated shipment deltas with header-based mapping and validation | Single-sheet working copy |

These are Markdown skill definitions, not executable scripts. No agent application, runner, model configuration, workbook, export, live connection or customer data is included. An agent capable of local file operations and spreadsheet/package inspection must interpret the runbooks. Loading and invocation depend on that agent; there is no universal install command.

## Techniques demonstrated

- Read `.xlsx` packages as ZIP/XML to avoid rebuilding a large workbook through a full object model.
- Resolve fields from actual header text rather than remembered column letters.
- Keep source files separate from working output; validate before writing.
- Process incremental exports in chronological order and preserve manual entries.
- Check formula references, cached values, package relationships and formatting.
- Stop for missing accounting rules instead of inventing financial logic.

## Demo configuration

All product names and rates in the example below are fictional, not real pricing or accounting guidance:

| Fictional product | Illustrative rate (USD) |
|---|---:|
| DEMO-DEVICE-A | 100 |
| DEMO-DEVICE-B | 200 |

The runbooks do not use these values as approved accounting policy. Supply an independently approved mapping and policy before using any real data. Original organization-specific calculations, price tables, financing categories and reconciliation layouts are intentionally omitted, not hidden behind renamed labels.

## Known limitations

1. **The two output contracts are not compatible as a ready-made pipeline.** The monthly runbook preserves a multi-sheet master; the daily runbook produces a single-sheet output under a different filename. This public cleanup does not redesign that handoff. Do not chain them unattended until an owner chooses and tests one contract.
2. **Cross-sheet formulas can break in the single-sheet output.** Preserved formula text and cached values do not guarantee correct recalculation after referenced sheets are removed. Inspect this before using the output.
3. **Product qualification needs an explicit policy.** The supplied daily design restricts qualifying hardware rows while also describing accessory-derived fields. This demo does not silently settle that rule conflict; stop if those cases affect a run.
4. Mapping workbooks and approved financial rules are not included. Omitted rules must be supplied and reviewed before real processing.
5. No end-to-end execution or workbook recalculation tests have been performed for this public demo. Structural validation alone does not establish financial correctness.

## Safe evaluation

Use synthetic workbooks in a disposable folder. Keep independent backups. Review every proposed move and overwrite, compare totals with an independent calculation, and inspect the rendered workbook. Do not use this demo to post entries, close books, or make accounting decisions without qualified review.

## Author

Daksh Chauhan. No open-source license grant is included in this repository.
