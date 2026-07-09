# Worked Examples

Two full walk-throughs showing the moves from SKILL.md composed end to end. The
first is a mechanistic-interpretability paper (the native habitat of this style);
the second is a non-technical strategy proposal, to show the structure is about
judgment, not domain. Annotations in `[brackets]` name the move being used —
they wouldn't appear in real output.

---

## Example A — a paper claiming a specific attention-head circuit implements a task

*Scenario: a paper claims that a small, identifiable circuit of ~26 attention
heads in a small transformer implements the Indirect-Object-Identification (IOI)
task, isolated via activation/path patching, and argues this shows models
implement modular, human-legible algorithms.*

**The claims, decomposed.** `[Move 1]` Four claims, each independently true-or-false:

1. *Empirical*: a specific set of heads (duplicate-token heads → S-inhibition
   heads → name-mover heads) implements IOI, with each class doing an
   identifiable sub-job.
2. *Methodological*: activation/path patching reliably isolates that circuit
   (the recovered circuit is faithful, complete, and minimal).
3. *Pragmatic*: this discovery recipe generalizes to other tasks and larger
   models.
4. *Framing*: this demonstrates that models implement modular, human-legible
   algorithms in general.

`[Move 2 — three axes, led by importance]` The claim that matters most is *not*
the specific IOI circuit — it's the **framing** claim (4), because "models run
legible modular algorithms" is what would change how I approach every future
model. But that's also the claim I'm *least* persuaded of on this evidence: one
clean circuit for one clean task is weak support for a general "legibility."
So: empirical claim (1) = high confidence, moderate importance; methodological
claim (2) = fairly persuaded, and it's a means; pragmatic claim (3) = the
weakest-supported, I'd discount it; framing claim (4) = most important, least
supported, and partly outside my standing (it shades into a claim about *all*
models I can't settle from one case).

**Why it should work, before the evidence.** `[Move 3 — first-principles prior]`
My model, independent of the paper: IOI requires resolving *which* of two names
is the indirect object. Mechanically that decomposes into detect-the-duplicated-
name, suppress it, and copy the remaining name to the output. Under a view where
models build compositional sub-computations, I'd *expect* a handful of components
with distinguishable roles rather than one monolithic IOI unit — so the paper is
supplying evidence for a structure I already had a prior on. That sets the bar:
the interesting question isn't "is there structure" but "did they show each head
does the *specific* job claimed, causally, not just correlationally."

`[Move 4 — worked example]` Concretely: "When John and Mary went to the store,
John gave a drink to ___" → "Mary". A duplicate-token head fires on the second
"John" (this name already appeared); an S-inhibition head writes "don't say the
duplicated one"; a name-mover head attends to "Mary" and copies it to the final
position. That division of labor is exactly the structure the circuit claim
asserts, and it's why a small legible circuit is plausible *for this task*.

**The evidence, graded.** `[Move 6, with Move 5 boring hypotheses]`

- The name-mover heads' attention pattern (attend to the IO token, and their
  output writes that token to the logits) is **compelling** — it's the causal +
  interpretable combination that's hard to get by accident. `[Move 5]` Boring
  hypothesis first: maybe ablating *any* high-impact head drops accuracy and
  these are just high-impact, not IOI-specific. What rules it out: the metric is
  the *logit difference between the two names*, and ablation flips it toward the
  *duplicated* name specifically — general capability loss would degrade fluency,
  not produce that precise directional flip.
- The faithfulness metric (the recovered circuit reproduces ~87% of the logit
  diff) is **compelling but not decisive** — the missing ~13%, and the existence
  of *backup* name-mover heads that activate when the primary ones are ablated,
  show the "circuit" is one path among redundant ones. That redundancy is itself
  interesting, but it undercuts a clean "*the* circuit" reading.
- The generalization claim (3) is **suggestive at most** — mostly asserted, with
  one or two illustrative extensions rather than a systematic demonstration. I
  don't lean on it.

