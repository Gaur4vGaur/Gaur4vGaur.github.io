---
title: Vibe Coding - Conversational Software Development - Part 1
description: 
tags: ["ai", "vibe coding"]
category: ["ai"]
date: 2025-05-24
permalink: '2025/ai/vibe-coding-conversational-software-development-part1/'
image:
  path: https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-05-24-ai_vibe-coding-conversational-software-development-part1/2025-05-24-cover-image.jpg
  width: 800
  height: 500
---

## Inroduction
Since I started coding, I’ve seen developer communities strive to make programming more human-readable—almost like writing in English. Many modern languages introduced [syntactic sugar](https://en.wiktionary.org/wiki/syntactic_sugar){:target="_blank"} to make code more intuitive and conversational. But now, we are witnessing something far more transformative.

Natural language can now be translated directly into functional software. The concept is widely referred as __Vibe Coding__. It is an _AI first approach_ for rapid software development. I have put down a step-by-step diagram showing end-to-end experience with chat-based AI development based on my understanding.

![End-to-end experience with chat-based AI development](https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-05-24-ai_vibe-coding-conversational-software-development-part1/end-to-end-experience-chat-based-builders.png
)*End-to-end experience with chat-based AI development*

The term Vibe Coding was popularized in early 2025 by researcher [Andrej Karpathy](https://en.wikipedia.org/wiki/Andrej_Karpathy){:target="_blank"}, known for his work in Open API and Tesla. Vibe Coding is the process of describing goals in natural language, letting advanced models handle implementation, and iterating quickly without worrying too much about the underlying code. In short - you describe, and large language models generate and refine. [^footnote]


## My Exploration of Vibe Coding
I recently started looking into this new concept. While the definition makes it look straightforward, its applications vary depending on what you want to achieve. So far, I’ve seen it manifesting in several forms based on what you want to get out of it. You can perform things like:

- **Rapid Prototyping** – This is my favourite use case. It can be used for a quick UI mock-up or lightweight programs to validate a hypothesis.
- **Automating Repetitive Tasks** – Developers constantly look for ways to reduce [toil](https://sre.google/sre-book/eliminating-toil/){:target="_blank"}.
- **Experimentation** – You can ask how a library works, generate example usage, or compare multiple implementations—all in minutes.
- **Problem Solving and Debugging** – Something, I use for debugging obscure issues. Although, I am not convinced that it is saving time.


## Shifting Focus from Syntax to Problem Solving
In my opinion, Vibe Coding shifts your focus from typing every bracket or declaring every variable to problem solving. You concentrate on delivering features instead of obsessing over implementation details.

You can use AI tools to bounce off ideas, look for alternative implementations, and race through your development. But be cautious, at the end of the day, it is still your code. You are responsible for verifying it and test it and making it production ready. You must critically review the code whether all edge cases are handled and make any tweaks, if needed.


## AI Driven Tools
As I mentioned earlier that it does not matter whether you are a new developer or a seasoned programmer - you can leverage Vibe coding to your advantage. Just like you would hire the right candidate for a specific role, you need to use the right AI platform for the right use case. 

In this series of blogs, I intend to explore several available options and categorize them based on how and where they can be used effectively.

## Easy Entry: Chat Based Builders
Let us start with the most accessible option that is chat-based builders. If you are new to all this, the easiest way to get started is using conversational AI tools. All you need is a well-structured prompt. 
These tools gained traction with popularity of [Anthropic’s Claude artefact](https://www.anthropic.com/product). It lets you build live dashboard, tools and interfaces directly in chat window. 

Try prompting:

> Create a dashboard for analysing social media posts performance using most relevant visualisation from D3.

Claude will generate a live preview that you can tweak, inspect, and even publish. Below is a screenshot of what it generated for me.

![Claude generated social media dashboard](https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-05-24-ai_vibe-coding-conversational-software-development-part1/claude-generated-ui.png)*Claude generated Social Media Analystics dashboard*

Tools like ChatGPT, Google Gemini and Microsoft Copilot offer similar features. You can generate fully functional code with simple natural language prompts. 

Try something fun like 
> Build a simple recipe idea generator. I want to input a few ingredients I have, and it suggests a random recipe. I should be able to save recipes I like and add my own custom recipes

You’ll get a working prototype that you can experiment with or customize further. Here’s another screenshot—this one is from ChatGPT.

![ChatGPT generated Recipe Ideas UI](https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-05-24-ai_vibe-coding-conversational-software-development-part1/chatgpt-generated-ui.png)*ChatGPT generated Recipe Ideas UI*

Even if you have no programming background, this is an excellent place to start. What’s more—you can download and share the code.

## What Is Next
In part 2, I will continue this discussion.


## References
[^footnote]: [Wikipedia. "Vibe Coding." Modified May 10, 2025](https://en.wikipedia.org/wiki/Vibe_coding){:target="_blank"}