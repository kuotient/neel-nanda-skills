# neel-nanda-skills

A Claude skill that makes your agent judge things the way Neel Nanda judges
papers. Not his writing style, his reasoning structure.

> Not affiliated with or endorsed by Neel Nanda. It encodes the reasoning
> structure of his
> [public commentary](https://transformer-circuits.pub/2026/workspace/index.html)
> on an Anthropic paper; the name is descriptive, like "Feynman technique."
> · [한국어](README.ko.md)

## Why I made this

Anthropic's "Global Workspace" paper shipped with an invited review by Neel
Nanda, and his commentary stuck with me. It wasn't just right, it was *built*:
he broke the paper into separate claims, said how much he believed each one,
laid out his own prior before touching the evidence, hunted the boring
explanation behind every result, and named the parts he wasn't qualified to
judge.

No matter how detailed a prompt I gave it, my own Claude had never produced
commentary that well-organized. It would write something fluent and plausible,
then quietly agree with whatever it had just read. So I studied what made his
commentary work, wrote it down as eight moves, and made it a skill.

## Install

**Claude Code (plugin)**

```
/plugin marketplace add kuotient/neel-nanda-skills
/plugin install neel-nanda-skills@neel-nanda-skills
```

**Claude Code (manual)**

```
git clone https://github.com/kuotient/neel-nanda-skills.git
cp -r neel-nanda-skills/skills/neel-nanda-analysis ~/.claude/skills/
```

**claude.ai / Desktop**: Settings → Capabilities → Skills → upload the
`skills/neel-nanda-analysis` folder (zipped).

It triggers on any judgment request ("review this", "what do you make of X",
"poke holes in this"), in any language.

## The eight moves

| # | Move | What it prevents |
|---|------|------------------|
| 1 | Decompose into independent claims; split the headline claim into its modest and ambitious versions | one verdict on everything |
| 2 | Rate importance / confidence / standing separately | "I liked it" |
| 3 | State your first-principles prior *before* the evidence | "the paper says so, so I believe it" |
| 4 | Build intuition with a worked example | slippery abstraction |
| 5 | Hunt the boring hypothesis behind every flattering result | motivated reasoning |
| 6 | Grade evidence on a spectrum; say which results are cruxy | uniform praise |
| 7 | Mark the edge of your competence *and* your coverage | fake omniscience |
| 8 | Bound the practical utility: for-what / where-it-fails | "this changes everything" |

The common shape of every judgment: **commit, then bound.** Say what you
believe, then immediately say how far.

## The difference

Same model, same memo (a plausible "let's make code review optional" pitch with
four loaded stats), with and without the skill. Both catch the bad stats. Watch
how they *open*:

> **Without the skill:** "This memo diagnoses a real problem and then reaches
> for the wrong cure with data that doesn't support it."
>
> **With the skill:** "My prior, before the numbers: review's value was never
> in the average PR. It lives in the tail [...] Same data, opposite conclusion,
> which means the data is not discriminating between us. That alone should stop
> the rollout."

One hands you a verdict. The other hands you a model of the world, then shows
the memo's evidence can't settle it. Full memo and both outputs:
[examples/](examples/).

## How I know it works

I tested it the hard way: gave a blind agent the skill and the same paper, with
Neel's review nowhere in its context, and compared. It independently landed
where he did, right down to the verdict:

> **Neel:** "I expect it to largely be useful as a hypothesis generation tool
> [...] I expect it to have many false positives."
>
> **Blind agent:** "use it to *find*, never to *clear*."

I also ran a no-skill baseline, because the honest question is whether the skill
did anything or the model was already good. It was already good. So a blind
auditor scored both against 8 structural checks: **6/8 without, 7/8 with.** The
two that flip are stating a prior before the evidence and declining a verdict
for lack of standing.

Every run (including two I disqualified), the audit, and the reproduce steps:
[validation/](validation/). Honest ceiling: what nothing reproduced is Neel
replicating the paper on another model, plus a decade of instinct. The
structure transfers; the expertise doesn't.

## Provenance & attribution

Built by studying [Neel Nanda](https://www.neelnanda.io/)'s invited external
commentary on
[*Verbalizable Representations Form a Global Workspace in Language Models*](https://transformer-circuits.pub/2026/workspace/index.html)
(Anthropic, Transformer Circuits, 2026). He has no affiliation with this repo
and has not endorsed it; his original commentary is better than any
reconstruction of it. Quotes here are brief fragments for comparison, linked to
the source.

Inspired in spirit by
[andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills):
the idea that a named researcher's working discipline can be packaged as a
skill. The content is independent.

## License

MIT; see [LICENSE](LICENSE).
