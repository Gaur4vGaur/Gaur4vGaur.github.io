---
title: Mistaking Coding Speed for Delivery Speed - AI Productivity Myths
description: 
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

*Why coding is not the bottleneck — and why speeding it up barely moves the needle on actual delivery.*

A few weeks ago I was at a company social event — one of those informal ones where people drift between conversations with drinks in hand. I expected the usual mix of project updates, football banter and weekend plans. What I got instead was wall-to-wall AI talk. More sort of personal kind. "What are you doing with it? What have you tried?"

Nobody asked about the domain complexity I work in daily. The upstream dependencies we untangled that quarter — that was invisible. The architectural decision that saved us three months of rework did not even register as a topic worth raising. They wanted AI stories.

I kept thinking about that evening for days afterwards. Not because the conversations were shallow — they were not — but something about the shape of them unsettled me. I spent a while trying to articulate what bothered me before it clicked. We have become so captivated by the tool that we have stopped asking whether it is pointed at the right problem. In software delivery, the right problem is almost never "typing code faster." Most people in that room would agree with me on this point.

---

## The Bottleneck That Nobody Benchmarks

Here is a question I have been putting to engineering managers for the last six months: *What percentage of your delivery time is spent writing new code?*

The answers land between 10% and 20%. Sometimes lower. The rest gets consumed by everything that surrounds the code. As developer you need understanding requirements that keep shifting, aligning with stakeholders who changed their minds since the last conversation, debugging production systems where the documentation was last updated eighteen months ago. Then there is the coordination work. Getting three teams with different sprint cadences to agree on an integration date. On top of that navigating codebases you did not write, left behind by people who have since moved on to different companies. I have barely scratched the surface of that list, honestly.

GitHub's own [developer experience research](https://github.blog/news-insights/research/survey-reveals-ais-impact-on-the-developer-experience/){:target="_blank"} backs this up — developers spend as much time waiting for builds and tests as they do writing new code, and that is before you count meetings, code review, and security work.

That survey is from 2023, so you could reasonably argue it predates the tools everyone is currently excited about.  DORA's [ROI of AI-Assisted Software Development](https://services.google.com/fh/files/misc/dora-roi-of-ai-assisted-software-development-2026.pdf){:target="_blank"} report, published this May, arrives at the same place from the money side. The returns come from the organisational system around the tool, not the tool, and what drives them is bottleneck removal. Their modelling shows 35-40% gains on simple tasks and 10% or less on complex legacy code. The gains shrink exactly where the real work lives.

And yet code generation is exactly what most AI coding benchmarks measure. Isolated greenfield tasks. A clean problem statement, a clear success criterion, no humans in the loop. Real delivery gives you a Slack thread from a product owner that contradicts last Tuesday's Jira ticket. A downstream service team who deployed a breaking change without telling anyone. No benchmark captures that. I am not sure one could.

I watched this play out recently with what one of our streams announced as its "first AI productivity win" — more than 50% reduction in build time for a shared service that twenty-plus developers work on daily. Impressive numbers. The kind of thing that makes a good slide. But when I dug into what actually changed, the story was more interesting than the headline. A senior engineer had traced execution paths through containerised test infrastructure and discovered that PostgreSQL's JIT compilation feature — beneficial for large production databases — was adding catastrophic overhead to their small JUnit test database. SQL execution time dropped from sixty-two minutes to six once they turned it off.

AI helped them find the configuration faster once they knew what to look for. But knowing which question to ask? That required someone who understood how the layers interacted — someone who had been burned by similar problems before and had a feel for where to poke. That is the work no benchmark measures. It is also the work that delivered the win everyone celebrated.

---

## Requirements and Alignment: Where Delivery Actually Happens

I have spent entire sprints where the most impactful engineering activity was a forty-minute conversation with a product owner. It prevented us from building the wrong thing. No code was written. It also means sprint velocity stayed flat. And yet that single conversation saved days — possibly weeks — of wasted effort downstream.

The hard part of most projects is not the implementation. I wish it were. The hard part is getting five people with different priorities to agree on what "done" means. Trade-off communication — explaining that doing the thing they want in the time they want it will break something they care about even more. These activities determine whether a project ships in six weeks or six months. They require judgment and the kind of trust that only builds through repeated interactions.

