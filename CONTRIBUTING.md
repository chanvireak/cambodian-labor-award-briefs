# Contributing

Two kinds of contribution: **correcting a brief**, and **adding a new award**.

## Correcting a brief

Open an issue with:

1. the document id (the filename, e.g. `140123`),
2. the paragraph number in the brief's citation scheme (or the sentence you dispute),
3. what the award actually says, with the paragraph of the award that says it.

In scope: factual errors (wrong party, outcome, amount, date), a ratio attributed to a paragraph that does
not state it, privacy failures (an individual named who should be an initial), and OCR defects the brief
did not disclose. Out of scope: disagreement with the panel's reasoning — these briefs report what the
panel decided, not whether it was right.

## Adding a new award

A brief is one file, filed by **the year of the award** and then by **topic**:

    briefs/<year>/<topic>/<doc-id>.md          e.g. briefs/2012/maternity/140112.md

### 1. Pick the year — from the award's date, not its number

Use the date the panel issued the award, as printed on its face or in the
`(Arbitration Council, <date>)` parenthetical of the title. **Do not derive the year from the award number**
(`128/11` is an index, not a date, and the two disagree in this corpus — a `/12` award issued in January 2013
files under 2013).

Accepted date forms: `29 June 2012`, `December 29, 2003`, `23-07-2003`, `11-2003`, `February 2004`.
If no date is readable, file under `briefs/unknown/<topic>/` and say so in the PR — do not guess.

### 2. Pick the topic — the first that applies, in this order

    most_representative, interest_dispute, fixed_term_to_indefinite, union_leader_protection,
    wages_during_suspension, severance_pay, reinstatement, dismissal_compensation, notice_pay,
    maternity, union_dues, annual_leave, attendance_bonus, seniority_bonus, meal_allowance,
    overtime, night_work, nursery, probation, strike, occupational_safety

Otherwise `general`. An award appears under **one** topic only — the rule that actually decided it — so the
topic counts add up to the number of briefs. The priority order exists because most of these cases turn on
standing (`most_representative`) or classification (`interest_dispute`) before anything else is reached.

### 3. Write the brief

Six sections, in this order and with these headings:

    # Award No. N/YY — <employer> (Arbitration Council, <date>)
    Forum: Arbitration Council of Cambodia · <panel> · Doc ID <id>

    ## 1. Facts
    ## 2. Issues in dispute          numbered questions, keeping the panel's own numbering
    ## 3. Positions of the parties
    ## 4. Award and reasoning        must contain an **Outcome:** line
    ## 5. Ratio decidendi
    ## 6. Source defects disclosed

Rules that are checked, not advisory:

- 250–1,400 words; every `[Para N]` counts as one word.
- At least eight `[Para N]` citations, each resolving to a line of the numbered source text.
- Every sentence naming an Article, Prakas, award number or amount carries its own `[Para N]`.
- §2 holds only issues the panel **decided**; demands it recorded but never reached belong in §1.
- `**Outcome:**` follows the operative order, not the reasoning.
- Workers, witnesses, union officials and employer representatives are initials or roles. Arbitrators are
  named, as they are on the face of the award.

### 4. Regenerate the index and digest

Both are generated from the briefs — never hand-edit them:

    python3 publish.py --out <repo>

That rewrites `reports/index/` and `reports/digest/` (one page per year and per topic), so a new award adds
rows to exactly two pages — `by-year/<year>.md` and `by-topic/<topic>.md` — in each tree, plus updated counts
in the four `README.md` files and `digest/corpus.md`. If you cannot run the generator, submit the brief alone
and note it in the PR; the maintainer will regenerate rather than review hand-written tables.

## Pull requests

- One award per PR, or one correction per PR. A PR touching many briefs cannot be reviewed.
- State the award number, date, employer and outcome in the description so the reviewer can spot-check.
- If a brief and the award disagree, the award controls — say which award you read.
