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

*Why coding is not the bottleneck — and why speeding it up barely moves the needle on actual delivery.*

A few weeks ago I was at a company social event. It was one of those informal ones where people drift between conversations with drinks in hand. I expected the usual mix of project updates, football banter and weekend plans. What I got instead was wall-to-wall AI talk. More sort of personal kind. "What are you doing with it? What have you tried?"

Nobody asked about the domain complexity I work in daily. The upstream dependencies we untangled that quarter — that was invisible. The architectural decision that saved us three months of rework did not even register as a topic worth raising. They wanted AI stories.

I kept thinking about that evening for days afterwards. Not because the conversations were shallow — they weren't — but something about the shape of them bothered me. We have become so captivated by the tool that we have stopped asking whether it is pointed at the right problem. In software delivery, the right problem is almost never "typing code faster." Most people in that room would probably agree with me on this, if I asked them directly.

## The Bottleneck That Nobody Benchmarks

Here is a question I have been putting to engineering managers for the last six months: *What percentage of your delivery time is spent writing new code?*

The answers land between 10% and 20%. Sometimes lower. The rest gets consumed by everything that surrounds the code. As developer you need to understand requirements that keep shifting, aligning with stakeholders who changed their minds since the last conversation or debugging production systems for live bugs. Then there is the coordination work. Getting three teams with different sprint cadences to agree on an integration date. On top of that navigating codebases you did not write. I have barely scratched the surface of that list here, honestly.

GitHub's own [developer experience research](https://github.blog/news-insights/research/survey-reveals-ais-impact-on-the-developer-experience/){:target="_blank"} backs this up — developers spend as much time waiting for builds and tests as they do writing new code, and that is before you count meetings, code review, and security work.

That survey is from 2023, so you could reasonably argue it predates the tools everyone is currently excited about.  DORA's [ROI of AI-Assisted Software Development](https://services.google.com/fh/files/misc/dora-roi-of-ai-assisted-software-development-2026.pdf){:target="_blank"} report, published this May, arrives at the same place from the money side. The returns come from the organisational system around the tool, not the tool itself, and what drives them is bottleneck removal. Their modelling shows 35-40% gains on simple tasks and 10% or less on complex legacy code. The gains shrink exactly where the real work lives.

And yet code generation is exactly what most AI coding benchmarks measure — isolated greenfield tasks, a clean problem statement, a clear success criterion, no humans in the loop. Whereas in real world deliveries  you find product owners that contradicts their own Jira tickets, or a downstream service team that deployed a breaking change without telling anyone. And no benchmark captures that.

I observed this recently when one of our streams announced as its "first AI productivity win". They claimed more than 50% reduction in build time for a shared service that twenty-plus developers work on daily. It made for an impressive slide. But when I dug into what actually changed, the story was more interesting than the headline. A senior engineer had traced execution paths through containerised test infrastructure and discovered that PostgreSQL's JIT compilation feature — beneficial for large production databases — was adding catastrophic overhead to their small JUnit test database. SQL execution time dropped from sixty-two minutes to six once they turned it off.

AI helped them find the configuration faster once they knew what to look for. But knowing which question to ask? That required someone who understood how the layers interacted — someone who had been burned by similar problems before and had a feel for where to poke. That's the work no benchmark measures. It's also the work that delivered the win everyone celebrated.

## Requirements and Alignment: Where Delivery Actually Happens

I have spent entire sprints where the most impactful engineering activity was a forty-minute conversation with a product owner. It prevented us from building the wrong thing. No code was written. It also means sprint velocity stayed flat. And yet that single conversation saved days — possibly weeks — of wasted effort.

The hard part of most projects isn't the implementation. I wish it were. The hard part is getting five people with different priorities to agree on what "done" means. Trade-off communication — explaining that getting a feature quickly can come at the cost of something they value even more. These activities determine whether a project ships in six weeks or six months. They require judgment and the kind of trust that only builds through repeated interactions.

AI coding tools are staggeringly good at answering "how do I implement X?" But they can't help you figure out whether X is the right thing to implement. That question lives in human context in organisational politics, business strategy, regulatory constraints. None of which fits in a prompt window. Andrew Diamond makes a sharp observation in [*Software Engineering in the Age of AI*](https://adiamond.me/2026/06/software-engineering-in-the-age-of-ai/){:target="_blank"}: AI cannot know whether the code it just generated violates a legal requirement your product is subject to, or whether it conflicts with features another team is planning to ship next quarter. That knowledge lives in people or if we call institutional memory. It is the accumulated context that is present for decisions and was never written down.

Here's something I didn't expect. When AI coding assistants started becoming part of my day-to-day work, the time they saved wasn't magically reinvested in better requirements or deeper thinking. It mostly got spent somewhere else: reviewing generated code, correcting suggestions that missed important context, and bouncing between tools. If anything, the pull towards implementation got stronger because implementation suddenly felt cheap. But building the wrong thing never became cheaper. We just got faster at doing it. That's the thought I kept coming back to after that social event. Everyone was excited about speed. Very few people were asking where all that speed was taking us.

## Production Debugging: The Context Problem

Ask any senior engineer where their most stressful hours go. It is not "writing a new feature." It is "figuring out why something that worked yesterday does not work today." I have been on both sides of that, and the debugging side is not even close to the same kind of work.

