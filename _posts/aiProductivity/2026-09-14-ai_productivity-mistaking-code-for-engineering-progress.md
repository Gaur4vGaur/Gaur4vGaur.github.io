---
title: Mistaking Code Production for Engineering Progress: AI Productivity Myths Part 1
description: "Lines of code and PR counts went up after AI adoption — so did production incidents. Why code volume is a poor proxy for engineering progress, and what GitClear's data on refactoring decline reveals instead."
tags: ["gen-ai", "ai-productivity-myths", "software-engineering"]
category: ["ai"]
date: 2026-09-14
permalink: '2026/ai/ai_productivity-mistaking-code-for-engineering-progress/'
image:
  path: /assets/blog_assets/img/ai/2026-09-14-ai_productivity-mistaking-code-for-engineering-progress/coverImage.png
  width: 800
  height: 500
---

## AI Productivity Myths: Lessons from the Real World — Part 1

*Why lines of code generated is almost meaningless as a measure of AI-assisted development.*

---

A few months ago, one of our teams celebrated a milestone in their quarterly review. AI adoption was up. The productivity dashboard showed developers were generating 40% more code per sprint. The engineering manager presented it as an unqualified win.

Three weeks later I was on a call, this time about a production issue. Errors were coming back in three different formats depending on which endpoint you hit. Our alerting was blind to a category of failure it had always caught before.

We had a centralised exception handler. One place. It logged context, mapped to the right HTTP status, and pushed alerts to our observability stack. Over the previous few weeks, AI-assisted PRs had started introducing their own try-catch blocks inline. Each one caught exceptions locally, logged in a slightly different format, returned a slightly different error shape. Some swallowed the exception instead of letting it propagate up to the handler that would have alerted us.

Every one of those PRs was correct. Every one passed review, including the reviews I did myself. We were checking for correctness. Nobody was checking for consistency. Consistency is not the kind of thing that shows up in a diff.

Cleaning it up took most of a sprint. And that productivity dashboard? It counted the original generation and the cleanup as output. Twice the code, twice the "productivity", for a net loss of engineering time.

The dashboard going up while the system got worse underneath it.

This is the mistake I keep seeing: confusing code production with engineering progress. The chart goes up. Everyone smiles. And nobody asks what the chart actually measures.

---

## The LOC Trap, Reloaded

