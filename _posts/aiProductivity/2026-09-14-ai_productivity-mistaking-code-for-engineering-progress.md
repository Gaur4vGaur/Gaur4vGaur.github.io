---
title: Mistaking Code Production for Engineering Progress - AI Productivity Myths
description: Lines of code and PR counts went up after AI adoption, and so did production incidents. Why code volume is a poor proxy for engineering progress, and what GitClear's data on refactoring decline shows instead
tags: ["gen-ai", "ai-productivity-myths", "software-engineering"]
category: ["ai"]
date: 2026-09-14
permalink: '2026/ai/ai_productivity-mistaking-code-for-engineering-progress/'
image:
  path: /assets/blog_assets/img/ai/2026-09-14-ai_productivity-mistaking-code-for-engineering-progress/coverImage.jpg
  width: 800
  height: 500
---

## AI Productivity Myths: Lessons from the Real World #1

*Why lines of code generated is almost meaningless as a measure of AI-assisted development.*

A few months ago, one of our teams celebrated a milestone in their quarterly review. AI adoption was up. The productivity dashboard showed that developers were generating on average 40% more code per sprint. The tech lead showed this as a major win.

Three weeks later I was on a call for a production issue. The challenge was that errors were coming in three different formats depending on which endpoint you hit. The alerting was blind to a category of failure it had always caught before.

We had a centralised exception handler. It logged context, mapped it to the right HTTP status, and pushed these alerts to our observability stack. When we investigated, we found that recent AI-assisted PRs had started introducing their own try-catch blocks inline. Each one caught exceptions locally, logged in a slightly different format, returned a slightly different error shape. Some swallowed the exception instead of letting it propagate up to the handler that would have alerted us.

Each one of those PRs was correct. Every one passed review, including the reviews I did myself. We were all checking for correctness, and consistency isn't the kind of thing that shows up in a git diff.

Cleaning it up took most of a sprint. And that productivity dashboard? It counted the original generation and the cleanup as output. Twice the code, twice the "productivity", for a net loss of engineering time. The dashboard was going up while the system got worse underneath it. That's the mistake I keep seeing where teams confuse code generation with engineering progress.

## The LOC Trap, Reloaded

