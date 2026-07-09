# Blind structural audit: run 0 vs run 3

This is the systematic version of "does the skill change anything." Instead of
the authors eyeballing two commentaries and declaring a winner, a separate
auditor model checked both against a fixed rubric of 8 binary criteria, blind
to which document was which, with a verbatim quote required for every verdict.
Anyone can verify any cell of the result by searching the quoted string in the
run files.

## Method

- **Documents.** Commentary A = [run 3](run3-clean-minimal-prompt.md) (skill,
  minimal prompt). Commentary B = [run 0](run0-baseline-no-skill.md) (no
  skill). The auditor was not told this mapping, was instructed not to guess
  provenance, and judged each document on its text alone.
- **Blinding surgery.** Both documents' "Coverage notes" appendices and our
  run headers were removed before auditing, because run 3's coverage notes
  mention "methodology files" (which would reveal it had a skill). Everything
  else is verbatim.
- **Auditor.** Claude Opus 4.8, single pass, strict-quote instructions. The
  full prompt is reproduced at the bottom of this file.
- **Known artifact (disclosed, not corrected).** Check 6 (coverage honesty)
  is distorted by the blinding surgery: run 3's coverage honesty largely lived
  in its stripped Coverage-notes appendix (where it flags appendices unread and
  figures unverified), so its FAIL on check 6 partly measures our surgery, not
  the document. We report the number as the auditor produced it and flag it
  here instead of adjusting it.

## Result

| # | Check | Run 0 (no skill) | Run 3 (skill) |
|---|-------|------------------|---------------|
| 1 | Enumerated claim decomposition, different confidence per claim | PASS | PASS |
| 2 | States own prior BEFORE evaluating evidence, uses it as the bar | **PARTIAL** | **PASS** |
| 3 | Boring-hypothesis discipline (mundane alternative + what rules it out, ≥2 results) | PASS | PASS |
| 4 | Graded evidence vocabulary (≥3 distinct strength levels) | PASS | PASS |
| 5 | Explicitly declines a verdict for lack of standing | **FAIL** | **PASS** |
| 6 | Coverage honesty (marks unchecked material as unassessed) | PASS | FAIL* |
| 7 | Bounded practical verdict (use context + failure mode) | PASS | PASS |
| 8 | Falsifiability (names what evidence would change the verdict) | PASS | PASS |
| | **Total** | **6 PASS, 1 PARTIAL, 1 FAIL** | **7 PASS, 1 FAIL*** |

\* see "Known artifact" above: run 3's coverage honesty was in the section we
had to strip for blinding.

## The honest reading of this table

The delta is **narrow and specific, not dramatic**. Without the skill, the
model already decomposes claims, grades evidence on a spectrum, hunts boring
hypotheses, and states falsifiers. What the skill reliably adds, per a blind
auditor working from quotes:

1. **A prior stated before the evidence and used as the grading bar**
   (check 2: PARTIAL → PASS). The auditor's quote from run 3: *"Here is my own
   model of what the J-lens must be, derived without the paper."*
2. **A declared competence boundary** (check 5: FAIL → PASS). The auditor
   found no qualifying passage anywhere in run 0: it renders verdicts on every
   claim, including the consciousness framing.

Those two habits are exactly the difference between a sharp take and an
auditable one. That is the product claim, no larger.

## Auditor's subjective note (quoted in full, labeled subjective by the auditor)

> My subjective judgment, separate from the mechanical audit: it is close, with
> complementary strengths. A has the more complete reasoning scaffold — an
> explicit prior it holds the paper against, a declared competence boundary,
> and a crisp operational verdict ("find, never clear") — which lets a reader
> re-derive its conclusions rather than trust them. B uniquely surfaces
> decision-critical external-validity risks A omits entirely: single
> closed-model family, no open-weights replication, and a training-confound
> (Claude optimized to narrate itself) that could explain the introspection
> results. For pure decision input I lean A for its disciplined, auditable
> trail, but B's reproducibility and training-confound caveats are exactly what
> bounds real-world reliance — I would read both, treating B as the risk annex
> to A.

We are publishing that note unedited even though "read both" is a less
flattering headline than "the skill wins," because it is the accurate one.

## Full audit output (verbatim)

<details>
<summary>Expand: the auditor's complete check-by-check output with quotes</summary>

### Check 1 — Enumerated claim decomposition
**A: PASS** — Numbered 5-claim list (1. Methodological … 5. Pragmatic), then differentiated: "I am fairly-to-well persuaded of (2) and (3); I take (1) as a real but partly definitional advance; I treat (4) as an organizing metaphor that paid rent"

**B: PASS** — "I want to separate this into six claims and grade them independently" — and the Bottom-line list assigns distinct grades to distinct claims ("High confidence" / "Moderate-to-high confidence" / "Weak-to-moderate" / "Moderate but under-controlled").

