---
title: Gen AI Isn't Solving the Problem Most Development Teams Actually Have
description: We optimised for code generation speed while the real bottleneck — cognitive overhead and knowing where to make changes — stayed completely untouched.
tags: ["gen-ai", "developer-productivity", "software-engineering"]
category: ["ai"]
date: 2026-06-01
permalink: '2025/ai/gen-ai-not-solving-problem-dev-have/'
image:
  path: /assets/blog_assets/img/ai/2026-06-01-gen-ai-faster-at-wrong-thing/coverImage.png
  width: 800
  height: 500
---

It was an afternoon when one of our reconciliation flows started throwing `NullPointerExceptions` in production. The fix, once we found it, was two lines. Finding those two lines took nearly six hours. Three engineers and endless log grepping. Tracing through an integration application with JSF UI that predates most of the libraries we take for granted today. No modern APIs exposed. No clean service boundary to isolate the problem. Just a chain of legacy integration points that required someone to hold the full mental map of the system in their head to understand what was breaking where.

Six months into using Gen AI extensively in software delivery, I keep coming back to those production bugs. They still feel like the norm, not the exception.

## The Conference Demo vs Your Monday Morning

The keynote demos are genuinely impressive. The presenter opens a laptop, talks to an AI assistant, and a fully functional web service materialises. The crowd cheers. It looks like the future.

And it is — for some problems. But most of the development work I do, the work I actually live in, does not look like spinning up a new service. It looks like debugging a reconciliation failure buried inside a legacy integration platform. It looks like a cron batch job that failed silently and left no useful trace. It looks like a data transformation bug hidden four systems deep in a component last documented sometime around 2018 by somebody who no longer works for the organisation. There are still systems working with XMLs.

The demos are not wrong. They just solve a problem that I rarely have.

## The Productivity Paradox

After six months with Gen AI tools, I have landed on something that feels uncomfortably true:

*Developers were never slow at writing code. They were slow at figuring out **where** to write it.*

Think about the last bug you fixed. How long did the actual code change take once you understood the problem? Five minutes? Ten? The hours were spent elsewhere. Reading unfamiliar code. Tracing execution paths. Building a mental model of the system. Validating assumptions. Eliminating false leads. The bottleneck was understanding - cognitive overhead. Not typing.

Gen AI has undeniably accelerated code generation. Boilerplate code, unit tests, documentation drafts, simple feature implementations — all of these can now be produced significantly faster than before. I've seen feature development accelerate in my own team. But the dominant bottleneck in most enterprise systems was never code generation. It was comprehension. The hidden dependencies. The opaque patterns that only become visible after somebody has spent enough time on a codebase to develop intuition about it.  That part stayed exactly where it was.

We got dramatically faster at the part that was already manageable.

## A Real Example: When Context Lives Outside the Code

Recently, I investigated a reconciliation issue where transactions appeared incorrect on the UI. The AI was genuinely useful at first. It quickly highlighted suspicious code paths within the application and suggested plausible root causes.

None of them were entirely correct. After several hours we eventually traced the issue through an integration database, a legacy transformation component, and an upstream system that communicated through this legacy integration layer. The component predated modern API standards and I could not write any MCP or connect to its database in any way.

The final code change took minutes. Understanding the path the data had taken through the system consumed most of the day. The interesting lesson was not that the AI failed. The interesting lesson was that the AI only had visibility into part of the problem. The knowledge required to solve the issue lived across multiple systems, integration layers, historical design decisions, and several teams.
The real challenge here was reconstructing context.

## Gen AI Amplifies What Already Exists

One thing surprised me during the last six months. Gen AI amplifies whatever it touches. Not just the good parts.

If your applications are well documented, integration contracts are clear, and operational practices are mature, AI can deliver meaningful productivity gains. The model has enough context to reason effectively.

If your environment contains technical debt, unclear ownership boundaries, fragile deployment processes, or institutional knowledge locked inside a handful of experienced engineers, AI surfaces those weaknesses just as effectively. I've seen AI-generated changes pass every unit test and then fail immediately when they hit an integration layer. The problem wasn't the generated code. The problem was that the integration contract existed only in somebody's head. That issue existed long before Gen AI arrived. AI simply made it impossible to ignore.

Gen AI has become a forcing function for problems that were always there. Pipeline complexity, team silos, missing runbooks — they did not appear with AI. In many ways, Gen AI acts like an amplifier. Good engineering practices become more valuable. Weak engineering practices become more visible. If you were already carrying process debt, Gen AI lands in that environment and makes the debt harder to defer.

This observation aligns closely with findings from the DORA research programme. The report has increasingly highlighted that AI's impact is heavily influenced by the surrounding engineering system rather than the tooling itself.

## Maybe We've Been Measuring The Wrong Thing

Most AI productivity discussions focus on output.

