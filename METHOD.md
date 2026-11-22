# Method

Every brief in this repository was produced by an AI pipeline from the OCR text of a public award, then
graded, revised, and either published clean or published with the defect written into its frontmatter.
Nothing was written from memory, and no award text was paraphrased into a conclusion the source does not
support.

The governing rule: **no model grades its own work.** Drafting, editing and grading are separate roles held
by different model families, and the decision to publish is computed in code from the rubric's numbers, not
taken as the judge's opinion.

## Pipeline

1. **Prepare.** OCR text is fetched from a document library, the script (Khmer or English) is detected, and a
   *numbered working text* is emitted: one sentence per line, split on the Khmer khan (។), each line prefixed
   with its number. That number is the citation key. The original OCR is kept byte-for-byte with its checksum
   recorded, so a brief can always be re-audited against the exact text it was written from.
2. **Claim.** A document is claimed with an atomic lock before any work starts, so parallel workers cannot
   double-process it, and a document cannot be billed twice.
3. **Draft, twice.** Two independent writers, from different model families, each produce a full six-section
   brief from the same numbered text. Neither sees the other's draft.
4. **Edit.** A third model, a legal editor, merges the two drafts into one — taking the better-sourced version
   of each contested point rather than averaging them.
5. **Gate.** Deterministic checks run in code, not by a model: all six sections present; the `Forum:` line is
   line two; §2 is phrased as numbered questions; an `**Outcome:**` line exists; every `[Para N]` resolves to a
   line that exists in the source; every sentence naming an Article, Prakas, award number or amount carries
   its own cite; no individual is named in full.
6. **Grade.** An independent judge scores the brief against a fixed rubric of 10 criteria worth 42 points —
   parties, issues, outcome, citations, sourced facts, redaction, no invented authority, a stated ratio,
   specific defects, attributed positions. **Every brief is graded twice.** Where the two passes disagree the
   stricter reading is kept, because model verdicts are not stable enough to trust a single pass.
7. **Revise.** The judge's open gaps are handed back to the writers as an explicit repair list, and the brief
   is rewritten against them once. A third revision is refused in code: beyond that, the model is guessing.
8. **Record.** The result is written into the brief's frontmatter: verdict, confidence, and — when the rubric
   found something a person should fix — `review_required`, `review_action`, `review_reason` and
   `review_items`.
9. **Publish.** Additive commit, push, verify the remote sha. `reports/` is then rebuilt from the published
   corpus, so the index and digest can never drift from the briefs.

## What gets published, and why everything does

Briefs are published **whether or not they pass**. A rubric Fail, a low confidence score, or a structural
defect does not hide the document; it puts a work item on it:

```yaml
review_required: true
review_action: "correct-citations"
review_reason: "cites_support_claims: Cite [Para 80] for the equipment claim…"
```

Holding a failing brief back was tried and abandoned. It hid the documents that most needed an expert eye,
left no public record of why they failed, and made the operator the only reader who ever saw them. A brief
that names its own defect is more useful than a brief that does not exist — and it can be found with
`grep -rl "^review_required: true" briefs/`.

Briefs with `review_required: false` passed both grading passes with no open gap. Briefs with
`review_required: true` are drafts with a named defect. Neither is legal advice.

## Quality gates, and what they cannot do

A brief that passes is *internally consistent and grounded in cited paragraphs*. That is not the same as
correct. The gates cannot tell you whether the panel's order was itself coherent, cannot fix an award whose
OCR is destroyed, and cannot catch a misreading that is consistent across all four models. Where a document
could not support a five-point brief — not an award, OCR beyond recovery, too thin to summarise — it was
excluded with a written reason instead of fabricated.

Confidence and verdict measure different things. A brief can carry `ai_confidence: 0.94` and still be flagged
`review_required: true`: confidence scores how well the brief is grounded in its source, while the review flag
records a specific rubric failure. When they disagree, the review flag is the one that matters.

## Privacy

Parties, witnesses, union officials and employer representatives are reduced to initials or roles. Arbitrators
are named, as they are named on the face of the award. Redaction is enforced twice: by the writers, and by a
gate check that rejects a brief naming an individual in full. Where OCR lost a name to a bare digit, the brief
says so rather than inventing one.

## Provenance

- **2,068 awards**, issued 2003–2026, across 22 topics. One file per award, plus generated index and digest.
- Each `[Para N]` was machine-verified against its numbered source line. Verification proves the paragraph
  exists; it does not prove the paragraph says what the sentence claims.
- Every brief records its own verdict and confidence in frontmatter, and every publish is a git commit — the
  history is the audit trail.
- `reports/index/` and `reports/digest/` are rebuilt from the briefs on every publish, one page per year and
  per topic, so no single file grows into something unreadable. Every number in them is computed from the
  brief text; nothing is hand-maintained.

## Reproducing it

The tooling is not public. It runs against a private document library and a paid API, and a re-run changes
what it produces: model versions move, and two runs over the same award will not be byte-identical. What is
reproducible is the audit: for any brief you can check every pin cite against the award and see exactly which
sentence the source supports and which it does not. That is the check this repository invites.