---
title: Vibe Coding - Conversational Software Development - Part 4 Guiding AI Through Iteration
description: Discover how AI is transforming software development in Part 4 of my 'Vibe Coding' series. Explore conversational coding, code generation from prompt, and the future of developer productivity.
tags: ["ai", "vibe coding"]
category: ["ai"]
date: 2025-07-18
permalink: '2025/ai/vibe-coding-conversational-software-development-part4/'
image:
  path: https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-07-18-ai_vibe-coding-conversational-software-development-part4/coverImage.png
  width: 800
  height: 500
---

## Introduction

Welcome to the fourth and final post in my __Vibe Code__ series. In the [previous article](https://www.gaurgaurav.com/2025/ai/vibe-coding-conversational-software-development-part3/){:target="_blank"}, I explained how *system prompts* can steer AI behaviour by setting initial expectations and boundaries. But if you have worked on even any mildly complex application, you will know that the first draft is never the final version. You always have some sort of UX tweaks, performance enhancements, or new feature requirements.

That is where __task-level prompting__ comes into play. Once your foundation is in place, the next step is to provide instructions to AI to build, improve, and polish step by step. In this blog post, I will share several approaches that have worked well for me.

![Task-level prompt illustration](https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-07-18-ai_vibe-coding-conversational-software-development-part4/introduction.png)*Task-level prompt*


## Begin Small: Simplicity First, Structure Later

Shipping into production is never a one-and-done task. There are always iterations, refactoring, testing, and including non-functional requirements like accessibility to account for. It is unrealistic to expect an AI assistant to accomplish this all in a single prompt. 

Instead, __start small and iterate__. In the early phases, simplicity wins. Here is an example of an intentionally open-ended prompt I might use:

>Create a dashboard with spaces for visual elements. Assume type of charts or some reason you think works best for this data.

Here I am just focused on the dashboard screen. Using such a prompt will direct AI without boxing it. You are setting a vibe and offering intent rather than detailed instructions. The prompt is open and allows AI to make suggestions. But you have a long way to go from here.

![Task level prompting process](https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-07-18-ai_vibe-coding-conversational-software-development-part4/TaskLevelPromptingProcess.png)*Application Development Iterations*


## One Task at a Time: Iterating with Focus
Let us go deeper, take the example from my previous blog around `London Air Quality Data`. Once the initial dashboard was created, I narrowed the focus:

> Consider this London Air Quality Data and analyse the dataset. Suggest data visualization or graph that captures key trends on the dashboard.

Now the AI is not just coding, it is thinking. Below are a few suggestions it came up with.

![AI visualisation suggestions](https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-07-18-ai_vibe-coding-conversational-software-development-part4/VibeCoding3_AIsuggestions.png)*London Air Quality Data Insights*

It might surface insights you hadn’t noticed or suggest charts that suit the story your data is telling. This is where AI becomes a true collaborator, not just a code generator.

> If [London Air Quality Data](https://data.london.gov.uk/download/290a22f1-5eef-4801-8286-3f288b49093a/acce7f88-70f0-4fd0-9160-f02a9d96b2c3/air-quality-london.xlsx){:target="_blank"} is not available at above link, I have [committed a copy](https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/air-quality-london.csv){:target="_blank"} of it, as CSV, at the time writing this post.
{: .prompt-info }


## Refining with Follow-Up Prompts
When I first asked Copilot to generate a dashboard for the London Air Quality dataset, the output was functional but visually felt basic. Below is the screenshot of the dashboard that Copilot generated for me after some initial prompts. It looked like a prototype with basic white background, simple line charts, and limited interactions. If I were to demo to a stakeholder, it would not feel like a product.

It needs further iterations to aggregate the data and add the depth I was after. It was difficult to identify any likely trends, spikes or patterns. I wanted my dashboard to be presentable with smoother lines on a modern theme. 


![Initial Dashboard Output](https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-07-18-ai_vibe-coding-conversational-software-development-part4/VibeCoding3_AIOutput1.png)*Initial Design*

So, I refined with these two targeted prompts:

__Prompt 1__
>Visualize air quality trends over time. Aggregate data by half-year, use smoother lines, add subtle gridlines, and stretch the graph to full page width.”

__Prompt 2__ 
>The dashboard has a white background and basic styling. Can you apply a modern dark theme and make it more visually appealing?

Here is the result. A cleaner and more elegant dashboard that is much closer to production quality.

![Final Dashboard Output](https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-07-18-ai_vibe-coding-conversational-software-development-part4/VibeCoding3_AIOutput2.png)*Final Dashboard*

We started with a messy draft and steadily refine it. The process is like software delivery lifecycle. AI is not doing the job for you, rather it is accelerating the loop.

You can push it further based on your requirements:
-	>Make the chart mobile friendly.
- >Add a tooltip to show percentage change from the previous half-year.
- >Apply WCAG accessibility standards to the chart labels and colours
- >Do you notice any unusual patterns we should highlight?

This is where you can make AI to act as your pair programmer, iterating with you. I treat every refinement as a small backlog item. I decomposed the improvements into a single step. I try to provide the same clarity as I expect from a Jira ticket. In my opinion, the approach leads to cleaner outputs and fewer surprises. 


## Structure the Conversation for Better Outcomes
If you are not sure where to begin? Try out different prompts but they keys is to keep them focused and open-ended. Try something like:

> Give me a few different dashboard layouts that summarise the data.

First results are rarely the final one. Don’t stress about getting everything perfect upfront. It usually takes three or four iterations, sometimes more to before it is ready for production. The goal is to progress.

## Prompting Pitfalls
Task level prompting is not an art rather it needs logical structured approach to guide AI assistants. One of the fastest ways to make it go wrong is to overload a single prompt. Just as a junior developer struggle with a vague, multi-purpose task, AI also struggles when you mix too many objectives. Below are a few DOs and DON’T’s

### Don'ts
- Mix multiple objectives (bug fixes, feature requests and styling) in one prompt
- Leave AI to guess your requirements 
- Assuming AI will automatically meet your compliance, security and coding standards

### Instead
- Focus on one goal at a time
- Clearly specify your expected outcome
- Encourage creativity from AI like 
`what could be most insightful chart for this data set?`

## Final Thoughts
I believe we are still in the early stages of AI-assisted development. Most of the tools expect you to break problems down and provide clear prompts, iterating step by step. But the market is evolving quickly. New models like GPT-5 have longer memory and contextual awareness are already making it possible to carry conversations across multiple sessions. I think, soon, it should be possible to orchestrate entire workflows or assign JIRA tickets to AI assistants.

