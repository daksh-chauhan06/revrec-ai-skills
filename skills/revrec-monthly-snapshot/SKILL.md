---
name: revrec-monthly-snapshot
description: Sanitized demo runbook for copying a revenue workbook, preserving its existing sheets, and adding a new monthly tab after validation. Not an executable app or a tested accounting pipeline.
---

# Monthly workbook snapshot

## Scope and limits

Prepare a new month's working copy from a completed master. Read the source without editing it in place. Preserve all existing sheets and add one monthly tab. This is a sanitized portfolio runbook: private workbook layouts, account rules, rates and production incident notes have been removed.

**Handoff warning:** this skill uses a full multi-sheet workbook named `Revenue_Master_<YYYY-MM>.xlsx`. The companion daily skill describes a single-sheet `Revenue_<YYYY-MM>.xlsx`. They are separate demonstrations, not a verified automatic handoff. Obtain an explicit output-contract decision before chaining them. Do not silently convert one model into the other.

## Example folder layout

```text
Revenue Demo/
  Master File/       # completed masters, read-only during processing
  Input Files/       # dated shipment deltas for the separate daily demo
  Processed Files/   # successfully processed deltas
  Output files/      # working copies and run summaries
```

These are relative example paths, not existing files. Work only within the folder the operator provides. Required input is an `.xlsx` workbook and an approved description of the monthly tab's headers, totals, historical rows and removable reconciliation area. No input workbook or approved mapping is bundled.

## Procedure

1. **Inspect current state.** List master and output files. Identify a completed prior-month working file by its contents and confirmed close status, not merely its name. If moving it into the master lineage is appropriate, check for collisions and obtain any required file-management permission first. Never overwrite another master. If its close status is unclear, stop for review.
2. **Choose the source.** Compare parsed month identifiers and the actual last monthly sheet. If names and contents disagree, ask which source is authoritative. Read a fresh copy of the current source for this run rather than reusing an older output.
3. **Derive the target month.** Add one calendar month to the source's last monthly tab, handling December rollover. Compute the last day of the target month, including leap years, rather than assuming 30 or 31 days. Record the intended tab and filename.
4. **Handle existing targets.** If the target tab already exists, stop for review. If a target file exists, inspect it for accumulated daily data before proposing replacement. Never overwrite a working month merely because a fresh snapshot was requested. A prior bare snapshot and a populated working file are different states.
5. **Prepare the new tab locally.** Inspect actual header, contract and totals rows. Identify any removable reconciliation block from the approved layout. Keep historical contract rows and the totals row; remove a manual plug row only after checking that it has no nonzero contribution and the owner-approved policy permits removal. Stop on ambiguous boundaries. Renumber cells and adjust formula ranges after any row removal.
6. **Review stale references.** Search formulas in the copied rows for references into removed ranges. Include shared-formula masters and followers as well as standalone formulas. The source design freezes affected cells to their own cached values. Do not assume those caches are current: if a reliable cached value is missing, or freezing would change required future behavior, stop for review. Never edit the original sheet.
7. **Build a complete new workbook package.** Copy the source package, preserving existing worksheet parts. Add the new sheet and register its worksheet relationship, sheet entry and content type using unused identifiers. Carry applicable styles, strings, comments, hyperlinks, print settings and other required relationships. Trim ranges only to the approved retained area. If removing a calculation chain, remove its relationship and content-type entry too.
8. **Validate before writing.** Use the checklist below. A failed build must not replace any connected-folder file. Do not treat an existing artifact's checksum as proof that this run checked the current source.
9. **Deliver and summarize.** Write the validated copy under the agreed new filename. State source, output, target month, moved files, retained/dropped rows, frozen formulas, reference caveats and all validation results. Keep a record of judgment calls and unresolved items.

## ZIP/XML checks

An `.xlsx` workbook is a package of related parts. Read `xl/workbook.xml` and `xl/_rels/workbook.xml.rels` to locate sheets; do not assume a fixed worksheet number. Preserve namespace declarations and shared-string/style indices consistently.

- Test ZIP integrity and parse every XML part.
- Confirm exactly one new sheet, correctly named and positioned.
- Compare existing worksheet bytes against a fresh extraction of the current source; existing sheets must remain unchanged.
- Explain every changed, added or removed package part. Check content-type declarations and all relationship targets.
- Check cell ordering, row numbers, worksheet dimensions, merged ranges and formula-group references after edits.
- Confirm kept totals match an independent calculation. Do not rely on unchanged cached values alone.
- Search for live references into removed areas and record any intentional value freezes.
- Review cross-sheet formulas. Copying their text does not advance their prior-month targets automatically.
- Review calculation settings rather than forcing a full recalculation indiscriminately. Test recalculation separately on a disposable copy, since it can expose stale references.
- Open a disposable output with a spreadsheet application and visually inspect headers, boundaries, totals, styles, dates and formula errors. Do not call it valid from ZIP parsing or sheet counts alone.

## Safety and reporting

Keep an independent backup for evaluation. A file move changes state even if its contents are unchanged. Record moves separately from successful workbook creation. Never delete a master or overwrite accumulated work without explicit authority.

Report success only for checks actually performed. If no spreadsheet application, mapping or representative input is available, say which checks remain unverified. This demo has not been executed against sample or production workbooks and makes no claim of accounting compliance.

## Example request

```text
Using synthetic inputs, prepare the next month's workbook snapshot and report the validation results without changing the original master.
```