Fred Brooks called this out in [*The Mythical Man-Month*](https://en.wikipedia.org/wiki/The_Mythical_Man-Month) decades ago. He explicitly mentioned that measuring programming productivity by lines of code is nonsensical. Everyone agreed and then somehow forgot. So why did we rebuild the exact same dashboard the moment AI arrived?

We are using the same flawed metric with AI branding on it and presenting that to boards. When a team lead reports that AI tools helped produce 40% more code, the follow-up I want to hear is *"Did we actually need 40% more code?"* Usually the answer is no. What we needed was the same outcomes with less effort, and effort in software lives overwhelmingly outside the act of typing. I'll come back to that.

There is a difference this time and it is worth naming. Teams aren't defending line counts out loud any more. The dashboards have moved on to merged pull requests, agent tasks completed and suggestions accepted. It is the same instinct in a unit that sounds more respectable in front of a board, counting the artefacts of work and reporting the count as progress.

## Why Senior Engineers Delete Code

Here's a pattern you'll recognise if you have led engineering teams for any length of time. Your best engineers often produce fewer lines of code than anyone else on the team. Some of their most impactful weeks come out to *negative* line counts.

That's expertise rather than laziness. I haven't fully worked out why the instinct for deletion over addition takes years to develop, but it does.

GitClear has been measuring this rather than speculating about it. Their [2026 analysis](https://gitkraken.gitclear.com/the_ai_code_quality_maintainability_gap) covers 623 million changes from 2023 to 2026, and the figure that stopped me had nothing to do with volume. Moved code, their proxy for refactoring, dropped from 21% of all changes in 2022 to 3.8% by the middle of this year. Duplicated blocks are up 81% across the same window. Cross-file function calls, which is roughly what reuse looks like in a diff, are down 35%.

Those three together describe a codebase that has stopped being rearranged when work goes in, and very little gets moved, or deleted. When someone takes 2,000 lines of tangled logic and replaces it with 200 clean ones, that looks like a loss on any volume metric. It is an enormous win for the system, and it is exactly the activity that has gone quiet.

One correction I owe, since I quoted the earlier version of this research at people for the better part of a year. GitClear's [2024 report](https://www.gitclear.com/coding_on_copilot_data_shows_ais_downward_pressure_on_code_quality) predicted two-week code churn would double in the AI era. It didn't double. It went up 15%. The headline projection was too aggressive, and the part almost nobody quoted, refactoring falling off a cliff, turned out worse than predicted.

Senior engineers get this intuitively. Every line of code is a liability, because every line has to be read, understood, tested, and maintained. So the best solution often makes code disappear, and that takes different forms like a well-chosen abstraction that kills duplication or a config change that removes a custom implementation. Sometimes it's just a conversation with PO that drops the requirement entirely.

Now think about what AI coding metrics would say about this. An engineer spends a day understanding a system, realises three services can collapse into one, and deletes 4,000 lines. By every AI productivity metric in use today, that engineer had a terrible day. In reality, they may have saved the organisation months of future pain.

[Gergely Orosz tells a revealing story](https://newsletter.pragmaticengineer.com/p/measuring-developer-productivity) about what happens when you optimise for the wrong signal. When Uber introduced diff-count metrics, engineers started creating more, smaller changes to *look* productive. They flooded CI systems, driving up costs. The metric improved and engineering got worse. We are setting ourselves up for the same trap with AI-generated LOC.

![Four GitClear stats: moved code down, duplication and error-masking up](/assets/blog_assets/img/ai/2026-09-14-ai_productivity-mistaking-code-for-engineering-progress/theRearrangementStats.jpg)

## AI's Tendency Toward Verbose Implementations

This gets worse when you look at what AI coding tools actually excel at, which is producing plausible code quickly. That skill carries a built-in bias toward verbosity.

Sometimes more code is genuinely the right call. Explicit beats implicit. A verbose but readable implementation can be better than a clever one-liner that nobody understands at 3am when production is on fire. I'm not arguing for code golf.

But AI-generated verbosity is a specific kind of bad, because it is *default* verbosity from ignorance of context rather than *chosen* verbosity for clarity. This distinction matters a lot more than I initially thought. Ask an AI assistant to implement a feature and you'll get a complete, working solution, longer than what an experienced developer would write, because it optimises for correctness and completeness in isolation. It may not pick up the utility you wrote last month to do exactly this. It doesn't realise the framework provides a one-liner if you structure the problem slightly differently. It can't tell the difference between "I should be explicit here for readability" and "I am reinventing something that already exists three directories over."

The handler drift I opened with is the cleanest example I have of it. The AI did exactly what it was asked, every single time. Each PR added code, each one passed review on its own terms, and nothing was wrong inside any of them. What we lost lived across them. One handler gave us one error shape, and one error shape gave our alerting something to fire on. No single diff broke that, and all of them together did.

GitClear tracks error-masking constructs, which is the failure mode buried in there, and they are up 47% since 2023. Our inline handlers were exactly that. I'd like to think we were an unlucky outlier, and the data says we were ordinary. I'm still not sure how you review for a property that isn't visible in the file in front of you.

## Complexity as the Hidden Cost

Code volume isn't a perfect proxy for system complexity, and I acknowledged that above. But it is a *directional* one, and in aggregate it holds. When your codebase grows by 30-40% in a quarter without a corresponding growth in functionality, complexity is almost certainly growing with it. And system complexity is the single biggest thing determining how fast your team can move over time. I haven't found a way around that in eighteen years.

Every line of code carries ongoing costs that nobody puts on a dashboard:

* Cognitive load for anyone working nearby
* Test coverage, without which it becomes a ticking risk
* Review time on every future change to it
* Dependencies it drags in that need constant updating
* Migration effort during every platform change

When AI tools grow your code volume by 30-40%, each of these costs grows. The productivity gain at the moment of writing is real and I'm not denying that. But it can be entirely eaten up by the downstream cost of maintaining a bigger, more complex system.

Then there is the study I keep coming back to, and the update to it that I nearly missed. In mid-2025, [METR](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/) found that experienced developers using AI coding tools took 19% *longer* on real-world tasks. The setup was specific. It had 16 open-source contributors working in their own repositories, code they'd lived in for years, 246 real issues averaging about two hours each, with Cursor Pro and Claude.

In February 2026, METR [published an update](https://metr.org/blog/2026-02-24-uplift-update/) that takes a fair amount of it back. They are redesigning the experiment, and the reasons aren't flattering to the original. Developers who would no longer work without AI declined to take part at all. Somewhere between 30% and 50% of participants avoided submitting exactly the tasks where they expected to want AI. The pay rate for the follow-up work dropped from $150 an hour to $50, which made recruitment worse again. Their own summary is that the newer data amounts to very weak evidence in either direction, and that developers are probably more sped up now, in early 2026, than the early-2025 estimate suggested.

So the 19% was never a fact about AI-assisted development. It was a measurement of sixteen people in one setting, and the people who ran it now think it read low.

What survives is the part that makes me think harder. Those developers *believed* they were 20% faster. Whatever the true effect was, it wasn't the effect they perceived, and not one of them could feel the gap while it was happening. Reviewing, and integrating suggestions consumed time none of them accounted for. Selection bias moved the headline number around, but it does not explain away a room full of experienced engineers being wrong about their own week.

I could never square that finding with my own experience, because I do feel faster on certain tasks. The update moves me off the fence, slightly. Maybe I wasn't fooling myself. I would still put no weight at all on my own estimate of how much faster I am, and that's close to the only thing here I am confident about. None of that is an argument against the tools, only against measuring them by how much code they produce.

## The Strongest Number Against Me

If you want to argue the other side, the best evidence available today is Microsoft's. Early in 2026 they rolled Claude Code and GitHub Copilot CLI out across the organisation and [studied what happened](https://arxiv.org/abs/2607.01418). Tens of thousands of engineers, four months, and the ones who adopted merged roughly 24% more pull requests than the counterfactual said they would have. That is not a lab, and it is not a small group of volunteers. It's the largest measurement of agentic coding tools anyone has published, the effect is large, and it points the right way. I take it seriously. I also notice what the unit is.

The authors get there ahead of any critic. Their paper says a merged PR isn't the same as the value it delivers, which is the argument of this entire post, conceded inside the study that is supposed to answer it. Twenty-four percent more merged pull requests is consistent with 24% more delivered value. It is equally consistent with the same work arriving in smaller slices, which is what happened at Uber the moment diff count landed on a dashboard.

There are narrower caveats and I won't pretend I have chased all of them. The comparison is against engineers who already had AI in their IDE, so what it measures is the increment from adding an agent rather than the effect of AI from zero. Four months is not long enough for maintenance cost to turn up. And engineers chose for themselves whether to adopt.

None of that makes the study wrong. It is a good measurement of pull request volume, and pull request volume behaves the way lines of code always did. It is easy to report and showcase, if somebody decides that moving it matters. Blind to what the system underneath is doing.

## What This Actually Means If You Are Leading a Team

If you're six months into your AI investment and your main evidence of ROI is that the output counter went up, whether that counter says lines or pull requests or tasks completed, that should worry you more than it reassures you. You might be measuring the accumulation of future cost and calling it present-day value.

Here is what I would look at instead.

**Cycle time** as *value* reaching production sooner, not just code getting written faster? Those two come apart more often than anyone expects.

**Rework rate** counts if you are fixing more bugs in AI-assisted code? If generated code carries a higher defect rate, your productivity gain is a mirage.

**Cognitive complexity trend** is when your application getting harder to understand? Tools like SonarQube measure this. If complexity is climbing faster than it did before AI adoption, you've got a compounding problem.

**Developer effort distribution** is where the time actually going? If writing dropped from 20% to 10% of developer time while code review grew from 15% to 30%, you have moved the burden rather than reduced it.

None of that is exotic, and it is not just my read. DORA's [2026 work on the ROI of AI-assisted development](https://services.google.com/fh/files/misc/dora-roi-of-ai-assisted-software-development-2026.pdf) arrives somewhere similar from a different direction. The return on these tools tracks the strength of the engineering system around them rather than the tools themselves. The things that decide it are unglamorous. Whether code review has any slack left in it. Whether people trust the test suite enough to act on a red build, which is the one I have never seen anybody audit. How much sits between a merge and production. Where those are weak, faster generation fills a queue and waits there, and the measurement science is still catching up to the tooling.

## The Uncomfortable Question

Here is what I would ask any engineering leader who reports AI productivity gains based on code output:

*"If your best engineer spent last week deleting 3,000 lines of AI-generated code and replacing them with 300 lines that do the same thing better, would your dashboard show that as a win or a loss?"*

On a line count that week is a catastrophe. On a PR count it is one merged pull request, which is what a typo fix is worth too. Neither number has any way of seeing what actually happened. If your dashboard shows a loss, you are measuring the wrong thing, and you are rewarding your team for building a larger, slower, more fragile system in exchange for a chart that goes up and to the right. The goal was never more code. It was better systems that delivers business value and maintained cheaply.

---

*Next in this series: **Optimising Benchmark Tasks Instead of Real Delivery Work**, on why the fact that coding is not the bottleneck makes most AI productivity claims irrelevant to actual delivery speed.*

**Series: AI Productivity Myths, Lessons from the Real World**
A series examining how engineering leaders misjudge AI coding productivity, based on industry research and real-world enterprise adoption patterns.

### References

* GitClear, [*The Maintainability Gap: 2026 AI Code Quality Research*](https://gitkraken.gitclear.com/the_ai_code_quality_maintainability_gap) — 623M changes, 2023–2026; moved code down from 21% to 3.8%, duplicated blocks up 81%, cross-file calls down 35%, error-masking constructs up 47%
* GitClear, [*Coding on Copilot: Data Shows AI's Downward Pressure on Code Quality*](https://www.gitclear.com/coding_on_copilot_data_shows_ais_downward_pressure_on_code_quality) — the earlier 153M-line report, including the churn-doubling projection that came in at 15%
* METR, [*Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity*](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/) — the 19% slowdown finding, and the perception gap that outlived it
* METR, [*We are Changing our Developer Productivity Experiment Design*](https://metr.org/blog/2026-02-24-uplift-update/) (February 2026) — the selection-bias problems, and their own view that the early-2025 estimate read low
* Microsoft Research, [*Adoption and Impact of Command-Line AI Coding Agents*](https://arxiv.org/abs/2607.01418) — tens of thousands of engineers, roughly 24% more merged PRs, plus the authors' own caveat on PR count as a proxy
* DORA, [*The ROI of AI-Assisted Software Development*](https://services.google.com/fh/files/misc/dora-roi-of-ai-assisted-software-development-2026.pdf) (2026) — why returns track engineering foundations rather than tooling
* Gergely Orosz, [*Measuring Developer Productivity*](https://newsletter.pragmaticengineer.com/p/measuring-developer-productivity) — why output metrics corrupt engineering behaviour
* Fred Brooks, [*The Mythical Man-Month*](https://en.wikipedia.org/wiki/The_Mythical_Man-Month) — the original argument against LOC as a productivity metric
