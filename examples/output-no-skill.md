# Demo output: no skill

Model: Claude Opus 4.8, run with the skill removed from the environment.
Prompt: see [README.md](README.md). Unedited.

---

This memo diagnoses a real problem and then reaches for the wrong cure with data that doesn't support it. Review latency is a legitimate, worth-fixing complaint. But the argument that mandatory review is the bottleneck to remove is built on a confounded comparison, the wrong outcome metric, and a rollout plan that should not survive contact with a serious room.

Start with the load-bearing evidence: the bypass cohort. Eighty-seven emergency-bypass PRs are not a random sample of your engineering output — they are the PRs someone hand-picked as urgent and safe enough to skip review, likely small hotfixes from senior engineers already under incident scrutiny. Comparing their merge speed and revert rate to 1,204 normal PRs is textbook selection bias. It tells you "the changes we judged safe to skip were safe to skip," which is a tautology, not a finding. Both point 1 (3.2 days) and point 3 (revert parity) inherit this flaw and can't be rescued.

The revert metric is also the wrong yardstick. Reverts catch catastrophic, obvious breakage. Almost nothing review actually protects against — security holes that don't crash anything, design decisions that rot over six months, missing edge cases, knowledge that would otherwise silo — shows up as a revert. So "revert rates are indistinguishable" is not evidence review is worthless; it's evidence you measured the one thing review isn't primarily for. And read the parity the other way: if risky normal PRs and hand-picked-safe bypass PRs land at the same low revert rate, review may be doing its job — pulling risky changes down to the safe baseline. This data literally cannot distinguish "review is useless" from "review is working."

The "82% nits" stat argues against the proposal, not for it. If most comments are lint-able style, the fix is to put those rules in the linter and free humans for the other 18%. That 18% — never quantified here — is where all the value lives, and the memo quietly drops it. Same with the survey framing: "only 12% said review caught a serious bug in their code last quarter" is spun as small. At 200 engineers that's roughly two dozen serious bugs caught in a quarter. And you're asking authors whether their own code had caught bugs — the worst-positioned people to know, since the reviewer is the one who saw what they missed.

The "15% of engineering time" number is unfounded. Calendar latency (a PR waiting in a queue) is not engineer effort — the author is working on something else while it waits. Converting wait-time into recovered headcount is a sleight of hand. And the LLM bot, the thing meant to preserve the "review signal," has zero validation here: no precision/recall, no comparison to human reviewers, introduced as a done deal. The prose — "tired humans," "legacy ritual," "teams like ours report happier engineers" (which teams? survivorship — the ones that imploded aren't blogging) — is carrying weight the numbers can't.

What I'd tell the VP before they ship it:

Don't ship this as written, and absolutely kill "all product teams next sprint unless there are objections." Opt-out rollout of a hard-to-reverse cultural change, with no pilot and no guardrail metrics, is the biggest red flag in the document.

Split the proposal. The low-risk half is good: enforce the 82% nits in the linter and add the LLM bot as an assist. Do that now — it removes most of the latency people are actually complaining about. The risky half — making human review optional — should be gated behind evidence.

Fix latency directly first: review SLAs (first pass within N hours), PR size limits, auto-assignment and reviewer load-balancing. Most review delay is queueing, not reviewing.

If you still want to test the hypothesis, run it as a real experiment: one or two volunteer teams, one quarter, shadow the bot against humans first, pre-register metrics that capture what review protects (escaped defects to prod, security findings, incident rate, onboarding time — not just reverts and merge speed), with a rollback trigger. Keep security-sensitive and compliance-scoped paths mandatory regardless.
