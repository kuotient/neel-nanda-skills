# Proposal: Make human code review optional for product teams

**Author: VP Engineering · Status: seeking sign-off this week**

Our shipping velocity is the #1 complaint in the last three engineering
surveys, and the data points at one bottleneck: mandatory code review.

**The data:**

1. PRs that go through review merge **3.2 days slower** on average than PRs
   merged under our emergency-bypass flag (n = 1,204 vs 87, last two quarters).
2. We sampled 400 review threads: **82% of comments are style or naming nits**,
   which our linter could enforce automatically.
3. Revert rates are **statistically indistinguishable** between reviewed PRs
   (1.9%) and bypass PRs (2.1%).
4. In the survey, **71% of engineers named review latency** as their top
   friction; only 12% said review had caught a serious bug in their code in
   the last quarter.

**The proposal:** Make review opt-in. Authors may request review; otherwise CI
(tests, linter, type checks, a new LLM review bot) is the merge gate. Staff
engineers estimate this recovers **~15% of engineering time**, and the LLM bot
gives us the review signal without the latency.

Modern CI plus LLM review is simply better positioned than tired humans to
catch the issues that matter. Teams like ours that have removed review
friction report faster onboarding and happier engineers. Review made sense
when tooling was weak; it is now a legacy ritual we keep out of habit.

We plan to enable this for all product teams next sprint unless there are
objections.
