---
name: revrec-daily-append
description: Sanitized demo runbook for appending dated shipment exports to a monthly single-sheet workbook using an approved header mapping, chronological processing and validation. Not an executable app or a tested accounting pipeline.
---

# Daily shipment append

## Scope and limits

Read a historical `.xlsx` sheet and incremental shipment exports. Build on a single existing output when available, append qualifying records, preserve manual entries and deliver a validated working copy. Original organization-specific products, rates, financing categories, account exclusions, financial formulas and incident details are omitted from this public demo.

**Not a ready-made monthly pipeline:** this skill's output is a single-sheet workbook named `Revenue_<YYYY-MM>.xlsx`. The companion monthly skill retains a full multi-sheet master with another filename. This cleanup does not change that mismatch. Agree and test a shared contract before running them together.

**Cross-sheet warning:** removing other tabs can leave formulas pointing at absent sheets. Cached values are not a substitute for working formulas. Inspect affected references and obtain an explicit decision about retaining referenced tabs or converting approved cells to values. Do not claim a single-sheet result will recalculate correctly without testing it.

## Example inputs

```text
Revenue Demo/
  Input Files/       # shipment_delta_<YYYY-MM-DD>.xlsx
  Processed Files/   # processed exports, moved only after validation
  Output files/      # Revenue_<YYYY-MM>.xlsx and run summaries
```

Required but not included: historical workbook, shipment exports, approved mapping, qualification policy, financial calculation rules and reconciliation definitions. Header names below are examples, not a production schema:

| Target field | Example source | Treatment |
|---|---|---|
| Account | Account Name | Copy after privacy review |
| Order | Order Number | Copy; multiple lines can share one order |
| Shipment date | Date of Shipment | Parse explicitly and write a date value |
| Product | Product Series | Map with the approved classification table |
| Quantity | Quantity | Validate numeric type and sign |
| Contract months | Contract Term | Use the approved field, not a similarly named substitute |
| Historical | Processing state | Derive from the month policy below |

Fictional product labels such as `DEMO-DEVICE-A` and `DEMO-DEVICE-B` may be used for synthetic tests. Never substitute fictional rates into real financial work. No real accounting formula or organization-specific rate table is provided here.

## Procedure

1. **Find the base and inspect its contents.** If a working output exists, read its actual covered dates and currently open month. Do not restart from a static historical sheet and lose accumulated daily rows. If multiple outputs or uncertain states exist, stop for review rather than guessing which one wins.
2. **List the work queue.** Consider only actual dated exports in `Input Files/`. Skip the historical workbook, lock files, backups and OS metadata. Resolve the date from an unambiguous filename or its shipment-date field. If duplicate exports cover the same date, compare modification metadata and contents; flag uncertainty before choosing one.
3. **Process chronologically.** Inspect all unprocessed dates, including missed days, and handle one date at a time. Reconcile the queue against covered dates before appending to avoid duplicate processing. Keep a summary of files considered, skipped and appended.
4. **Read real headers.** Find the export's header row by its actual field names. Keep true column indices, including blank spacer columns. Skip footer totals and non-record rows. Resolve the output's target columns by header text, not remembered letters or a style map.
5. **Apply the approved qualification policy.** Check order type, record type, product class, region and any approved exclusions independently. Log exclusion reasons. This public demo contains no private account list or region-specific business rule. Stop if the policy is missing or if qualifying device-only rows conflicts with a requested accessory-derived field. Do not silently widen the filter or invent a rule.
6. **Build each mapped row.** Copy approved direct fields. Normalize same-row references only after verifying formula intent against the current workbook and approved mapping. Leave unresolved/manual fields unpopulated and report them. Do not infer financial formulas from nearby scratch calculations or broken references.
7. **Track monthly state.** Group qualifying records by shipment month in chronological order. For the source design, historical/closed months are TRUE and only the latest open month is FALSE. Never reopen a closed month silently. Append to the existing open-month block, or close it and begin the next month when the approved policy permits. Preserve earlier state across daily runs rather than marking every prior-run row historical.
8. **Update summaries and reconciliation.** Extend month-scoped formulas rather than summing the entire history. For incremental exports, add each day's approved contribution to the existing running total. Preserve manually entered values in manual-only reconciliation rows; never fabricate figures to force a zero variance. Original category labels, financing tables and customer exceptions are intentionally absent. An approved generic layout is required before this step can run.
9. **Construct a temporary output package.** Preserve required styles and related parts. Write the complete row span with styled empty cells where needed; omit neither dates nor blank-column formatting. Check every cell reference and package relationship. The supplied design extracts one monthly sheet, so perform the cross-sheet-reference review before deciding that model is safe for the selected input.
10. **Validate before replacement.** Run the checklist below against a disposable output. Only replace the working output after the checks pass and the destination is confirmed. Do not overwrite an unreviewed file merely because it has the expected name.
11. **Move processed exports after success.** Move only exports actually reflected in the validated output into the sibling `Processed Files/` folder. Do not delete them or move the historical workbook. Check collisions and obtain required folder permissions first. Record each move.
12. **Deliver the output and summary.** State covered dates, qualifying/appended/excluded row counts, omitted fields, manual inputs preserved, policy decisions and remaining validation limits. Do not call a partial run complete.

## Package technique

Use direct ZIP/XML processing when a full spreadsheet object model is unsuitable for the file size. This is a technique, not a bundled implementation.

- Locate the monthly worksheet through workbook relationships, not a fixed sheet number.
- Preserve shared-string and style indices, namespace declarations, comments, hyperlinks and required drawing/print relationships consistently.
- Update worksheet dimensions and any affected ranges after adding rows or columns.
- Emit cells in strict left-to-right column order. A well-formed XML document can still describe the wrong spreadsheet.
- For a single-sheet package, make workbook metadata, application properties, content types and relationships agree on the actual included parts. Remove dangling calculation-chain references if that part is dropped.
- Do not copy stale multi-sheet application metadata into a one-sheet result.
- Inspect formulas that depend on omitted sheets before removing those sheets. Do not rely on cached values to hide broken references.
- Copy or derive styles from the current source rather than reusing numeric style IDs from another workbook.

## Validation checklist

- ZIP integrity passes and every XML part parses.
- Workbook metadata, content types and relationship targets agree.
- Source bytes are unchanged; only the intended destination is proposed for replacement.
- Output filename and tab month agree with the approved latest-month convention.
- Newly written cells are ordered, mapped to their own headers and typed correctly.
- Dates, quantities and identifier-shaped values match the headers they sit under.
- Row counts and totals match an independent recomputation from the input records.
- There is no duplicated delta or lost date between the queue and the output.
- Month flags and recap ranges preserve the approved open/closed-month state.
- Reconciliation uses the correct scope and preserves manual entries; a residual variance is reported, not hidden.
- Formula references remain valid or have an explicitly approved conversion. Test actual recalculation on a disposable copy.
- Open and visually inspect the resulting workbook: headers, borders, date formats, totals, reconciliation colors and visible errors. Parsing, cached values and sheet counts alone are insufficient.

## Stop conditions

Stop for missing mappings, uncertain output lineage, overlapping exports, unavailable financial rules, incompatible workbook formats or ambiguous product qualification. Do not silently redesign the financial workflow. No sample workbook or end-to-end test is included in this public repository; real use requires independent verification and qualified financial review.

## Example request

```text
Using synthetic inputs and the approved demo mapping, append unprocessed shipment exports in date order. Show validation results and unresolved decisions before replacing the working file.
```