Fred Brooks called this out in [*The Mythical Man-Month*](https://en.wikipedia.org/wiki/The_Mythical_Man-Month) decades ago — measuring programming productivity by lines of code is nonsensical. The industry agreed. Then somehow forgot. Why did an industry that agreed LOC was nonsense rebuild the exact same dashboard the moment AI arrived?

Here we are, dressing up that same flawed metric with AI branding and presenting it to boards. When a team lead reports that AI tools helped produce 40% more code, the follow-up I want to hear is: *"Did we actually need 40% more code?"* Usually the answer is no. What we needed was the same outcomes with less effort. Effort in software lives overwhelmingly outside the act of typing — but I will come back to that.

There is a difference this time and it is worth naming. Hardly anyone defends raw line counts out loud any more. The dashboards moved on, to merged pull requests, agent tasks completed, suggestions accepted. Same instinct in a more respectable unit. Counting the artefacts of work and reporting the count as progress.

Plenty of places have not moved on at all, mine included, which is how a quarterly review ends up presenting 40% more code per sprint as a result. So read LOC in this post as whatever your dashboard happens to count. The argument does not change with the unit.

---

## Why Senior Engineers Delete Code

Here is a pattern you will recognise if you have led engineering teams for any length of time. Your best engineers — the ones you rely on for the hardest problems — often produce fewer lines of code than anyone else on the team. Some of their most impactful weeks result in *negative* line counts.

That is not laziness. That is expertise. I have not fully worked out why it takes years to develop the instinct for deletion over addition, but it does.

GitClear has been measuring this rather than speculating about it. Their [2026 analysis](https://gitkraken.gitclear.com/the_ai_code_quality_maintainability_gap) covers 623 million changes from 2023 to 2026, and the figure that stopped me had nothing to do with volume. Moved code, their proxy for refactoring, dropped from 21% of all changes in 2022 to 3.8% by the middle of this year. Duplicated blocks are up 81% across the same window. Cross-file function calls, which is roughly what reuse looks like in a diff, are down 35%.

Those three together describe a codebase that has stopped being rearranged. Work goes in. Very little gets moved, merged, or deleted. When someone takes 2,000 lines of tangled logic and replaces it with 200 clean ones, that looks like a loss on any volume metric. It is an enormous win for the system, and it is exactly the activity that has gone quiet.

One correction I owe, since I quoted the earlier version of this research at people for the better part of a year. GitClear's [2024 report](https://www.gitclear.com/coding_on_copilot_data_shows_ais_downward_pressure_on_code_quality) predicted two-week code churn would double in the AI era. It did not double. It went up 15%. The headline projection was too aggressive, and the part almost nobody quoted, refactoring falling off a cliff, turned out worse than predicted.

Senior engineers get this intuitively. Every line of code is a liability. Each one has to be read, understood, tested, maintained, and eventually migrated or deleted. The best solution often makes code disappear — a well-chosen abstraction that kills duplication, a config change that eliminates a custom implementation, or sometimes just a conversation with product that removes a requirement entirely.

Now think about what AI coding metrics would say about this. An engineer spends a day understanding a system, realises three services can collapse into one, and deletes 4,000 lines. By every AI productivity metric in use today, that engineer had a terrible day. In reality, they may have saved the organisation months of future pain.

[Gergely Orosz tells a revealing story](https://newsletter.pragmaticengineer.com/p/measuring-developer-productivity) about what happens when you optimise for the wrong signal: when Uber introduced diff-count metrics, engineers started creating more, smaller changes to *look* productive — flooding CI systems and driving up costs. The metric improved. The engineering got worse. We are setting ourselves up for the same trap with AI-generated LOC.

![Four GitClear stats: moved code down, duplication and error-masking up](/assets\blog_assets\img\ai\2026-09-14-ai_productivity-mistaking-code-for-engineering-progress\theRearrangementStats.jpg)

---

## AI's Tendency Toward Verbose Implementations

This gets worse when you look at what AI coding tools actually excel at: producing plausible code quickly. And "plausible code produced quickly" has a built-in bias toward verbosity.

I want to be careful here. Sometimes more code is genuinely the right call. Explicit beats implicit. A verbose but readable implementation can be better than a clever one-liner that nobody understands at 3am when they are half-awake and production is on fire. I am not arguing for code golf.

But AI-generated verbosity is a specific kind of bad. It is not *chosen* verbosity for clarity. It is *default* verbosity from ignorance of context. That distinction matters more than I initially gave it credit for.

Ask an AI assistant to implement a feature and you will get a complete, working solution. It will also be longer than what an experienced developer would write — not because it is wrong, but because it optimises for correctness and completeness in isolation. It does not know your codebase already has a utility that does exactly this. It does not realise the framework provides a one-liner if you just structure the problem slightly differently. It cannot distinguish between "I should be explicit here for readability" and "I am reinventing something that already exists three directories over."

The handler drift I opened with is the cleanest example I have of it. The AI did exactly what it was asked, every single time. Each PR added code, each one passed review on its own terms, and nothing was wrong inside any of them. What we lost lived across them. One handler, which is what gave us one error shape, which is what gave our alerting something to fire on. No single diff broke that. All of them together did.

GitClear tracks a signal for the specific failure mode buried in there: error-masking constructs, the empty catch and the quietly swallowed exception, up 47% since 2023. Our inline handlers were precisely that. I would like to think we were an unlucky outlier. The data says we were ordinary.

I am still not sure how you review for a property that is not visible in the file in front of you.

---

## Complexity as the Hidden Cost

This is where it really hurts.

Code volume is not a perfect proxy for system complexity — I acknowledged that above. But it is a *directional* one, and in aggregate it holds. When your codebase grows by 30-40% in a quarter without a corresponding growth in functionality, complexity is almost certainly growing with it. System complexity is the single biggest thing that determines how fast your team can move over time. I have not found a way around that fact in eighteen years of doing this work.

Every line of code carries ongoing costs that nobody puts on a dashboard:

- **Cognitive load** — someone has to understand it to work anywhere nearby
- **Test burden** — it needs coverage or it becomes a ticking risk
- **Review overhead** — someone has to read and approve any changes to it
- **Dependency surface** — it may pull in libraries that need constant updating
- **Migration cost** — it all has to be dealt with during platform changes

When AI tools grow your code volume by 30-40%, every one of these costs grows with it. The productivity gain at the moment of writing is real — I am not denying that. But it can be entirely eaten up by the downstream cost of maintaining a bigger, more complex system.

Then there is the study I keep coming back to, and the update to it that I nearly missed.

In mid-2025, [METR](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/) found that experienced developers using AI coding tools took 19% *longer* on real-world tasks. Longer. The setup was specific: 16 open-source contributors working in their own repositories, code they had lived in for years, taking on 246 real issues that averaged about two hours each, with Cursor Pro and Claude. Bug fixes, features, refactors. Ordinary work.

That number went everywhere. Mine included, more than once.

In February 2026, METR [published an update](https://metr.org/blog/2026-02-24-uplift-update/) that takes a fair amount of it back. They are redesigning the experiment, and the reasons are not flattering to the original. Developers who would no longer work without AI declined to take part at all. Somewhere between 30% and 50% of participants avoided submitting exactly the tasks where they expected to want AI. The pay rate for the follow-up work dropped from $150 an hour to $50, which made recruitment worse again. Their own summary is that the newer data amounts to very weak evidence in either direction, and that developers are probably more sped up now, in early 2026, than the early-2025 estimate suggested.

So the 19% was never a fact about AI-assisted development. It was a measurement of sixteen people in one setting, and the people who ran it now think it read low.

What survives is the part that unsettled me in the first place. Those developers *believed* they were 20% faster. Whatever the true effect was, it was not the effect they perceived, and not one of them could feel the gap while it was happening. Reviewing, correcting and integrating suggestions consumed time that none of them accounted for. Selection bias moved the headline number around. It does not explain away a room full of experienced engineers being wrong about their own week.

I could never square that finding with my own experience, because I do feel faster on certain tasks. The update moves me off the fence, slightly. Maybe I was not fooling myself. I would still put no weight at all on my own estimate of how much faster I am, and that is close to the only thing here I am confident about.

This is not a case against AI tools. It is a case against measuring them by how much code they spit out.

---

## The Strongest Number Against Me

If you want to argue the other side, the best evidence available today is Microsoft's. Early in 2026 they rolled Claude Code and GitHub Copilot CLI out across the organisation and [studied what happened](https://arxiv.org/abs/2607.01418): tens of thousands of engineers, four months, and the engineers who adopted merged roughly 24% more pull requests than the counterfactual said they would have. That is not a lab. It is not sixteen volunteers. It is the largest field measurement of agentic coding tools anyone has published, and the effect is large and it points the right way.

I take it seriously. I also notice what the unit is.

The authors get there ahead of any critic. Their paper says it plainly: a merged PR is not the same as the value it delivers. That is the argument of this entire post, conceded inside the study that is supposed to answer it. Twenty-four percent more merged pull requests is consistent with 24% more delivered value. It is equally consistent with the same work arriving in smaller slices, which is what happened at Uber the moment diff count landed on a dashboard.

There are narrower caveats and I will not pretend I have chased all of them. The comparison is against engineers who already had AI in their IDE, so what it measures is the increment from adding an agent rather than the effect of AI from zero. Four months is not long enough for maintenance cost to turn up. And engineers chose for themselves whether to adopt, which is a problem large enough that a later part of this series is about nothing else.

None of that makes the study wrong. It makes it a good measurement of pull request volume, and pull request volume behaves the way lines of code always did. Easy to report. Easy to move, if somebody decides that moving it matters. Blind to what the system underneath is doing.

---

## What This Actually Means If You Are Leading a Team

If you are six months into your AI investment and your main evidence of ROI is that the output counter went up, whether that counter says lines or pull requests or tasks completed, I would gently suggest that should worry you more than reassure you. You might be measuring the accumulation of future cost and calling it present-day value.

Here is what I would look at instead:

**Cycle time** — Are features actually reaching production faster? Not "is code being written faster" but "is *value* arriving sooner?" Those sound like the same question. They are not.

**Rework rate** — Are you fixing more bugs in AI-assisted code? If generated code has a higher defect rate, your productivity gain is a mirage.

**Cognitive complexity trends** — Are your systems getting harder to understand? Tools like SonarQube measure this. If complexity is climbing faster than before AI adoption, you have got a compounding problem.

**Developer effort distribution** — Where is time actually going? If writing dropped from 20% to 10% of developer time, but code review grew from 15% to 30%, you have not reduced the burden. You have moved it.

None of that is exotic, and it is not just my read. DORA's [2026 work on the ROI of AI-assisted development](https://services.google.com/fh/files/misc/dora-roi-of-ai-assisted-software-development-2026.pdf) arrives somewhere similar from a different direction: the return on these tools tracks the strength of the engineering system around them rather than the tools themselves. The things that decide it are unglamorous. Whether code review has any slack left in it. Whether people trust the test suite enough to act on a red build, which is the one I have never seen anybody audit. How much sits between a merge and production. Where those are weak, faster generation fills a queue and waits there.

The measurement science here is still catching up to the tooling.

---

## The Uncomfortable Question

Here is what I would ask any engineering leader who reports AI productivity gains based on code output:

*"If your best engineer spent last week deleting 3,000 lines of AI-generated code and replacing them with 300 lines that do the same thing better — would your dashboard show that as a win or a loss?"*

On a line count that week is a catastrophe. On a PR count it is one merged pull request, which is what a typo fix is worth too. Neither number has any way of seeing what actually happened.

If your dashboard shows a loss, you are measuring the wrong thing. And you are quietly incentivising your team to build a larger, slower, more fragile system, in exchange for a chart that goes up and to the right.

The goal was never more code. It was better systems, delivered faster, maintained cheaply.

That dashboard from three weeks after the celebration, the one counting the cleanup as more "productivity" — I keep wondering whether anyone ever went back and reconciled those numbers. I suspect not.

---

*Next in this series: **Optimising Benchmark Tasks Instead of Real Delivery Work** — why the fact that coding is not the bottleneck makes most AI productivity claims irrelevant to actual delivery speed.*

---

**Series: AI Productivity Myths — Lessons from the Real World**
A series examining how engineering leaders misjudge AI coding productivity, based on industry research and real-world enterprise adoption patterns.

---

### References

- GitClear, [*The Maintainability Gap: 2026 AI Code Quality Research*](https://gitkraken.gitclear.com/the_ai_code_quality_maintainability_gap) — 623M changes, 2023–2026; moved code down from 21% to 3.8%, duplicated blocks up 81%, cross-file calls down 35%, error-masking constructs up 47%
- GitClear, [*Coding on Copilot: Data Shows AI's Downward Pressure on Code Quality*](https://www.gitclear.com/coding_on_copilot_data_shows_ais_downward_pressure_on_code_quality) — the earlier 153M-line report, including the churn-doubling projection that came in at 15%
- METR, [*Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity*](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/) — the 19% slowdown finding, and the perception gap that outlived it
- METR, [*We are Changing our Developer Productivity Experiment Design*](https://metr.org/blog/2026-02-24-uplift-update/) (February 2026) — the selection-bias problems, and their own view that the early-2025 estimate read low
- Microsoft Research, [*Adoption and Impact of Command-Line AI Coding Agents*](https://arxiv.org/abs/2607.01418) — tens of thousands of engineers, roughly 24% more merged PRs, plus the authors' own caveat on PR count as a proxy
- DORA, [*The ROI of AI-Assisted Software Development*](https://services.google.com/fh/files/misc/dora-roi-of-ai-assisted-software-development-2026.pdf) (2026) — why returns track engineering foundations rather than tooling
- Gergely Orosz, [*Measuring Developer Productivity*](https://newsletter.pragmaticengineer.com/p/measuring-developer-productivity) — why output metrics corrupt engineering behaviour
- Fred Brooks, [*The Mythical Man-Month*](https://en.wikipedia.org/wiki/The_Mythical_Man-Month) — the original argument against LOC as a productivity metric
