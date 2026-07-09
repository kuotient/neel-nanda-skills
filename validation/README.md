# Validation

A reader should ask two questions about a "reason like X" skill, and each
deserves an answer you can check in under a minute:

1. **Does using it actually change anything?**
2. **Was the test that claims so actually sound?**

This page answers both, then shows the full detail underneath.

---

## Q1. Does the skill change anything?

**Blind structural audit** ([full report](structural-audit.md)): an auditor
model, not told which document was which, checked the no-skill and with-skill
commentaries against 8 binary criteria, quote required for every verdict.

| | Run 0: no skill | Run 3: with skill |
|---|---|---|
| 8-check blind audit | **6 PASS**, 1 partial, 1 fail | **7 PASS**, 1 fail* |
| States own prior before the evidence, uses it as the bar | partial | **PASS** |
| Explicitly declines a verdict for lack of standing | fail | **PASS** |

\* the with-skill fail (coverage honesty) is partly an artifact of our blinding
surgery; disclosed, not corrected, in [the report](structural-audit.md).

The delta is **narrow and specific, and we state it at that size**: the base
model already decomposes claims, grades evidence, and hunts confounds on its
own. What the skill reliably adds are the two habits above, which happen to be
the ones that make a judgment *auditable* rather than just sharp. The
auditor's own subjective summary: it would read the skill run as the main
analysis and the baseline as "the risk annex," because the baseline surfaced
external-validity risks the skill run missed.

For the version anyone can eyeball in 60 seconds, read
**[side-by-side.md](side-by-side.md)**: four verbatim excerpt pairs, including
one the baseline won.

## Q2. Was the test itself sound?

