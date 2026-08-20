# Category-cleaning workbook contract

This file defines the repeatable workbook output for `keyword.library.clean`. Category and substitute decisions must follow the separate project judgment-boundary document.

## Input lock

- Preserve the complete source sheet and its original fields.
- Record source row count, category-anchor version and cleaning version before processing.
- Keep original row number or Keyword_ID as the stable cross-stage key.
- Do not collect new keywords in this stage.

## Fixed output

| Sheet | Content |
|---|---|
| Sheet1 | Complete source data, unchanged |
| Sheet2 | Traffic gate passed and the complete keyword remains in the top-level category |
| Sheet3 | Traffic gate failed/missing, or the term is an undeclared third-party brand/IP, Gift, accessory/complement, broad/fragmentary, service, multi-product, unrecognized or unrelated |
| Sheet4 | Traffic gate passed, no brand/Gift exclusion, and the complete product is a direct substitute; header only is valid |

Each classified row retains at least:

- Keyword_ID or original row number.
- Original keyword and original traffic fields.
- ABA-gate result, search-volume-gate result and final OR result.
- Center purchase object.
- Primary judgment type.
- Optional additional hit labels.
- One final destination.
- Concrete semantic reason, not only “low relevance”.
- Canonical ABA/search-volume source and fallback status.
- Applicable labels for English-letter typo, other language and self-brand; no correction/suggested-correction field.

## Execution sequence

1. Normalize only for judgment while retaining the original text.
2. Apply the confirmed OR traffic gate.
3. Route a failed traffic gate to Sheet3 and exclude it from the current main frequency population.
4. Apply brand/IP and Gift routing, keeping declared self-brand target-product terms eligible for Sheet2.
5. Determine whether the keyword independently expresses a complete purchase object.
6. Identify the center object and route it using the project Sheet2/3/4 boundary.
7. Retain multiple diagnostic labels if useful, but assign one primary type and one final destination.
8. Keep recognizable English-letter typo and other-language target-product phrases in Sheet2 with labels; do not create corrected keywords. Route adjacent complete substitute products to Sheet4 when all substitute conditions hold.
9. If the keyword itself cannot resolve its purchase object, route it to Sheet3 with a concrete reason; never leave it outside the three destinations or interrupt the run for every row.
10. Reconcile counts, scan formula errors, render every sheet and complete human visual review. After a confirmed boundary change, rerun and produce an old-to-new destination diff.

## Quality gate

The cleaning result passes only when:

1. `Sheet2 data rows + Sheet3 data rows + Sheet4 data rows = Sheet1 source rows`.
2. Every source row has exactly one destination and a stable source key.
3. Source-provided relevance fields are preserved but never used as the final decision.
4. Same-category subtype, appearance, structure, attribute, function and configuration terms are not deleted because the target SKU lacks them.
5. High-traffic brand, Gift and accessory terms do not enter Sheet2 merely because they contain the category anchor.
6. Broad, scene-only and incomplete expressions do not enter Sheet2 through weak association.
7. Every Sheet4 term is a complete direct substitute; accessories and merely adjacent products are excluded.
8. Removed rows retain metrics, judgment type and reason.
9. A term with ABA rank above 1,000,000 but search volume above 100 still receives semantic review.
10. Sheet4 may contain zero data rows.
11. Sheet2 is not represented as a target-SKU exact-match library.
12. English-letter spelling errors and other languages are not automatic Sheet3 reasons; declared self-brand and undeclared third-party brands are distinguished.

## Current downstream boundary

Only all non-empty Sheet2 complete keywords participate in the downstream word-frequency step. Sheet3, Sheet4 and traffic-gate failures do not. This cleaning Skill does not calculate frequency; after its quality gate passes, route the workbook to `amazon-keyword-word-frequency`, which owns the confirmed V2.2 tokenization, counting, sorting and `Sheet5_词频统计` output contract.
