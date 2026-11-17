# Method

Every brief in this repository was produced by an AI agent from the OCR text of a public award, then passed
through an acceptance gate. Nothing was written from memory, and no award text was paraphrased into a
conclusion the source does not support.

## Pipeline

1. **Prepare.** OCR text is fetched from a document library, the script (Khmer or English) is detected, and
   a *numbered working text* is emitted: one sentence per line, split on the Khmer khan (។), each line
   prefixed with its number. That number is the citation key. The original OCR is kept byte-for-byte and
   its checksum recorded, so a brief can always be re-audited against the exact text it was written from.
2. **Claim.** A document is claimed with an atomic lock file before any work starts, so parallel workers
   cannot double-process it.
3. **Brief.** The reader writes six sections: facts, issues in dispute, positions of the parties, award and
   reasoning, ratio decidendi, and source defects disclosed. Issues keep the panel's own numbering; demands
   the panel recorded but never decided are described in §1, not presented as decided.
4. **Record.** The acceptance gate re-reads the brief and rejects it unless: all six sections are present;
   the `Forum:` line is line two; §2 is phrased as numbered questions; an `**Outcome:**` line exists; every
   pin cite resolves to a line that exists in the source; every sentence naming an Article, Prakas, award
   number or amount carries its own cite; and no individual is named in full.
5. **QC.** A structured decision model is asked a series of narrow typed questions — parties match, outcome
   matches the operative section, the issues are the panel's, §5 states a rule, §6 names concrete defects —
   each with its own threshold. Two rounds maximum; a third attempt is refused in code.

## Quality gates, and what they cannot do

A brief that passes the gate is *internally consistent and grounded in cited paragraphs*. That is not the
same as correct. The gate cannot tell you whether the panel's order was itself coherent, and it cannot
resolve an award whose OCR is destroyed. Where a document could not support a five-point brief — not an
award, OCR beyond recovery, too thin to summarise — it was excluded with a written reason instead of
fabricated.

## Privacy

Parties, witnesses, union officials and employer representatives are reduced to initials or roles.
Arbitrators are named, as they are named on the face of the award. Where OCR lost a name to a bare digit,
the brief says so rather than inventing one.

## Provenance

- 2,059 awards are briefed here, from a library of 2,078 documents; 3 were excluded as not-award or
  OCR-destroyed, and 16 are held back pending review.
- Each `[Para N]` was machine-verified against its numbered source line.
- All counts in `reports/digest.md` are computed from the brief text on every run; the argument in that
  file is authored prose, kept in `reports/synthesis.md` and spliced in.
