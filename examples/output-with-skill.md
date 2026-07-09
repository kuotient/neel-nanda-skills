# Demo output: with the skill

Model: Claude Opus 4.8. Prompt: see [README.md](README.md). Unedited.

---

The memo renders one verdict ("review is a legacy ritual, kill it"), so the first move is to break it into claims that can fail independently:

1. *Empirical*: mandatory review adds ~3.2 days of latency.
2. *Empirical*: review provides little safety value (revert parity, 82% nits, 12% bug-catch).
3. *Pragmatic/methodological*: CI plus an LLM bot can carry whatever safety function review had.
4. *Framing*: human review is obsolete tooling from a weak-tooling era.
5. *Pragmatic*: this recovers ~15% of engineering time.

The load-bearing claim is not the latency (1); nobody disputes review takes time. It is (2)+(3): that the safety you would give up is both small and replaceable. Everything else is decoration on that, so that is where the scrutiny goes, and it is where the argument is weakest.

My prior, before the numbers: review's value was never in the average PR. It lives in the tail: the occasional missing authz check, the subtle data-corruption path, the bad abstraction that costs six months. A healthy review process should produce mostly nits on the surface and rare, high-value catches underneath. Under that model I would expect exactly what the memo reports: most comments are nits, most PRs are fine, reverts look similar. The memo reads those as review being useless. I read the same data as consistent with review working. Same data, opposite conclusion, which means the data is not discriminating between us. That alone should stop the rollout.

Now the boring hypotheses, because every number here has one and none are ruled out.

The 3.2-day gap (n=1,204 vs 87): the bypass flag is not random assignment, it is an emergency lane. Bypass PRs are self-selected to be urgent, small, senior-authored, high-attention, the ones that would merge fast regardless. This is selection bias, not a natural experiment, and it confounds the headline number outright. Grade: confounded.

Revert parity (1.9% vs 2.1%): the same selection bias applies, and n=87 cannot distinguish those rates anyway. "Statistically indistinguishable" here means underpowered, not equal (absence of evidence, not evidence of absence). Worse, reverts are the wrong metric. They catch loud, obvious breakage; review prevents the quiet failures that never trigger a revert. You measured what review does not target and correctly found nothing. Grade: confounded, and near-irrelevant to the claim it is asked to support.

82% nits: fully compatible with "the other 18% is where all the value is." One caught authz bug a quarter can justify the whole process. And the honest implication of "most comments are automatable nits" is automate the nits, which removes the friction without removing the human. The data argues for the cheaper intervention, not this one.

The 12%: "12% of engineers had a serious bug caught in their own code last quarter" is framed as small. It is large. That is a high catch rate, and it is the strongest pro-review number in the memo, misread as anti.

Claim (3), the LLM bot, is vaporware sitting in the load-bearing slot: no eval, no agreement number, no baseline, it does not exist yet. And LLM review is weakest at exactly the substantive architectural and security judgment (the 18%) and strongest at nits the linter already covers. The substitution is upside-down.

What I would tell the VP: do not ship this next sprint, and not big-bang. Your two best numbers are both eaten by the same selection bias, your safety metric measures the wrong failures, and your replacement is unvalidated. The data you have supports a smaller move: kill the 82% nits with better linters and formatters, add review SLAs, push smaller diffs. Capture the latency win, keep the net. If you still want to test removal, run it as a real experiment: a few pilot teams, a control group, and a rollback trigger defined on silent failures (security escapes, sev counts, not reverts), over a quarter. A blanket rollout does not just risk the tail; it destroys your ability to ever measure it.

Where I hold back: whether "opt-in" quietly decays into "never" under deadline pressure is a behavioral and org prediction. My hunch is yes, defaults are sticky, but that is outside what I can settle from the memo, and I cannot price your real risk tolerance without your incident history. On the statistics and the metric validity, though, I am not hedging: the case as written does not support the change.
