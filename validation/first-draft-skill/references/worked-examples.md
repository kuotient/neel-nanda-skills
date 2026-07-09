# Worked Examples

Two full walk-throughs showing the moves from SKILL.md composed end to end. The
first is a mechanistic-interpretability paper (the native habitat of this style);
the second is a non-technical strategy proposal, to show the structure is about
judgment, not domain. Annotations in `[brackets]` name the move being used —
they wouldn't appear in real output.

---

## Example A — an ML paper claiming a "cognitive space" inside models

*Scenario: a paper introduces a lens (call it J-Lens) that reads out intermediate
variables from a model's residual stream, and argues these form a working-memory-
like "cognitive space" analogous to a global workspace.*

**The claims, decomposed.** `[Move 1]` There are really four claims here, and they
deserve very different treatment:

1. *Scientific*: there is a cognitive space — a place where the model stores
   intermediate variables mid-computation.
2. *Methodological*: J-Lens is a good way to find and read that space.
3. *Pragmatic*: you could use this for auditing or forensics on real models.
4. *Philosophical*: this is meaningfully like a "global workspace."

`[Move 2 — the three axes, led by importance]` The claim that matters most is not
the method — it's the **scientific** one. If models really do maintain a
working-memory-like space with consistent internal directions for concepts, that
reshapes how I think about what's happening inside a forward pass. The method is
downstream of that. So: scientific claim = most important, and I'm fairly
persuaded. Methodological claim = I'm also persuaded, but it interests me less;
it's a means. Pragmatic claim = partially persuaded, with real caveats below.
Philosophical claim = the one I'm *least qualified* to assess, so I'll hold back
on it rather than fake a view.

**Why it should work, before the evidence.** `[Move 3 — first-principles prior]`
Here's my model, independent of the paper. If a language model does two or three
steps of reasoning inside a single forward pass, the intermediate results have to
be *stored somewhere* between steps. The residual stream is the bottleneck
between layers, so those intermediates are very likely written as directions in
the residual stream. Under that model, I should *expect* something like a
cognitive space to exist — so the paper isn't creating a belief from nothing;
it's supplying evidence for a structure I already had a prior on. That matters:
it means the bar for the evidence is "does it show the *specific* properties," not
"does such a thing exist at all."

`[Move 4 — worked example building intuition]` Concretely: "Michael Jordan plays
the sport of…". The model identifies Jordan from the token, looks up not just
basketball but adjacent facts (Chicago, the Bulls), and pulls out the piece it
needs when it needs it. If several downstream circuits all have to read "the
sport this entity plays," it's *efficient* for the model to maintain one
consistent internal direction for basketball that any circuit can read and write.
That efficiency argument is why a shared cognitive space is plausible — and it's
exactly the structure J-Lens claims to expose.

**The evidence, graded.** `[Move 6, with Move 5 boring hypotheses throughout]`

- The abstract J-Lens readouts are **interesting** but not load-bearing on their
  own — a readout being decodable doesn't prove the model *uses* it.
- The multi-hop causal intervention is **more compelling**: patching "France"
  and getting "Paris" out. `[Move 5]` But the boring hypothesis first — the
  "Paris" unembedding might just be ≈ "France" + a generic capital-city
  direction, in which case the model never did a second hop; it combined
  Frenchness and capital-ness in parallel. What makes this non-boring is the
  result (fig. 15 in the paper) that the boring version can't produce. That
  result is doing the real work.
- The bandit-prompt intervention is **particularly compelling** — the hardest of
  the results to explain away.
- The multilingual result is **mildly interesting**; I wouldn't hang anything on
  it.
- The eval-awareness sampling intervention I find **less reliable** — there's
  enough confound that I read it as suggestive at most.
- The counterfactual-reflection training is **cool, but not much evidence** for
  the central claim; nice idea, thin support.

Notice this is the opposite of uniform praise. Two results are carrying the
scientific claim; the rest are texture.

**Practical read.** `[Move 8]` I'd expect J-Lens to be useful for **hypothesis
generation** — run it early in an investigation, and treat an interesting signal
as weak evidence worth chasing. I would *not* trust it for **validation**: expect
false positives, and expect it to miss things it should have flagged. So the
honest framing is "a tool for the first pass, not the verdict."

**Where I hold back.** `[Move 7]` On whether this is genuinely a "global
workspace" in the cognitive-science sense — that's the claim I'm least qualified
to judge, and I'd rather not launder a strong opinion I don't have. But because
I've been specific on the other three claims, that reticence should read as
calibration, not evasion.

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
- The grades span a real spectrum — "particularly compelling" and "cool but not
  much evidence" live in the same review.
- Utility is bounded to a use-context and a failure mode, never left as "useful."
- The competence boundary is drawn explicitly, and it makes the committed claims
  *more* credible, not less.
