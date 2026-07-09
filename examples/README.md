# The everyday-memo demo

The validation against Neel Nanda proves the ceiling. This folder shows the
floor: what the skill does to a document anyone might actually have to judge
on a Tuesday.

## Setup

- **Input:** [demo-input.md](demo-input.md), a fictional but realistic VP memo
  proposing to make code review optional. It is written to be seductive: four
  real-sounding statistics, each with a flaw that is obvious only once
  someone says it out loud.
- **Two runs, same model (Claude Opus 4.8), same instruction** ("write your
  honest assessment, be direct and opinionated, ~500-700 words"):
  - [output-no-skill.md](output-no-skill.md): the memo only, with the skill
    removed from the agent's environment.
  - [output-with-skill.md](output-with-skill.md): the memo plus the three
    skill files from [`../skills/neel-nanda-analysis/`](../skills/neel-nanda-analysis/),
    with the usual minimal framing ("read these methodology files, apply the
    methodology").
- Outputs are unedited. n = 1 per condition; this is a demo, not a study. For
  the controlled version of the with/without comparison, see
  [../validation/structural-audit.md](../validation/structural-audit.md).
- One footnote for the skeptics: our first attempt at the no-skill run had the
  skill still *installed* in the agent's environment, and the agent
  auto-triggered it from the task wording alone (which is what a skill
  description is for). That run was discarded, the skill was removed from the
  environment, and the baseline here is the clean re-run.

## The prompts, verbatim

**No skill:**

```
Read this internal engineering memo: [demo-input.md]

Then write your honest assessment of it: what you make of the proposal,
whether the argument holds, and what you would tell the VP before they ship
it. Be direct and opinionated, not a neutral summarizer. Around 500-700 words.
```

**With skill:** identical, prefixed with:

```
First, read these three methodology files fully and internalize the judgment
structure they describe: [SKILL.md, phrase-bank.md, worked-examples.md]
[...] Apply the methodology to write your honest assessment of the memo [...]
```

## What to look at

Honesty first: **both runs caught the memo's statistical flaws.** The
selection bias in the bypass comparison, reverts being the wrong metric, the
82%-nits stat arguing for a linter rather than for removing review, the 12%
figure being large rather than small: the base model finds all of that on its
own. If the skill claimed to add insight, this demo would refute it.

What differs is the architecture of the judgment. Two places to see it:

**How they open.**

> **No skill:** "This memo diagnoses a real problem and then reaches for the
> wrong cure with data that doesn't support it."
>
> **With skill:** "My prior, before the numbers: review's value was never in
> the average PR. It lives in the tail [...] Under that model I would expect
> exactly what the memo reports [...] Same data, opposite conclusion, which
> means the data is not discriminating between us. That alone should stop the
> rollout."

One opens with a verdict. The other opens with a model of the world that says
what evidence *would* settle the question, and then shows the memo's evidence
can't. A VP can argue with a verdict; arguing with a discrimination argument
requires bringing better data, which is the point.

**How they close.**

> **No skill:** "If you still want to test the hypothesis, run it as a real
> experiment: one or two volunteer teams, one quarter, shadow the bot against
> humans first [...]"
>
> **With skill:** "Where I hold back: whether 'opt-in' quietly decays into
> 'never' under deadline pressure [...] that is outside what I can settle from
> the memo [...] On the statistics and the metric validity, though, I am not
> hedging: the case as written does not support the change."

Same recommendation underneath. Only one of the two tells you which parts of
it are certain, which are hunches, and where its authority ends. That is the
difference between advice you can follow and advice you can *audit*.