Production debugging requires understanding system interactions across service boundaries and reading incomplete logs — and by "incomplete" I do not mean "we have not indexed them yet." I mean nobody thought this information was worth logging in the first place. Then you need the institutional knowledge: *this* service was deployed last Thursday while *that* team quietly changed their retry policy without updating the runbook. Good luck finding that in a prompt.

AI tools help with specific debugging tasks — pattern matching in logs, suggesting hypotheses, explaining unfamiliar code. I use them regularly and they are genuinely useful for that narrow band. But the hard part is knowing *where to look*. That requires exactly the kind of cross-system context that no model possesses. Speeding up code generation does nothing for the engineer at 2am tracing a timeout through four services and a message queue, trying to figure out which of seventeen changes deployed that day introduced a subtle ordering dependency. I've been that engineer. The bottleneck was never typing speed.

## Cross-Team Dependencies: The Silent Killer

I've never seen a project delayed by slow typing. I have seen dozens delayed by a dependency on another team. Waiting for an API contract to be finalised — it has been reprioritised and nobody told you. A shared library update that keeps getting bumped sprint after sprint. The platform team cannot provision your environment until you file a ticket, which requires approval from someone who is on leave until next week. Security review is backed up. And somewhere in there, a decision is blocked because it requires input from someone who is in back-to-back meetings until next Thursday. I could keep going, but you've probably lived through most of this yourself.

The [Theory of Constraints](https://en.wikipedia.org/wiki/Theory_of_constraints){:target="_blank"} applies to software delivery just as it applies to manufacturing — improving a non-bottleneck process does not improve system throughput. If coding is 15% of your cycle time and cross-team coordination is 40%, making coding twice as fast saves you 7.5%. Making coordination 25% more effective saves you 10%. The maths is not complicated. We just prefer not to look at it directly, because coordination problems are messy and political and do not have clean solutions you can demo on a slide.

A Harness-commissioned survey of 700 practitioners and managers, run this April, underlines the same point: [94% of engineering leaders](https://www.devopsdigest.com/ai-has-outpaced-how-engineering-organizations-measure-developer-productivity){:target="_blank"} say key factors — including tech debt, validation time, and developer burnout — are missing from their productivity metrics entirely. We are measuring what is easy to measure and calling it the whole picture. It's easier to ship a tool than to fix a process.

## The Uncomfortable Question

If your AI strategy is primarily "make developers write code faster," you're optimising a step that was already not the bottleneck. Your delivery speed is constrained by everything around the code — the understanding, the alignment, the coordination, the debugging, the waiting.

The industry is not blind to this. AI-powered log analysis, RAG over documentation, automated PR summaries, meeting transcription — these are real products solving real problems. I use several of them daily and find them genuinely helpful. But they still operate at the level of a single engineer's information access. They make it faster for *me* to find a log line or catch up on a meeting I missed. What they do not touch is the coordination layer. AI can summarise what was *said* in a meeting. It cannot surface what was *left unsaid*, and I don't think anyone has a tool for that yet.

A platform team built a tool that flagged when two squads were planning changes to the same service boundary in the same sprint. The detection worked — it surfaced genuine conflicts weeks before they would have caused integration failures. But adoption stalled. Teams did not trust an automated flag enough to change their plans. Resolving conflicts still required a human with enough context to drive consensus. The adoption problem was social, not technical. We keep running into this same wall across different teams and different tools.

There is a counter argument worth taking seriously: AI for coding is a stepping stone toward harder problems. I'm not fully convinced. Coding is a problem of *translation* — turning intent into instructions. Coordination is a problem of *negotiation* — reconciling competing intents. These require fundamentally different capabilities, and the risk is that you spend three years optimising the 15% while the 40% compounds.

The sharper objection is that the whole framing is already stale. Agents don't just type any more — they run the suite, trace the failing test, open the PR. That's true, and it's the argument I take most seriously. But watch where the queue forms. Everything an agent produces still lands in front of a human who has to decide whether to trust it, and that is precisely where teams are getting stuck now: review and verification have become the constraint that build time used to be. Researchers have started [rethinking review from first principles](https://arxiv.org/abs/2605.17548){:target="_blank"} because of it, and engineering leaders are [describing it in the same terms](https://www.cio.com/article/4207438/the-code-review-crisis-and-how-you-should-rebuild-review-models.html){:target="_blank"} — a review model that worked when humans wrote the code is now buckling under volume it was never designed for. DORA describes the mechanism as a J-curve: a dip before the gain, where the dip comes from verification overhead rather than from learning the tool. The constraint hasn't disappeared, it's just moved. Automating the 15% harder only pushes more volume into the queue sitting behind it.

So the question isn't whether AI helps — it does, and I use it every day. The real question is whether the distribution of our investment matches the distribution of our constraints. From what I've seen in my own practice, it does not, though I wouldn't claim that holds everywhere.

If you're an engineering leader reading this on a Monday morning, don't measure how long it takes code to move from PR to merge. Measure how long it takes an idea to become something a customer can use. Most organisations already have the data. It's buried in Jira tickets, Slack conversations, and calendar invites. When you discover that six weeks elapsed but only three days were spent writing code, the bottleneck becomes obvious. You don't need another tool to find it. You just need to look at the whole journey instead of one small part of it.

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
