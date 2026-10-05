---
title: Mistaking Coding Speed for Delivery Speed - AI Productivity Myths
description: Coding is 10-20% of delivery time, yet most AI strategies optimise only that step. Why speeding up code generation barely moves the needle when requirements, debugging, and cross-team coordination are the real bottleneck.
tags: ["gen-ai", "ai-productivity-myths", "software-engineering"]
category: ["ai"]
date: 2026-10-02
permalink: '2026/ai/ai_productivity-mistaking-code-speed-for-delivery-speed/'
image:
  path: /assets/blog_assets/img/ai/2026-10-02-ai_productivity-mistaking-code-speed-for-delivery-speed/coverImage.jpg
  width: 800
  height: 500
---

## AI Productivity Myths: Lessons from the Real World #2

*Why coding is not the bottleneck and why speeding it up will not move an actual delivery.*

A few weeks ago I was at a company social event, one of those informal ones where people drift between conversations with drinks in hand. I'd gone along expecting project updates and some football banter. Almost every conversation I joined ended up on AI though, and in a surprisingly personal way. People wanted to know what you had been doing with it and what you'd tried.

Nobody asked about the domain complexity I work in daily. The upstream dependencies we untangled that quarter did not come up either. The architectural decision that saved us three months of rework did not even register as a topic worth raising because people wanted AI stories.

I kept thinking about that evening for days afterwards. Not because the conversations were shallow — they weren't — but something about the shape of them bothered me. We have become so captivated by the tool that we have stopped asking whether it is pointed at the right problem. In software delivery, the right problem is almost never "typing code faster." Most people in that room would probably agree with me on this, if I asked them directly.

## The Bottleneck That Nobody Benchmarks

Here is a question I have been putting to engineering managers for the last six months: *What percentage of your delivery time is spent writing new code?*

The answers usually land between 10% and 20%, and a couple of managers have gone lower. The rest goes on everything around the code. As a developer you spend it understanding requirements that keep shifting, chasing stakeholders who have changed their minds since the last meeting, or debugging live issues in code you never wrote. And that's before the coordination work, like getting three teams on different sprint cadences to agree on an integration date.

GitHub's own [developer experience research](https://github.blog/news-insights/research/survey-reveals-ais-impact-on-the-developer-experience/){:target="_blank"} backs this up. Developers in that survey said they spend as much time waiting on builds and tests as writing new code, and that was before anyone counted meetings or code review.

That survey is from 2023, so you could reasonably argue it predates the tools everyone is currently excited about.  DORA's [ROI of AI-Assisted Software Development](https://services.google.com/fh/files/misc/dora-roi-of-ai-assisted-software-development-2026.pdf){:target="_blank"} report, published this May, arrives at the same place from the money side. The returns come from the organisational system around the tool, not the tool itself, and what drives them is bottleneck removal. Their modelling shows 35-40% gains on simple tasks and 10% or less on complex legacy code. The gains shrink exactly where the real work lives.

And yet code generation is what most AI coding benchmarks measure, usually on isolated greenfield tasks with a clean problem statement and a clear pass or fail. Real deliveries have a product owner who contradicts their own Jira ticket, or a downstream team that shipped a breaking change without telling anyone. I haven't come across a benchmark that models either of those.

I saw this play out recently when one of our streams announced its "first AI productivity win": build time cut by more than half on a shared service that twenty-plus developers work on daily. When I looked into what had actually changed, AI turned out to be a fairly small part of it. A senior engineer had traced execution paths through the containerised test infrastructure and found that PostgreSQL's JIT compilation, which helps on large production databases, was adding huge overhead to their small JUnit test database. Turning it off took execution time from sixty-two minutes to six.

AI did help them find the right setting quickly, once they knew what they were looking for. Knowing what to look for was the hard bit. That came from someone who understood how the layers interacted and had been burned by something similar before, and it's the part the slide left out.

## Requirements and Alignment is Where Delivery Actually Happens

I have spent entire sprints where the most useful engineering activity was a forty-minute conversation with a product owner that stopped us building the wrong thing. Velocity stayed flat that sprint because no code got written, but that one conversation probably saved us days, maybe weeks, of wasted effort.

