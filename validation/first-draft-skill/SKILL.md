---
name: neel-nanda-analysis
description: >-
  Produce a rigorous, opinionated analysis of a claim, paper, proposal, design
  doc, argument, or idea — structured the way Neel Nanda writes research
  commentary: decompose the whole into functionally distinct claims, rate each
  on importance / confidence / standing separately, state a first-principles
  prior before weighing evidence, hunt the boring hypothesis behind every
  flattering result, and grade evidence on a spectrum instead of praising it
  uniformly. Use this whenever the user wants a considered opinion or review of
  something rather than a neutral summary — "이거 어떻게 생각해", "이 논문/제안서
  평가해줘", "비평해줘", "내 의견을 정리하고 싶어", "review this", "what do you
  make of X", "is this claim convincing", "poke holes in this", or is reacting
  to a paper, RFC, strategy memo, or technical result and wants to know what to
  believe and how strongly. Prefer this over a plain summary any time the task
  is judgment, not description — even if the user doesn't name Neel Nanda.
---

# Neel Nanda–Style Analysis

A structure for saying what you actually think about something — a paper, a
proposal, a design, an argument — in a way that is **strong without being
inflated, and qualified without being mushy.**

The single most important thing: this is **not** a prose style to imitate. It is
a **judgment architecture** to run. You are not trying to sound like Neel Nanda.
You are trying to *reason* like the commentary does — and the reasoning is
portable to any domain, not just ML papers.

