# Contributing

Four ways to help, in order of how much they are worth:

1. **Fix a brief that is flagged as needing review.** Highest value: the defect is already written down.
2. **Report an error in a brief that is not flagged.** Nothing found it; you did.
3. **Add an award that is missing.** Lowest priority — the pipeline already generates new briefs automatically.
4. **Fund the compute.** Docs, model calls and OCR are paid for; see [Funding](#funding).

You do not need to know Cambodian labour law to help with 1 and 2. You need the award in front of you.

## Start here: the review queue

Every brief whose grading found a defect carries the defect in its frontmatter:

```yaml
review_required: true
review_action: "correct-citations"
review_reason: "cites_support_claims: Cite [Para 80] for the equipment claim…"
review_items:
  - "cites_support_claims — Cite [Para 80] for the equipment claim…"
review_verdict: "Needs Improvement"
review_confidence: 0.881
```

```bash
grep -rl "^review_required: true" briefs/            # everything that needs a person
grep -rl '^review_action: "verify-redaction"' briefs/   # privacy first, always
grep -rl '^review_action: "correct-citations"' briefs/  # the biggest bucket
```

Take one file, check the finding against the award, and either fix it or say the grader was wrong. Both
outcomes are useful — a false positive removed is as valuable as a citation corrected.

## Correcting a brief

Open an issue, or send a pull request that changes exactly one brief.

**Issue format**

```
Brief: briefs/2015/severance_pay/140123.md  (doc id 140123, Award No. 128/11)
Finding: review_action "correct-citations", Para 44
What the brief says: "…"
What the award says: "…" (Award No. 128/11, Para 44)
```

**In scope**

- Factual errors: wrong party, employer, union, date, amount, or outcome.
- A ratio attributed to a paragraph that does not state it.
- A citation that does not support its sentence, or points at the wrong paragraph.
- An invented Article, Prakas, notice or award number.
- **Privacy**: an individual named in full who should be an initial. Mark the issue `privacy`.
- An OCR defect the brief did not disclose in §6.
- A missing or malformed section (§2 must be numbered questions; §4 must carry an `**Outcome:**` line).

**Out of scope**

- Disagreement with the panel's reasoning. These briefs report what the panel decided, not whether it was
  right. If the panel was wrong, say so in your own article and cite the award.
- Style preferences, American vs British spelling, or rewording that changes no fact.
- Rewriting a brief from scratch because you would have written it differently.

**The rule that settles every dispute:** if the brief and the award disagree, the award controls. Quote the
paragraph of the award you read, by number, in the issue or PR description.

## Adding a new award

The pipeline normally does this automatically, so check that the award is genuinely missing before writing one
— search `reports/index/by-year/<year>.md` for the award number and `grep -rl "<award no>" briefs/`.

A brief is one file, filed by **the year of the award** and then by **topic**:

    briefs/<year>/<topic>/<doc-id>.md          e.g. briefs/2012/maternity/140112.md

### 1. Pick the year — from the award's date, not its number

Use the date the panel issued the award, as printed on its face or in the `(Arbitration Council, <date>)`
parenthetical of the title. **Do not derive the year from the award number** (`128/11` is an index, not a date,
and the two disagree in this corpus — a `/12` award issued in January 2013 files under 2013).

Accepted date forms: `29 June 2012`, `December 29, 2003`, `23-07-2003`, `11-2003`, `February 2004`. Write the
frontmatter `date:` as ISO (`2013-01-24`), and **in ASCII digits** — Khmer numerals in `date:` or `year:` have
already put one brief in a directory named `២០០៨`.

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

Frontmatter for a contributor-written brief:

```yaml
date: 2013-01-24
year: 2013
review_required: false        # true if you know something is still wrong — say what
review_action: ""             # set only when review_required is true
```

Do not touch `reports/`. It is generated.

### 4. Do not touch `reports/`

`reports/index/` and `reports/digest/` are generated from the briefs by the maintainer's pipeline on every
publish — one page per year and per topic. They rebuild completely: anything hand-written in `reports/` is
deleted by the next publish, including hand-maintained tables.

So submit the brief, not a regenerated index. If your PR adds a brief and the index pages still list the old
counts, that is expected; the maintainer's next publish corrects them.

## Pull requests

- One award per PR, or one correction per PR. A PR touching many briefs cannot be reviewed.
- State the award number, date, employer and outcome in the description so the reviewer can spot-check.
- If a brief and the award disagree, the award controls — say which award you read.
- A PR that fixes a `review_required: true` brief may also set `review_required: false` and clear
  `review_action` / `review_reason` / `review_items`. Do not clear them if you only fixed part of the finding.

## Funding

Every brief costs real money: roughly **USD 0.02–0.06** per award for drafting twice, editing, grading twice
and revising — about USD 0.045 measured, so on the order of **USD 90** for the whole 2,068-award corpus, and
about the same again to re-run it after a prompt change. Contributing time is worth more than money — one
corrected brief beats ten regenerated ones — but if you want to cover compute:

- Open an issue titled `funding`, saying what you want to support (a year, a topic, a full refresh). The
  maintainer replies with the amount and a payment route. No funds are collected without a number and a stated
  purpose.
- Money buys API credits and OCR time first. Beyond that it pays a lawyer to review the briefs flagged
  `review_required: true`. It buys no editorial influence: the award controls, always.

## What maintainers will ask you

- Which award did you read, and which paragraph?
- Is this a correction to the brief, or a disagreement with the panel? Only the first is a change to this
  repository.
- Does your change introduce any individual's name?

Answer those three in the PR and review is fast.