The hard part of most projects isn't the implementation. I wish it were. The difficutly is getting five people with different priorities to agree on what "done" means. Trade-off communication — explaining that getting a feature quickly can come at the cost of something they value even more. These activities determine whether a project ships in six weeks or six months. They require judgment and the kind of trust that only builds through repeated interactions.

AI coding tools are very good at answering "how do I implement X?" They can't tell you whether X is worth implementing, because that depends on business context and regulation, and very little of either fits in a prompt. Andrew Diamond makes the same point in [*Software Engineering in the Age of AI*](https://adiamond.me/2026/06/software-engineering-in-the-age-of-ai/){:target="_blank"}: AI cannot know whether the code it just generated breaks a legal requirement your product is subject to, or clashes with something another team plans to ship next quarter. That knowledge sits with people. Some call it institutional memory, the context behind decisions that never got written down.

When AI assistants became part of my day-to-day work, I assumed the time they saved would go into better requirements or deeper thinking. It mostly didn't. It went into reviewing generated code and fixing suggestions that had missed important context. If anything, implementation felt so cheap that the pull towards it got stronger, and building the wrong thing costs the same whether it took two days or two weeks to type. I kept coming back to that after the social event. Everyone in the room was excited about speed, and hardly anyone asked where it was taking us.

## Production Debugging is the Context Problem

Ask any senior engineer where their most stressful hours go. It is not "writing a new feature." It is "figuring out why something that worked yesterday does not work today." I have been on both sides of that, and the debugging side is not even close to the same kind of work.

Production debugging requires understanding system interactions across service boundaries and reading incomplete logs — and by "incomplete" I do not mean "we have not indexed them yet." I mean nobody thought this information was worth logging in the first place. Then you need the institutional knowledge: *this* service was deployed last Thursday while *that* team quietly changed their retry policy without updating the runbook.

AI tools help with specific debugging tasks — pattern matching in logs, suggesting hypotheses, explaining unfamiliar code. I use them regularly and they are genuinely useful for that narrow band. But the hard part is knowing *where to look*. That requires exactly the kind of cross-system context that no model possesses. Speeding up code generation does nothing for the engineer at 2am tracing a timeout through four services and a message queue, trying to figure out which of seventeen changes deployed that day introduced a subtle ordering dependency. I've been that engineer and the bottleneck was never typing speed.

## Cross-Team Dependencies

I've never seen a project delayed by slow typing. I have seen dozens delayed by a dependency on another team. Waiting for an API contract to be finalised — it has been reprioritised and nobody told you. A shared library update that keeps getting bumped sprint after sprint. The platform team cannot provision your environment until you file a ticket, which requires approval from someone who is on leave until next week. Security review is backed up. And somewhere in there, a decision is blocked because it requires input from someone who is in back-to-back meetings until next Thursday. I could keep going, but you've probably lived through most of this yourself.