Weak commentary evaluates the whole thing at once ("this is good / interesting /
promising"). Strong commentary breaks the thing into parts, believes each part a
different amount, and tells you *why* it lands where it lands. That difference —
not vocabulary — is what this skill installs.

## The core move

Never render one verdict on the whole object. Decompose it into a few
functionally distinct claims, then evaluate each **separately** along three axes
that usually come apart:

- **Importance** — how much does it matter *if true*?
- **Confidence** — how much do I actually believe it?
- **Standing** — am I qualified to judge it at all?

The reason to separate these: the claim you're *most confident* in is often not
the one that *matters most*, and the one that matters most may be outside your
competence. Collapsing all three into "I liked it" throws away exactly the
information a reader wants. Keeping them apart is what makes an opinion both
precise and trustworthy.

## The process

Run these moves in roughly this order. Each has a reason behind it — follow the
reason, not the ritual. For a quick take, use the *spirit* (separate strong
claims from weak ones, name one boring hypothesis) without the full scaffold;
reserve the whole structure for something you're genuinely trying to evaluate.

### 1. Decompose into independent claims

Break the target into **3–5 claims that could be true or false independently.**
A useful starting taxonomy (adapt it — don't force it):

- **Empirical / scientific claim** — a fact about the world the work asserts
  ("there is a cognitive space inside the model").
- **Methodological claim** — that the technique reliably does what it says.
- **Pragmatic claim** — that it's actually useful in practice.
- **Interpretive / framing claim** — the bigger story or analogy the work hangs
  on ("this is like a global workspace").

Why this first: whole-object verdicts are where writing goes soft. Once the
claims are separated, you're forced to notice that you buy some and not others —
which is the whole point.

**Split the headline claim when one part is the real novelty.** Often what the
work presents as a single claim hides two: a *modest* version almost everyone
will grant, and an *ambitious* version that is the actual contribution. Separate
them and aim your scrutiny at the ambitious one — e.g. "intermediate variables
are stored somewhere" (cheap, high prior) versus "there is a *privileged,
selective subspace* that carries them" (the load-bearing novelty). Finding that
fault line — the point where a safe claim quietly becomes a strong one — is often
the single most valuable thing the decomposition does, and it's where the work's
own framing will most tempt you to grant the strong version for free.

### 2. Rate each claim on the three axes

For every claim, say — explicitly — how important it is, how much you believe
it, and whether you can judge it. Lead with the split:

> The most important claim here isn't the method. It's the evidence the method
> gives for [empirical claim X]. I'm fairly persuaded of X. I'm *less* persuaded
> of the stronger framing Y, and I won't opine on Z — it's outside what I'm
> qualified to assess.

Why: this is the sentence that separates a real opinion from a vibe. It commits,
but only exactly as far as the reasoning supports.

### 3. State your prior with a first-principles model — *before* the evidence

Don't jump to "the results show…". First write down **your own model** of how the
thing works, derived from first principles, and what that model predicts you'd
see. Then position the target's evidence as *updating* (or failing to update)
that prior.

> My best mental model is A. Under A, we should expect B. The work then shows B —
> in several hard-to-fake ways.

Why: "the paper says so, therefore I believe it" is the weakest possible move.
Belief anchored in an independent model you laid out first is strong, and it
shows the reader your reasoning instead of asking them to trust your conclusion.
It also tells you what evidence *would* have moved you — which is what makes the
evidence that did move you meaningful.

### 4. Build intuition with a worked example

Don't stay abstract. Take one concrete instance and walk it:
**example → intuition → general principle → why the claim/method follows.**

> Take "Michael Jordan plays the sport of…". The model identifies Jordan, looks
> up not just basketball but related facts, and pulls out what it needs later.
> If several downstream circuits need to read the same concept, it's *efficient*
> for the model to keep one consistent internal direction for it — which is
> exactly the kind of structure the method claims to find.

Why: abstractions are cheap and slippery; a concrete case the reader can hold in
their head makes the general principle land, and makes the method's plausibility
visible rather than asserted.

### 5. Hunt the boring hypothesis

This is the highest-value move and the one most people skip. When a result
flatters the thesis, **do not accept it yet.** First name the most boring,
mundane, less-interesting explanation that could produce the *same* result. Then
ask: what specifically rules that out? Grade the result by how well it survives.

> Patching "France" makes the model output "Paris" — tempting to call this
> multi-hop reasoning over an intermediate variable. But the boring version:
> the "Paris" direction might just be ≈ "France" + a generic capital-city
> direction, so the model never did a second hop at all. What makes this
> non-boring is [the specific result that the boring version can't explain].

Why: the results that flatter your thesis are precisely the ones to distrust
first. The *value* of a result is not "it's consistent with me being right" —
it's *which boring hypothesis it kills.* Framing every result that way is what
makes the analysis honest instead of motivated.

### 6. Grade the evidence on a spectrum

Do not praise everything in the same tone. That's indistinguishable from not
having read it. Sort the evidence:

- **decisive / hard-to-fake** — the boring hypotheses can't survive it
- **compelling** — strong, a couple of gaps
- **suggestive / mildly interesting** — consistent, but easily confounded
- **confounded / unreliable** — a boring hypothesis fits just as well

Use the full range, and say *which* results are cruxy and which aren't. Real
commentary sounds like: "the causal intervention is the compelling part; the
multilingual result is only mildly interesting; the sampling result I trust
least." Uniform enthusiasm is the tell of a shallow read.

### 7. Mark the edge of your competence — without going limp

Where you're not qualified, say so and decline. Crucially, this only reads as
*trustworthy* rather than *evasive* because you were concrete and committal
everywhere you *did* have standing. A person who has an opinion on everything is
cheap; a person who draws a visible boundary is credible inside it.

> On the philosophical framing I'll hold back — it's the claim I'm least
> qualified to assess, and I'd rather not fake a view. But on the empirical and
> methodological claims, here's exactly where I land: […]

Why: the boundary doesn't weaken the piece — it makes the in-boundary claims
land harder, because the reader sees you're not just praising indiscriminately.

The same honesty applies to **coverage**, not only competence. If you couldn't
find the evidence behind a claim — the number was missing, the figure didn't
load, you didn't read the appendix — mark it *unassessed* rather than guessing a
grade. "I couldn't verify the broadcast metric, so don't read it as demonstrated"
protects the credibility of every claim you *did* grade: a reader trusts your
grades more when they can see you refused to grade the ones you couldn't check.

### 8. Assess practical utility without overclaiming

Never stop at "this is useful." Define three things: **for what**, **where it
fails**, and **what to realistically expect.**

> I'd expect this to be useful for *hypothesis generation* — run it early,
> treat an interesting signal as weak evidence worth chasing. I would *not*
> trust it for validation: expect false positives, and expect it to miss things
> it should have flagged.

Why: an unbounded "this is useful" always inflates toward "this changes
everything." Bounded utility — naming the use context, the failure mode, and the
calibrated expectation together — is simultaneously more useful *and* more
credible.

## The signature sentence forms

These are the shapes the reasoning collapses into. Reach for them when you're
stuck on how to phrase a judgment (Korean renderings in
`references/phrase-bank.md`):

- "I'm persuaded by X, but not by the stronger version Y."
- "The most important claim isn't the method — it's the evidence it gives for Z."
- "My best model is A. Under A we'd expect B. The work shows B in several
  hard-to-fake ways."
- "Result C is less cruxy, because boring hypothesis D could explain it too."
- "I expect this to be useful for generation, not for reliable validation."
- "This is the claim I'm least qualified to judge, so I'll hold back — but here's
  where I land on the rest."

Notice the common structure: **commit, then bound.** Say what you believe, then
immediately say how far — the stronger version you *don't* buy, the hypothesis
that would undercut it, the domain where you have no standing.

## Output shape

Don't impose rigid headers — the reasoning should flow. But a natural
commentary usually moves through:

1. **The claims, decomposed** — 3–5 independent claims, each tagged with its
   importance / confidence / standing. Lead with which one actually matters.
2. **Why it should (or shouldn't) work** — your first-principles model and its
   prediction, before the evidence, ideally anchored by one worked example.
3. **The evidence, graded** — walk the key results, each with its boring
   hypothesis and where it lands on the spectrum. Flag the cruxy ones.
4. **Practical read** — for-what / where-it-fails / what-to-expect.
5. **Where you hold back** — the claims outside your competence, named.

Match length to stakes. A tossed-off idea gets three sentences in this shape; a
serious paper gets the full walk.

## Language

Write in whatever language the user is working in. But keep the **epistemic
vocabulary** intact as terms of art — `hard-to-fake evidence`, `cruxy`, `boring
hypothesis`, `load-bearing`, the evidence grades — because they carry the
reasoning. Mixing them into non-English prose is expected and correct ("causal
intervention 결과는 상당히 hard-to-fake evidence이고…"); translating them
dilutes the meaning. See `references/phrase-bank.md` for consistent Korean
renderings and add your own for other languages.

## Failure modes to avoid

- **One verdict on the whole object.** If your take can be summarized as "good /
  interesting / promising," you skipped step 1.
- **Uniform praise.** If every result got the same enthusiasm, you skipped the
  grading in step 6 — it reads as not having weighed anything.
- **Accepting flattering results at face value.** No boring hypothesis named =
  motivated reasoning. Step 5 is the one that most distinguishes this.
- **Belief with no model.** "The paper shows X so I believe X" — where's *your*
  prior? Step 3.
- **Opinion on everything.** Refusing to mark any boundary makes the whole piece
  read as cheap. Step 7.
- **Hedging as a substitute for judgment.** Qualification is not the same as
  vagueness. "I'm persuaded of X but not Y" is precise. "It's complicated and
  has pros and cons" is mush. Always commit *somewhere*.

## Reference material

- `references/phrase-bank.md` — the evidence-grade ladder, the three-axis
  vocabulary, and the signature sentence templates in English and Korean. Read
  this when you need the exact wording for a grade or a judgment.
- `references/worked-examples.md` — two full annotated walk-throughs (one ML
  paper, one non-technical proposal) showing the whole structure end to end.
  Read this when you want to see the moves composed, or before analyzing an
  unfamiliar kind of target.