**Practical read.** `[Move 8]` I'd treat this recipe as useful for **explaining a
specific behavior you already have in hand** — you have a narrow task, you want a
mechanistic account, patching gives you candidate components to test. I would
*not* expect the circuit you recover to be the whole story: expect backup/
redundant paths you didn't map, and expect the recipe to need real re-work on a
different task or a much larger model, where the clean picture tends to smear.

**Where I hold back.** `[Move 7]` On the framing claim — that this shows models
are *generally* modular and legible — I decline a strong verdict. One legible
circuit on one algorithmic task is genuinely cool, but the inference to "models
run human-legible algorithms" is a much larger claim about systems and tasks I
haven't seen analyzed, and I'd rather not launder enthusiasm for the specific
result into a general thesis. Because I've been concrete on claims 1–3, that
reticence should read as calibration, not evasion.

---

## Example B — a proposal to replace manual QA with an LLM-judge pipeline

*Scenario: a strategy memo argues the team should retire manual review of support
responses and adopt an automated LLM-judge that scores every response against a
rubric.*

**The claims, decomposed.** `[Move 1]`

1. *Empirical*: the LLM judge agrees with human reviewers at ~90%.
2. *Methodological*: the pipeline reliably measures the quality we actually care
   about (not just a proxy).
3. *Pragmatic*: it saves the team ~15 reviewer-hours/week.
4. *Framing*: "this makes QA continuous and data-driven — a senior reviewer on
   every ticket."

`[Move 2]` Most important: the **methodological** claim. A high agreement number
is worthless if the thing being measured is a proxy that drifts from real
quality. So that's where my scrutiny goes, and it's where I'm *least* persuaded
so far. The empirical 90% I half-believe (see boring hypothesis below). The
pragmatic time-saving I'm moderately persuaded of but think is overstated. The
framing claim I think is doing rhetorical work and I'd discount it.

**Why it might work, before the evidence.** `[Move 3]` My prior: LLM judges are
genuinely good at *surface* rubric items (tone, format, did-it-answer) and
genuinely unreliable at *substantive* ones (is the answer actually correct for a
gnarly case). Under that model, I'd expect agreement to be high on the easy
majority of tickets and to collapse exactly on the hard tickets that matter
most — which are the ones manual QA exists to catch.

**The evidence, graded, with boring hypotheses.** `[Moves 5 + 6]`

- The 90% agreement figure is **suggestive, not decisive.** `[Move 5]` Boring
  hypothesis: most tickets are easy, both humans and the judge rubber-stamp them,
  and the 90% is dominated by trivial agreement. What would make it cruxy is
  agreement *restricted to the hard, escalated tickets* — which the memo doesn't
  report. Until it does, I read the headline number as inflated.
- The time-savings estimate is **plausible but confounded**: it counts reviewer
  hours removed but not the new hours spent maintaining prompts and adjudicating
  the judge's mistakes. Net savings is probably positive but smaller than 15h.
- There's no evidence at all for the *drift* question — whether the judge stays
  calibrated as the product changes. That's the load-bearing risk and it's
  unaddressed.

**Practical read.** `[Move 8]` I'd support this **for triage, not for sign-off**:
let the judge flag likely-bad responses for a human to confirm, which preserves
the safety net while capturing most of the time savings. I would *not* support
letting it be the final gate on hard tickets — the failure mode (silently
passing a wrong answer on exactly the cases that matter) is the one manual QA was
protecting against.

**Where I hold back.** `[Move 7]` I can't judge the org/political side — whether
the reviewers freed up get redeployed well, or whether removing the human gate
erodes the team's felt ownership of quality. That's a management call outside my
standing. But on the measurement question, I'm confident: don't ship this as a
gate until you've seen agreement *on the hard subset.*

---

## What both examples share

- The verdict is never one number. It's 4 claims, each believed a different
  amount, with the *most important* one named up front.
- The prior comes before the evidence, so the evidence has something to update.
- Every flattering result meets its boring hypothesis before it's accepted.
- The grades span a real spectrum — "compelling" and "suggestive at most" live in
  the same review.
- Utility is bounded to a use-context and a failure mode, never left as "useful."
- The competence boundary is drawn explicitly, and it makes the committed claims
  *more* credible, not less.