AI coding tools are staggeringly good at answering "how do I implement X?" But they cannot help you figure out whether X is the right thing to implement. That question lives in human context — organisational politics, business strategy, regulatory constraints — none of which fits in a prompt window. Andrew Diamond makes a sharp observation in [*Software Engineering in the Age of AI*](https://adiamond.me/2026/06/software-engineering-in-the-age-of-ai/){:target="_blank"}: AI cannot know whether the code it just generated violates a legal requirement your product is subject to, or whether it conflicts with features another team is planning to ship next quarter. That knowledge lives in people. In institutional memory. In the accumulated context of having been present for decisions that were never written down.

Here is a thing I did not expect, and I am still processing its implications. When we started using AI coding assistants, the time saved writing code did not flow back into requirements work. It flowed into reviewing AI-generated code, fixing context-blind suggestions, and context-switching between tools. The temptation to skip straight to implementation got stronger — because implementation now *felt* cheap. But the cost of building the wrong thing did not get cheaper. It just got faster to arrive at. I keep coming back to that image of people at the social event, excited about speed, not asking where the speed points.

---

## Production Debugging: The Context Problem

Ask any senior engineer where their most stressful hours go. It is not "writing a new feature." It is "figuring out why something that worked yesterday does not work today." I have been on both sides of that, and the debugging side is not even close to the same kind of work.

Production debugging requires understanding system interactions across service boundaries and reading incomplete logs — and by "incomplete" I do not mean "we have not indexed them yet." I mean nobody thought this information was worth logging in the first place. You are working with absence. Then you need the institutional knowledge: *this* service was deployed last Thursday while *that* team quietly changed their retry policy without updating the runbook. Good luck finding that in a prompt.

AI tools help with specific debugging tasks — pattern matching in logs, suggesting hypotheses, explaining unfamiliar code. I use them regularly and they are genuinely useful for that narrow band. But the hard part is knowing *where to look*. That requires exactly the kind of cross-system context that no model possesses. Speeding up code generation does nothing for the engineer at 2am tracing a timeout through four services and a message queue, trying to figure out which of seventeen changes deployed that day introduced a subtle ordering dependency. I have been that engineer. The bottleneck was never typing speed.

---

## Cross-Team Dependencies: The Silent Killer

I have never seen a project delayed by slow typing. Not once.

I have seen dozens delayed by a dependency on another team. Waiting for an API contract to be finalised — except the team owning it has been reprioritised and nobody told you. A shared library update that keeps getting bumped sprint after sprint. The platform team cannot provision your environment until you file a ticket, which requires approval from someone who is on leave until next week. Security review is backed up. And somewhere in there, a decision is blocked because it requires input from someone who is in back-to-back meetings until next Thursday. I could keep going, but you have lived this. You know.

The [Theory of Constraints](https://en.wikipedia.org/wiki/Theory_of_constraints){:target="_blank"} applies to software delivery just as it applies to manufacturing — improving a non-bottleneck process does not improve system throughput. If coding is 15% of your cycle time and cross-team coordination is 40%, making coding twice as fast saves you 7.5%. Making coordination 25% more effective saves you 10%. The maths is not complicated. We just prefer not to look at it directly, because coordination problems are messy and political and do not have clean solutions you can demo on a slide.

A Harness-commissioned survey of 700 practitioners and managers, run this April, underlines the same point: [94% of engineering leaders](https://www.devopsdigest.com/ai-has-outpaced-how-engineering-organizations-measure-developer-productivity){:target="_blank"} say key factors — including tech debt, validation time, and developer burnout — are missing from their productivity metrics entirely. We are measuring what is easy to measure and calling it the whole picture. I suspect we know this. It is easier to ship a tool than to fix a process.

---

## The Uncomfortable Question

If your AI strategy is primarily "make developers write code faster," you are optimising a step that was already not the bottleneck. Your delivery speed is constrained by everything around the code — the understanding, the alignment, the coordination, the debugging, the waiting.

The industry is not blind to this. AI-powered log analysis, RAG over documentation, automated PR summaries, meeting transcription — these are real products solving real problems. I use several of them daily and find them genuinely helpful. But they still operate at the level of a single engineer's information access. They make it faster for *me* to find a log line or catch up on a meeting I missed. What they do not touch is the coordination layer — the part where five people need to reconcile conflicting assumptions, or where a decision is blocked because it requires trust that only builds over time. AI can summarise what was *said* in a meeting. It cannot surface what was *left unsaid*. I am not sure that is even a solvable problem, honestly.

I saw this play out internally. A platform team built a tool that flagged when two squads were planning changes to the same service boundary in the same sprint. The detection worked — it surfaced genuine conflicts weeks before they would have caused integration failures. But adoption stalled. Teams did not trust an automated flag enough to change their plans. Resolving conflicts still required a human with enough context and relationship capital to broker the negotiation. The technology worked. The adoption problem was social, not technical. I spent a while puzzling over why we keep running into this wall before accepting that maybe the wall is load-bearing.

There is a counterargument worth taking seriously: AI for coding is a stepping stone toward harder problems. Maybe. But coding is a problem of *translation* — turning intent into instructions. Coordination is a problem of *negotiation* — reconciling competing intents. These require fundamentally different capabilities, and the risk is that you spend three years optimising the 15% while the 40% compounds quietly. I have not decided whether I think the stepping-stone argument is compelling or just comforting. I keep going back and forth.

The sharper objection is that the whole framing is already stale. Agents do not just type any more — they run the suite, trace the failing test, open the PR. That is true, and it is the version of this argument I take most seriously. But watch where the queue forms. Everything an agent produces still lands in front of a human who has to decide whether to trust it, and that is precisely where teams are getting stuck now: review and verification have become the constraint that build time used to be. Researchers have started [rethinking review from first principles](https://arxiv.org/abs/2605.17548){:target="_blank"} because of it, and engineering leaders are [describing it in the same terms](https://www.cio.com/article/4207438/the-code-review-crisis-and-how-you-should-rebuild-review-models.html){:target="_blank"} — a review model that worked when humans wrote the code, now buckling under volume it was never designed for. DORA describes the mechanism as a J-curve: a dip before the gain, and the dip comes from verification overhead rather than from learning the tool. So the constraint moved. It did not go away. Automating the 15% harder just pushes more volume into the queue sitting behind it.

So the question is not whether AI helps. It does. I use it every day. The question is whether the *distribution* of our investment matches the *distribution* of our constraints. From what I can see — it does not. But I am looking from one vantage point in one organisation, so I hold that loosely.

If you are an engineering leader reading this on a Monday morning, here is one thing you can do this week: measure cycle time from *idea to production*, not from *PR to merge*. Most teams already have the data — it is just scattered across Jira, Slack, and calendar invites that nobody has stitched together. When you see that your average feature takes six weeks from concept to deployment but only three days of that is active coding, the investment question answers itself. You do not need a new tool. You need a different chart.

---

*Next in this series: **Confusing Activity Metrics with Outcomes** — why commits, PRs, tickets, and adoption rates tell you about busyness, not effectiveness.*

---

**Series: AI Productivity Myths — Lessons from the Real World**
A series examining how engineering leaders misjudge AI coding productivity, based on industry research and real-world enterprise adoption patterns.

---

### References

- GitHub, [*Survey Reveals AI's Impact on the Developer Experience*](https://github.blog/news-insights/research/survey-reveals-ais-impact-on-the-developer-experience/){:target="_blank"} (2023) — developers spend as much time waiting for builds/tests as writing new code
- DORA / Google Cloud, [*The ROI of AI-Assisted Software Development*](https://services.google.com/fh/files/misc/dora-roi-of-ai-assisted-software-development-2026.pdf){:target="_blank"} (May 2026) — returns come from the organisational system rather than the tools; bottleneck removal drives ROI; 35-40% gains on simple tasks against 10% or less on complex legacy code
- Harness / DevOps Digest, [*AI Has Outpaced How Engineering Organizations Measure Developer Productivity*](https://www.devopsdigest.com/ai-has-outpaced-how-engineering-organizations-measure-developer-productivity){:target="_blank"} (April 2026 survey, 700 respondents) — 94% of engineering leaders say key productivity factors (tech debt, validation time, burnout) are missing from their metrics entirely
- [*Rethinking Code Review in the Age of AI*](https://arxiv.org/abs/2605.17548){:target="_blank"} and CIO, [*The Code Review Crisis*](https://www.cio.com/article/4207438/the-code-review-crisis-and-how-you-should-rebuild-review-models.html){:target="_blank"} — review and verification emerging as the downstream constraint as agent-generated volume grows
- Andrew Diamond, [*Software Engineering in the Age of AI*](https://adiamond.me/2026/06/software-engineering-in-the-age-of-ai/){:target="_blank"} — AI optimises the wrong bottleneck while hollowing out institutional capacity
- Eliyahu Goldratt, [*The Theory of Constraints*](https://en.wikipedia.org/wiki/Theory_of_constraints){:target="_blank"} — improving a non-bottleneck process does not improve system throughput