### Check 2 — Prior before evidence
**A: PASS** — Dedicated section "My prior, before their evidence": "Here is my own model of what the J-lens *must* be, derived without the paper." Followed by "Under that model I would predict three things before reading a single result" and used as "the bar."

**B: PARTIAL** — "exactly what you'd predict from the construction" — predict-from-construction is a passing remark, not a distinct prior-before-evidence reasoning step.

### Check 3 — Boring-hypothesis discipline
**A: PASS** — Result 1: The boring hypothesis — "perturbing any high-impact direction degrades the answer" — is killed by that specificity: general degradation gives you fluency loss, not "6." Result 2 (ablation): matched-norm control "directly answers the 'you just deleted 10 directions, of course things break' boring hypothesis."

**B: PASS** — Result 1 (dissociation): over-determination worry, "the fact that the concept is *present in the J-lens at equal rates* and still doesn't move the automatic output substantially blunts that concern." Result 2 (capacity): "Small fraction of variance" may partly reflect that denominator, not a clean capacity bound.

### Check 4 — Graded evidence vocabulary
**A: PASS** — Three+ distinct grades: "Suggestive, and partly circular", "Compelling", "Interesting, under-controlled".

**B: PASS** — Three+ distinct grades: "moderate-to-high confidence and genuinely important", "Solid, moderate confidence.", "suggestive, weak-to-moderate".

### Check 5 — Declared competence boundary
**A: PASS** — "is partly outside my standing; I can weigh the ML content, the causal interventions, and the confounds, but not referee the cross-disciplinary mapping at full resolution."

**B: FAIL** — no qualifying passage found. (B renders verdicts on every claim including consciousness; declines to update for lack of *evidence*, never for lack of the author's *standing*.)

### Check 6 — Coverage honesty
**A: FAIL** — no qualifying passage found. (A's admitted limits are competence/analytic — "I can't fully exclude it," "outside my standing" — never coverage gaps like unseen figures or unfound metrics.)

**B: PASS** — "the fetch confirms **no random-injection or wrong-concept false-positive baseline is reported.**" Also flags "sizes undisclosed" and no open-model replication as things not found.

### Check 7 — Bounded practical verdict
**A: PASS** — "use J-lens readouts for *hypothesis generation*" but "Do **not** use it to *certify the absence* of a hidden intention" — "Use it to *find*, never to *clear*."

**B: PASS** — "My advice to anyone citing this: cite the dissociation, not the ocean." Specific use (citing this work) plus specific anti-use (the consciousness metaphor). (Citation-scoped, thinner than A's deployment verdict, but an explicit do/don't.)

### Check 8 — Falsifiability statement
**A: PASS** — "Until the same-principles SFT baseline exists, I read this as "a possibly-more-efficient, possibly-just-equivalent way to instill a disposition," not as proof the mechanism is special."

**B: PASS** — "'majority of trials' is uncalibrated without knowing how often the model confidently reports a concept that wasn't injected." (Names the specific evidence whose absence blocks a verdict.)

</details>

Note: the em-dashes inside the audit block are the auditor's own output,
preserved verbatim like the run transcripts.

## Limits of this audit

- One auditor, one pass. Rubric-check extraction is far more reproducible than
  free-form quality judgment, but it is still an LLM reading text.
- The rubric was written by us. It operationalizes the eight moves the skill
  teaches, so it measures "does the skill's structure appear," not "is the
  commentary good." The auditor's subjective note exists precisely because
  those are different questions.
- n = 1 document per condition, one model family, one paper.

## Reproduce

1. Strip the headers and Coverage-notes sections from
   [`run0-baseline-no-skill.md`](run0-baseline-no-skill.md) and
   [`run3-clean-minimal-prompt.md`](run3-clean-minimal-prompt.md).
2. Label them B and A (or re-randomize; if you flip the labels, flip them in
   the prompt too).
3. Give a fresh model the prompt below and compare its table to ours.

<details>
<summary>Expand: the exact auditor prompt</summary>

```
You are a blind structural auditor. You will read two expert commentaries on
the same research paper, labeled A and B. You do NOT know who or what produced
them, and you must not try to guess. Your job is purely mechanical: check each
document against a fixed rubric of binary structural criteria, with exact
supporting quotes.

[The 8 checks as listed in the Result table above, each defined in one
sentence, followed by:]

Output format (strict): for each check, for each document: PASS/FAIL plus the
exact quote (verbatim, max ~40 words) that justifies the verdict, or "no
qualifying passage found" for FAIL. Borderline cases: PARTIAL plus a <=15 word
explanation. Then a summary table. Then one final section titled "Auditor note
(subjective, secondary)": <=120 words on which document you would trust more
as decision input, clearly labeled as subjective.

Rules: quotes must be verbatim substrings. Judge each document on its own text
only. No guessing about provenance, no style commentary. Be strict: a check
passes only if the text actually does the thing.
```

</details>
