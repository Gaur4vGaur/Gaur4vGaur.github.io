---
title: Vibe Coding - Conversational Software Development - Part 2 In Practice
description: Discover how AI is transforming software development in Part 2 of our 'Vibe Coding' series. Explore conversational coding, real-time code generation in practice, and the future of developer productivity.
tags: ["ai", "vibe coding"]
category: ["ai"]
date: 2025-06-21
permalink: '2025/ai/vibe-coding-conversational-software-development-part2/'
image:
  path: https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-06-21-ai_vibe-coding-conversational-software-development-part2/cover-image-compressed.png
  width: 800
  height: 500
---

## Inroduction
In my [previous blog post](https://www.gaurgaurav.com/2025/ai/vibe-coding-conversational-software-development-part1/){:target="_blank"}, I introduced the concept of __Vibe Coding__. It is one of the new ways that is attracting even non-programmers. Users can describe their thoughts using natural language and AI tools would convert that into a working application. Spotting this opportunity, I thought I should experiment and understand what that actually looks like in action. I took this opportunity to test out a few tools and see how they really impact my workflow. 

![Chat-based AI development illustration](https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-06-21-ai_vibe-coding-conversational-software-development-part2/introduction_vibe.png
)*Vibe coding is a declarative approach*

It is not just about automating tasks; it is about changing our behaviour on how to approach a problem. To me, it feels like a declarative approach, especially when you are navigating a new framework or language for the first time. 

## Smarter Coding in the IDE: GitHub Copilot

I first started with the most common tool that is gaining popularity in the corporate world. It is [GitHub Copilot](https://github.com/features/copilot){:target="_blank"}. I have been using it regularly in VS Code, and while it is not exactly magic, it is undoubtedly helpful. When I am deep into the code and I use it for quick assistance with things like scaffolding code, writing tests, or debugging tricky edge cases. It saves me time context switching to browser. You can interact with Copilot right in your code or you can open a chat window to explain your issue.

Copilot now has an “*agent mode*,” which allows you to give broader instructions that can span across multiple files at feature level. It is not flawless and can sometimes come up with generic solutions. But most of the time it helps to cut down time spent on the boilerplate code.

![Copilot Agent Mode](https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-06-21-ai_vibe-coding-conversational-software-development-part2/copilot-agent-mode.png)*Github Copilot with agent mode*

The best part of copilot is that it is embedded in the editor, which means it is just a click away whenever I need it. Also, it is context-aware, it often makes suggestions that fits with existing application style and architecture.

![Copilot Inline Editing](https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-06-21-ai_vibe-coding-conversational-software-development-part2/inline-copilot.png)*Github Copilot inline editing*

## Terminal-First Development: Claude Code
There are developers who love working in the terminal. I would suggest tools like [Claude Code](https://docs.anthropic.com/en/home){:target="_blank"} or [OpenCodeX CLI](https://help.openai.com/en/articles/11096431-openai-codex-cli-getting-started){:target="_blank"} for them. These are worth checking out. Although it works from the terminal, but it understands your entire codebase and can implement changes based on natural language commands.

I recently tried out Claude Code for a side project. From the CLI, I was able to set up a basic project, refactor functions, and even tidy up some old scripts. It is not a traditional simple text-based interface, and I recommend it as a must have in your toolkit.

Anthropic has done a good job with [Claude Code documenration and working examples](https://docs.anthropic.com/en/docs/claude-code/quickstart){:target="_blank"}. It is worth checking out if you are curious.

## AI in the Browser: Replit
The other tool I tried was [Replit](https://replit.com/){:target="_blank"}. It has latest AI features in browser and target developers above beginner level. I provided it below prompt to generate me a Trello like dashboard to manage agile teams.
>Generate me an app something like Trello where I can track my tasks from to-do, in-progress and done. It should have a board to move tickets and ticket analysis capabilities

![Replit prompt example](https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-06-21-ai_vibe-coding-conversational-software-development-part2/Replit-prompt.png)*Replit prompt example*

What really impressed me was how collaborative the experience is—you provide a prompt, and it guides you through the development process step by step. It feels almost like working alongside a junior developer who has done their homework. You are not just getting code; you are getting a plan, followed by clean, organized output. And if something goes wrong, Replit adapts and tries again. It really helps you navigate the development journey, generating and even debugging as it goes.

This tool is especially convenient for generating a quick prototype. While it might not be the best fit for highly complex systems, it shines when you need to get something up and running in a browser environment. Below is a sample application that it generated for my above prompt. It has done a decent job and has added features like tags for tickets, included date on every ticket and even presenting in-progress bar per ticket.

![Replit priview example](https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-06-21-ai_vibe-coding-conversational-software-development-part2/Replit-preview.png)*Replit priview example*

## A Growing Ecosystem
These are not the only available options in the market. The app store for AI-driven development is rapidly filling up, and each tool has its own feature set and niche.
-	[**Lovable**](https://lovable.dev/){:target=_"blank"} is a great option for working with UIs and interfaces that you can tweak visually. It is another non-IDE alternative that lets you can interact with simple natural language text prompts. You can describe what you want, and it updates the interface accordingly. It also supports backend connections, integrations, and multi-user collaboration.
-	[**Bolt.new**](https://bolt.new/){:target="_blank"} is another available option for full stack apps and frameworks like [Next.js](https://nextjs.org/){:target="_blank"} or [Svelte](https://svelte.dev/){:target="_blank"}, and mobile via Expo. I think Bolt is outstanding if you are a beginner. The design outputs are sometimes a bit better than what I was expecting.
-	Another similar tool is [**V0 by Vercel**](https://v0.dev/){:target="_blank"}. It is more developer focussed online tool. It is built by the team behind [Next.js](https://nextjs.org/){:target="_blank"} and it supports modern web technologies out of the box.

The tool to consider and adopt really depends on your problem statement. 


## Final Thoughts
I think AI tools are enhancing our programming capabilities. If I have to pick one of those, I will pick that seamlessly blends into the background, providing just the right amount of assistance without being intrusive. Tools are only valuable if they help you build faster and improves overall experience. I will continue the discussion in part 3 of this series
