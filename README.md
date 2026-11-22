# Cambodian Labour Arbitration Award Briefs

**AI-generated five-point case briefs of Cambodian labour arbitration awards — research notes, not legal
advice.** Written by an agent from the OCR'd Khmer and English originals of Arbitration Council awards.
Corrections are welcome and expected.

2,068 awards · 2003–2026 · 22 topics · English · one Markdown file per award.

---

## What this is

- A **finding aid**. Each file answers five questions about one award: what happened, what the panel was
  asked to decide, what each side argued, what the panel ordered and why, and what rule the case stands for.
- A **citation map**. Every sentence that names an Article, a Prakas, an award number or an amount carries a
  `[Para N]` pin cite into a numbered working text of that award, machine-verified to resolve.
- A **defect record**. Section 6 of each brief lists the dates, amounts and names the award contradicts in
  different places, or that OCR destroyed. Read it before quoting a figure.
- **Searchable as a corpus.** `reports/index/` is one page per year and per topic with links; `reports/digest/`
  is the cross-case view — how a topic is usually decided, what the recurring ratios are.

## What this is not

- **Not the award.** The authoritative text is the Arbitration Council's own document. This repository
  publishes structured notes about awards; it does not republish the awards. Where a brief and an award
  disagree, the award controls and the brief is wrong.
- **Not legal advice.** No attorney-client relationship, no opinion on your matter. Obtain advice from a
  qualified Cambodian lawyer.
- **Not precedent.** These are first-instance arbitration decisions. They are evidence of how panels reason,
  not binding law.
- **Not complete.** It covers the awards that reached this pipeline and whose OCR could support a brief.
  Awards that were unreadable, or were not awards at all, are excluded rather than invented.
- **Not all reviewed by a person.** Briefs are machine-written and machine-graded. Some are marked
  `review_required: true` in their frontmatter — the pipeline found a specific defect and nobody has fixed it
  yet. Treat those as drafts.
- **Not a substitute for reading.** A brief compresses 10–40 pages into 250–1,400 words. The compression is
  where meaning is lost.

## Read this before using anything here

- These are **machine-generated research notes**, not law.
- Every `[Para N]` resolves against the numbered working text of the award. Resolution is **not** correctness:
  the cited paragraph exists, and the sentence may still misread it.
- OCR damage is widespread in the underlying documents. Each brief discloses the specific defects it worked
  around in §6 instead of silently repairing them. A defect the agent did not notice may still be present.
- **Check the frontmatter before you rely on a brief.** `review_required: true` means something in that file
  is known to be wrong — see [The review queue](#the-review-queue).

## How to use it

**Find an award by year or by topic** — start at the index, not the file tree:

- `reports/index/by-year/2014.md`, `reports/digest/by-topic/severance_pay.md`
- `reports/digest/corpus.md` — how many awards per year, per topic, per outcome

**Find a rule, not a case.** Search the corpus, then read §5 (ratio) of the candidates:

```bash
grep -rl "fixed-term contract.*indefinite" briefs/ | head
grep -rn "Article 73" briefs/2015/ | head
```

**Find what most needs a reader.** Every brief carrying a known defect:

```bash
grep -rl "^review_required: true" briefs/
```

**Quote it properly.** Cite the award, and describe the brief as an AI-generated summary:

> Case brief of Award No. 128/11 (Arbitration Council, 2011), AI-generated summary, verified against
> [Para 131] — then cite the award itself.

**An award number is not a date.** `128/11` is an index — `/11` is a sequence, not a year. Briefs are filed by
the date the panel issued the award, so a `/12` award issued in January 2013 is filed under `2013`. Never
infer a year from an award number.

## The review queue

Frontmatter carries the work item, so the corpus can be filtered instead of read:

```yaml
review_required: true
review_action: "correct-citations"     # what to do
review_reason: "cites_support_claims: Cite [Para 80] for the equipment claim…"
review_items:                          # the grader's findings, with paragraph anchors
  - "cites_support_claims — Cite [Para 80] for the equipment claim…"
review_verdict: "Needs Improvement"
review_confidence: 0.881
```

| `review_action` | What it means |
| --- | --- |
| `verify-redaction` | an individual may be named who should be an initial — **fix first** |
| `remove-invented-authority` | the brief cites a rule, Prakas or award that does not appear in the source |
| `correct-citations` | a pin cite is wrong, missing, or attached to the wrong sentence |
| `verify-outcome` | the `**Outcome:**` line does not follow the award's operative order |
| `check-parties` | a party, employer or union is misdescribed |
| `review-ratio`, `review-issues`, `review-defects` | §5, §2 or §6 needs a human read |
| `review-structure` | a required section is missing or malformed |

Pick one, fix it, open a pull request. `CONTRIBUTING.md` has the format for both.

## What is in here

| Path | What it is |
| --- | --- |
| `briefs/<year>/<topic>/<doc-id>.md` | One brief per award, filed by the year it was issued, then by the one topic that decided it. |
| `reports/index/` | Browsable index: `by-year/` and `by-topic/`, one page per group, each linking to the brief. |
| `reports/digest/` | Cross-case digest: `corpus.md`, plus `by-topic/` and `by-year/` pages with the ratios. |
| `METHOD.md` | How the briefs are produced, the citation scheme, the gates — and what the gates cannot do. |
| `DISCLAIMER.md` | The full disclaimer in one place. Read it before relying on anything here. |
| `CONTRIBUTING.md` | How to correct a brief, add an award, or fund the compute. |
| `CITATION.cff` | Machine-readable citation metadata. |

`reports/` is generated, never hand-edited: it is rebuilt from the briefs on every publish.

The underlying award texts are **not** published here. They are public documents of the Arbitration Council
of Cambodia; only structured notes about them are reproduced.

## Privacy

Parties, witnesses, union officials and employer representatives are reduced to initials or roles. Only
arbitrators are named, as they are named on the face of the award. Where OCR lost a name to a bare digit, the
brief says so rather than inventing one.

A privacy failure is the most serious error you can report. Open an issue marked `privacy`; it is handled
before anything else.

## Cost, and how to help pay for it

Every brief is written by language models against a paid API. One award costs roughly **USD 0.02–0.06** to
fetch, draft twice, edit, grade twice and revise. The whole corpus cost roughly **USD 90–130** to produce and
costs about the same to re-run after a prompt change. New awards arrive at a few dollars a year.

**Contributing time is worth more than money.** One corrected brief is worth more than ten regenerated ones.
If you still want to cover compute:

- Open an issue titled `funding` saying what you want to support — re-running a year, a topic, a full refresh.
  The maintainer replies with the actual amount and a payment route. Nothing here is a donation funnel: no
  funds are collected until you have a number and a purpose in writing.
- Money buys API credits first. Anything beyond that pays a lawyer to review the briefs flagged
  `review_required: true`. It buys no editorial influence — the award still controls.

## License

MIT — see `LICENSE`. Applies to the briefs and reports here. The underlying awards remain public documents of
the Kingdom of Cambodia and are not relicensed.

## Citing this work

Cite it as _machine-generated research notes_, not as a source of law — e.g. "Case brief of Award No. 128/11
(Arbitration Council, 2011), AI-generated summary, verified against \[Para 131\]" — and then cite the award
itself. `CITATION.cff` carries the same metadata in machine-readable form.

Maintained by **Seng Chanvireak** ([@chanvireak](https://github.com/chanvireak)). Corrections welcome — see
`CONTRIBUTING.md`.