Lines of code written.

Features completed.

Pull requests merged.

Like someone was telling me that they have written a service while they were on a train journey. But enterprise software rarely fails because developers could not type quickly enough. It fails because knowledge is fragmented. Teams cannot trace dependencies. Integration contracts are undocumented. Critical operational knowledge exists only inside someone's head.

If Gen AI is teaching us anything, it may be that software engineering productivity has always been a knowledge-management problem disguised as a coding problem. The more I work with AI, the more I suspect that this distinction matters.

## The Real Unlock: Context, Not Code

I have been trying to articulate where productivity genuinely improves. The best answer I have today is simple:

*Productivity is fewer hours spent chasing failures you cannot explain.*

The contained problems — bugs where the scope is clear and the relevant context is available — are noticeably faster to solve. Describe the issue, provide the relevant code and prompt right. The AI often gets you surprisingly close to the answer.

The difficult problems are different. A failure appears in one system but originates three hops upstream. The relevant context is spread across source code, operational dashboards, integration platforms, databases, support tickets, and undocumented assumptions accumulated over years. AI cannot reason about information it cannot access. 
The real opportunity is not generating more code. It is about It is helping developers navigate complexity and reducing the search space. Surfacing hidden patterns. Making system behaviour more understandable.

That is fundamentally a context problem. Not a coding problem.

## The Bottleneck Simply Moves

Another pattern I've observed is that AI often shifts bottlenecks rather than eliminating them. Feature development that previously took several days may now take hours.

But deployment approvals, compliance reviews, security assessments, testing processes, release windows, and governance controls have not accelerated at the same pace. The queue simply moves downstream. Code gets generated faster. Organisational throughput does not necessarily improve.

In highly regulated environments, the distance between "code written" and "code deployed" often remains the dominant constraint. If anything, AI has made that gap more visible. The development teams can move faster. The surrounding delivery system frequently cannot.

It is not uncommon that releases are moved because of end of month activities or business peak activities.

## The Next Enterprise Problem: Agent Sprawl

Another challenge is already emerging. For years, organisations struggled with tool sprawl. Now we are creating agent sprawl.

One team uses GitHub Copilot. Another uses Claude and someone using cursor. Someone else is experimenting with MCP servers, vector databases, and workflow orchestration platforms.

Teams are building custom internal agents to help the platform. Every agent develops its own context boundary, permissions model, knowledge source, and operational behaviour. Over time this begins to look familiar. We have seen similar patterns before with microservices, cloud platforms, and integration technologies. Multiple solutions for same problem with slight variations.

Without standardisation, governance, and ownership, complexity grows faster than value. Access to AI is no longer the limiting factor. Integration and consistency increasingly are.

## What This Means For Engineering Teams

The AI story is becoming less about individual tools and more about organisational architecture. The teams that will benefit most from Gen AI are unlikely to be the teams with the most sophisticated prompts.

They will be the teams that can feed AI enough and have invested in:

* Strong observability
* Well-defined integration contracts
* Discoverable architectural knowledge
* Consistent engineering practices

In other words, the foundations that good engineering organisations have always needed. 

The uncomfortable reality is that AI cannot compensate for missing context. It can only amplify whatever context already exists. And I do not have a clean answer for how you achieve that in a large organisation. I am not sure anyone does yet.

## Final Thoughts

I still believe Gen AI represents a genuine shift in software engineering. I am faster than I was a year ago. My team is faster than it was a year ago. Many activities that once felt repetitive now take a fraction of the time.That is real progress.

But I still think about that production issue. Six hours searching for two lines of code. Three engineers on a call. A JSF application none of us originally designed. A chain of integration points that only became understandable after piecing together knowledge from multiple systems and multiple people.

Gen AI did not change that day. It could not. The dominant problem was never code generation. The problem was that the knowledge required to understand the system was fragmented and largely invisible.

The biggest productivity gains over the next few years may not come from better models. They may come from making systems more legible. Better observability. Clearer integration contracts. Architectural decisions that live somewhere other than inside somebody's head. Gen AI did not create the need for those things. It simply made it much harder to pretend they were optional.

---

*Have you seen a similar pattern in your organisation? Where AI works brilliantly for contained problems but struggles at integration boundaries? I'd be interested to hear what has worked — and what hasn't — in your teams.*

**References**
- [DORA Report 2025 — State of AI-assisted Software Development](https://dora.dev/dora-report-2025?utm_source=chatgpt.com){:target="_blank"}
- [Paradis et al. — How much does AI impact development speed? An enterprise-based randomized controlled trial (Google)](https://arxiv.org/abs/2410.12944?utm_source=chatgpt.com)
- [Afroz et al. — Developer Productivity with GenAI](https://arxiv.org/abs/2510.24265?utm_source=chatgpt.com)