The [Theory of Constraints](https://en.wikipedia.org/wiki/Theory_of_constraints){:target="_blank"} applies to software delivery just as it applies to manufacturing — improving a non-bottleneck process does not improve system throughput. If coding is 15% of your cycle time and cross-team coordination is 40%, making coding twice as fast saves you 7.5%. Making coordination 25% more effective saves you 10%. The maths is not complicated. We just prefer not to look at it directly, because coordination problems are messy and political and do not have clean solutions you can demo on a slide.

A Harness-commissioned survey of 700 practitioners and managers, run this April, underlines the same point: [94% of engineering leaders](https://www.devopsdigest.com/ai-has-outpaced-how-engineering-organizations-measure-developer-productivity){:target="_blank"} say key factors — including tech debt, validation time, and developer burnout — are missing from their productivity metrics entirely. We are measuring what is easy to measure and calling it the whole picture.

## The Uncomfortable Question

If your AI strategy is primarily "make developers write code faster," you're optimising a step that was already not the bottleneck. Your delivery speed is constrained by everything around the code — the understanding, the alignment, the coordination, the debugging, the waiting.

The industry is not blind to this. AI-powered log analysis, RAG over documentation, automated PR summaries, meeting transcription — these are real products solving real problems. I use several of them daily and find them genuinely helpful. But they still operate at the level of a single engineer's information access. They make it faster for *me* to find a log line or catch up on a meeting I missed. What they do not touch is the coordination layer. AI can summarise what was *said* in a meeting. It cannot surface what was *left unsaid*, and I don't think anyone has a tool for that yet.

One platform team built a tool that flagged when two squads were planning changes to the same service boundary in the same sprint. It worked, and it caught real conflicts weeks before they would have broken an integration. Hardly anyone acted on the flags though. Teams didn't trust an automated warning enough to change their plans, so resolving a conflict still needed someone with enough context to get both sides to agree. We've hit the same problem with other tools too.

There is a counter argument worth taking seriously: AI for coding is a stepping stone toward harder problems. I'm not fully convinced. Coding is a problem of *translation* — turning intent into instructions. Coordination is a problem of *negotiation* — reconciling competing intents. These require fundamentally different capabilities, and the risk is that you spend three years optimising the 15% while the 40% compounds.

A stronger objection is that this whole framing is already out of date. Agents don't just type now. They run the test suite, trace the failing test and open the PR. I take that seriously, but look at where the queue builds up. Everything an agent produces still lands in front of a human who has to decide whether to trust it, and that's where teams are getting stuck now. Review and verification have become the constraint that build time used to be. Researchers have started [rethinking review from first principles](https://arxiv.org/abs/2605.17548){:target="_blank"} because of it, and engineering leaders are [saying much the same thing](https://www.cio.com/article/4207438/the-code-review-crisis-and-how-you-should-rebuild-review-models.html){:target="_blank"}: review processes designed for human-written code can't keep up with the volume. DORA describes this as a J-curve, a dip before the gain, where the dip comes from verification overhead rather than from people learning the tool. So the constraint has moved downstream, and pushing harder on the 15% just sends more work into the queue behind it.

So the question isn't whether AI helps — it does, and I use it every day. The real question is whether the distribution of our investment matches the distribution of our constraints. From what I've seen in my own practice, it does not, though I wouldn't claim that holds everywhere.

If you lead an engineering team, try measuring how long it takes an idea to reach a customer instead of how long a PR takes to merge. Most organisations already have that data scattered across Jira and Slack. Once you find that six weeks went by and only three days of it were spent writing code, it's fairly clear where the bottleneck is, and you didn't need a new tool to find it.

---

*Next in this series: **Confusing Activity Metrics with Outcomes** — why commits, PRs, tickets, and adoption rates tell you about busyness, not effectiveness.*

**Series: AI Productivity Myths, Lessons from the Real World**
A series examining how engineering leaders misjudge AI coding productivity, based on industry research and real-world enterprise adoption patterns.

### References

- GitHub, [*Survey Reveals AI's Impact on the Developer Experience*](https://github.blog/news-insights/research/survey-reveals-ais-impact-on-the-developer-experience/){:target="_blank"} (2023) — developers spend as much time waiting for builds/tests as writing new code
- DORA / Google Cloud, [*The ROI of AI-Assisted Software Development*](https://services.google.com/fh/files/misc/dora-roi-of-ai-assisted-software-development-2026.pdf){:target="_blank"} (May 2026) — returns come from the organisational system rather than the tools; bottleneck removal drives ROI; 35-40% gains on simple tasks against 10% or less on complex legacy code
- Harness / DevOps Digest, [*AI Has Outpaced How Engineering Organizations Measure Developer Productivity*](https://www.devopsdigest.com/ai-has-outpaced-how-engineering-organizations-measure-developer-productivity){:target="_blank"} (April 2026 survey, 700 respondents) — 94% of engineering leaders say key productivity factors (tech debt, validation time, burnout) are missing from their metrics entirely
- [*Rethinking Code Review in the Age of AI*](https://arxiv.org/abs/2605.17548){:target="_blank"} and CIO, [*The Code Review Crisis*](https://www.cio.com/article/4207438/the-code-review-crisis-and-how-you-should-rebuild-review-models.html){:target="_blank"} — review and verification emerging as the downstream constraint as agent-generated volume grows
- Andrew Diamond, [*Software Engineering in the Age of AI*](https://adiamond.me/2026/06/software-engineering-in-the-age-of-ai/){:target="_blank"} — AI optimises the wrong bottleneck while hollowing out institutional capacity
- Eliyahu Goldratt, [*The Theory of Constraints*](https://en.wikipedia.org/wiki/Theory_of_constraints){:target="_blank"} — improving a non-bottleneck process does not improve system throughput
