# Word-frequency workbook contract

This file defines the repeatable workbook input, calculation and output contract for `keyword.library.word-frequency`. The business meaning and non-inference boundaries remain owned by the project knowledge and judgment-boundary documents.

## Input lock

- Work from a copy of a category-cleaning workbook that has already passed `Sheet2 + Sheet3 + Sheet4 = source rows`.
- Read only the complete-keyword column in Sheet2. Do not include headers, blanks, Sheet3, Sheet4 or the unchanged source sheet.
- Treat each non-empty Sheet2 data row as one keyword occurrence. If identical complete keywords appear more than once without an existing explanation, stop and request review rather than silently deduplicating.
- Preserve every original worksheet, cell value, formula, style and table. Never overwrite the source file.

## Tokenization contract

For each Sheet2 keyword:

1. Create a statistical copy using Unicode NFKC compatibility normalization.
2. Lowercase English letters.
3. Treat whitespace, punctuation and hyphens as separators.
4. Keep letter and number sequences, including stopwords, numeric specifications and identifiable non-English words.
5. Do not merge singular/plural forms, synonyms, spelling variants, stems, word order or translations.
6. Do not use search volume, ABA, advertising, source count, SKU facts or model-generated corrections.

The original keyword remains unchanged in Sheet2.

## Counting contract

### Single-word frequency

Count every token occurrence. If a token appears twice in one complete keyword, count both occurrences.

### Two-word frequency

For tokens `t1 t2 t3`, count the adjacent ordered pairs `t1 t2` and `t2 t3`. Do not count non-adjacent `t1 t3`; `t1 t2` and `t2 t1` are different pairs.

### Sorting

- Primary key: occurrence count, descending.
- Tie-breaker: normalized term, ascending character order.
- Keep every unique single word and two-word pair, including count 1.
- “Frequency grading” means this ordering only. Do not create high/medium/low bands or a weighted score.

## Fixed output

Append one worksheet at the end of the copied workbook. For the current four-sheet cleaning contract, name it `Sheet5_词频统计`.

The sheet contains:

| Section | Required columns |
|---|---|
| Single-word frequency | rank, single-word term, occurrence count |
| Two-word frequency | rank, two-word term, occurrence count |
| Calculation note | source sheet, input keyword count, tokenization, non-merge and sorting rules |

Keep the two frequency tables easy to scan, preserve the workbook's visual language, use filters where supported and freeze header rows when useful.

## Quality gate

The result passes only when:

1. The input keyword count equals the number of non-empty Sheet2 keyword rows.
2. The sum of single-word counts equals the total number of generated tokens.
3. The sum of two-word counts equals `sum(max(token_count_per_keyword - 1, 0))`.
4. When every input row contains at least one token, the two-word total also equals `single-word total - input keyword count`.
5. The number of output data rows equals the number of unique single words or unique two-word pairs respectively.
6. Counts are descending and tie ordering is stable.
7. All source sheets are unchanged and the frequency sheet is last.
8. Formula-error scanning and a visual pass of every worksheet show no material defect.

If any gate fails, do not deliver the workbook as complete. Preserve the source and report the exact mismatch.