| Protocol item | Status |
|---|---|
| Reference standard exists and predates our runs | ✅ Neel Nanda's published commentary on the same paper |
| Blind | ✅ agents banned from reading any commentary or discussion of the paper |
| Contamination controlled | ✅ skill scrubbed of all target-paper content, keyword-verified; tested files preserved byte-for-byte in [`decontaminated-skill/`](decontaminated-skill/) |
| Ablation | ✅ no-skill baseline (run 0) under identical rules |
| Attribution controlled | ✅ headline run used a minimal prompt; the earlier enumerated-prompt run was demoted for exactly this confound |
| Structure delta measured blind | ✅ [structural audit](structural-audit.md) with verifiable quotes |
| Failed runs published | ✅ two runs disqualified/demoted and kept, with reasons ([process log](#process-log-what-we-got-wrong-along-the-way)) |
| Known limits stated | ✅ below, unhidden |

**Limits, stated plainly.** n = 1 run per condition, one model family
(Opus 4.8), one paper. The structural audit is an LLM applying a rubric we
wrote; its quotes are verifiable, its judgment is still a model's. The
convergence-with-Neel reading below is the authors' annotation, which is to
say: human-plus-LLM-as-judge, not an independent measurement. We label it as
such and treat it as suggestive, not decisive. One more disclosure: the
baseline agent's environment may have listed the installed skill's one-line
description among available skills (we later confirmed such leakage is
possible and now run baselines with the skill removed from the environment;
see the note in [../examples/](../examples/)). Behaviorally, run 0 did not
apply the skill's structure (it fails exactly the skill-specific audit
checks), and any influence would have pushed the measured delta toward zero,
making the reported delta conservative. If you replicate any of this on
another paper or model, we will link it.

---

## The four runs

All runs: Claude Opus 4.8, same paper
(*Verbalizable Representations Form a Global Workspace in Language Models*,
Transformer Circuits, 2026,
<https://transformer-circuits.pub/2026/workspace/index.html>), hard ban on
reading any existing commentary, outputs preserved unedited (their punctuation
included).

| Run | Skill | Prompt | Status | File |
|-----|-------|--------|--------|------|
| **0** | none | generic "opinionated expert commentary" | **baseline** | [`run0-baseline-no-skill.md`](run0-baseline-no-skill.md) |
| 1 | first draft (written directly from Neel's commentary on this very paper) | enumerated the moves | disqualified: answer-key contamination | [`run1-contaminated-commentary.md`](run1-contaminated-commentary.md) |
| 2 | decontaminated | enumerated the moves | demoted: prompt carried the structure | [`run2-clean-blind-commentary.md`](run2-clean-blind-commentary.md) |
| **3** | decontaminated | minimal ("read these files, apply the methodology") | **headline result** | [`run3-clean-minimal-prompt.md`](run3-clean-minimal-prompt.md) |

## How close did the skill run get to Neel Nanda?

**Label first: this section is author-annotated reading, not a blind
measurement.** Treat it accordingly. "Neel" refers to his published commentary
in the external-commentary collection linked from the paper page; quoted
fragments are short, and the original is better than any summary of it.

Where run 3 and Neel independently converged (neither saw the other):

- **Same prior, derived the same way.** Both argue from first principles that
  multi-step reasoning inside a forward pass forces intermediate variables into
  the residual stream, and both state this before weighing any result.
- **Same evidence hierarchy.** Both grade the causal interventions as the
  compelling core and the multilingual/structural results as texture. Both
  treat one specific control as what "rules out the boring version" of the
  headline causal result.
- **Same bounded verdict.** Run 3: "use it to *find*, never to *clear*." Neel:
  a hypothesis-generation tool, expect false positives, don't expect it to
  reliably flag everything.
- **Same declined claim.** Both explicitly hold back on the
  global-workspace-as-consciousness framing, and both say why: standing.

Where they diverge, the direction is consistent: Neel replicated the paper's
core findings on another model (Qwen 3.6 27B) and found something new; he
covers appendices and case studies the agent runs flagged as unread or skipped;
and his methodological asides draw on years of interpretability practice. The
structure transferred. The expertise didn't. We consider that the correct
outcome of a fair test, and it is the skill's honest ceiling.

## Process log (what we got wrong along the way)

Kept because deleting flawed runs is exactly what this project argues against.

1. **Contamination, found and fixed.** The skill's first draft was written
   directly from Neel's commentary on this very paper, so its worked examples
   contained answer-key fragments (the Michael Jordan example, the
   France→Paris boring hypothesis, the four-claim taxonomy as applied to this
   paper). Run 1 used those files.
   Interestingly, the agent didn't copy the canned examples and produced
   original judgments anyway, but the run can't count as evidence. We rebuilt
   the examples around a different paper (the IOI circuit), keyword-scanned for
   residue (`cognitive space`, `global workspace`, `Michael Jordan`, `France`,
   `Paris`, `J-lens`, `J-space`, `Jacobian`, `bandit`, `spider`,
   `working memory`, `verbaliz*`: all absent), and preserved the original in
   [`first-draft-skill/`](first-draft-skill/).
2. **Prompt confound, found and fixed.** Run 2's task prompt enumerated the
   eight moves as numbered instructions, so the structure could have come from
   the prompt rather than the skill. Run 3 removed the enumeration: "read these
   files, internalize the judgment structure, apply it."
3. **Eyeball grading, upgraded.** Our first scorecard was the authors reading
   both outputs and judging convergence, which is LLM/human-as-judge with extra
   steps. The [blind structural audit](structural-audit.md) replaced the
   with/without comparison with binary checks and verifiable quotes. The
   Neel-convergence reading above remains author-annotated and is labeled so.

## What shipped vs. what was tested

The skill in [`../skills/neel-nanda-analysis/`](../skills/neel-nanda-analysis/)
is the tested decontaminated skill plus documented post-test changes: the
"split the headline claim" paragraph in Move 1 and the coverage-honesty
paragraph in Move 7 (both folded back from what the runs showed), removal of the
Korean-specific localization the skill originally carried (it is
language-agnostic; output follows the user's language), and a
punctuation-style pass. The tested artifact is preserved byte-for-byte in
[`decontaminated-skill/`](decontaminated-skill/); diff them yourself.

## Reproducing

1. Give a fresh agent the files in
   [`decontaminated-skill/`](decontaminated-skill/) and the paper URL, with a
   minimal instruction ("read these methodology files, internalize the judgment
   structure, apply it to this paper").
2. Forbid reading any existing commentary or discussion of the paper.
3. Run a second agent with no skill and a generic "opinionated expert
   commentary" instruction as the baseline.
4. Run the [structural audit](structural-audit.md) on both outputs, blind.
5. Compare both against Neel Nanda's commentary in the external-commentary
   collection linked from the paper page.
