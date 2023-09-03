---
title: 'Choreography Pattern: <TODO>'
description: <TODO>
tags: ["cloud", "distributed systems", "design", "scaling"]
category: ["architecture", "patterns"]
date: 2023-09-02
permalink: 'patterns/choreography-pattern/'
counterlink: 'patterns-choreography-pattern/'
image:
  path: https://raw.githubusercontent.com/Gaur4vGaur/traveller/master/images/patterns/2023-09-02-choreography-pattern/choreography-pattern-cover-image.jpg
  width: 1200
  height: 630
---

## Introduction
In today's rapidly evolving technology landscape, applications migrating to the cloud adopt microservice architecture. The microservice architecture, while offering benefits like scalability, reusability, and adaptability, presents a unique challenge - effectively managing communication among these microservices. Successfully coordinating messages among the services is an essential aspect of designing microservices. There are two popular methodologies available to seamlessly weave together these microservices. The first is [Service Orchestration](https://www.gaurgaurav.com/patterns/orchestration-pattern/), which I described in my last article. This article aims to understand and unravel the pros and cons of the second methodology, the Choreography.


## Problem Context
Is there a one-size-fits-all solution? Can a central orchestrator effectively manage all the business problems, or are there scenarios where a different approach besides being preferable is also essential? The problem statement is the same as we have already covered during the discussion of the Orchestration Pattern. 

We have a collection of microservices, where each microservice is responsible for a specific operation or a particular domain. Yet, they operate independently, almost like parallel universes, often unaware of each other's existence. Imagine a user initiating a request like submitting forms or searching a catalogue. These actions set off a series of business transactions that ripple across this microservices cluster. The challenge is clear: How can we ensure these transactions flow seamlessly from one microservice to another, completing a successful business transaction?

To address this challenge, we've introduced the concept of an [orchestrator](https://www.gaurgaurav.com/patterns/orchestration-pattern/#what-is-orchestration-pattern), a central authority tasked with coordinating and streamlining the flow of transactions across these autonomous microservices. But here's the question that looms large: Is this [orchestration pattern](https://www.gaurgaurav.com/patterns/orchestration-pattern/#what-is-orchestration-pattern) a universal solution, or does its effectiveness vary depending on the scenario? To answer this, we need to explore the intricacies and evaluate the costs associated with adopting this pattern.

## Avoiding Distributed Monolith Anti-pattern
While designing for effective microservices architecture, it's crucial to stay clear of what we call the "Distributed Monolith" anti-pattern. This term is often associated with scenarios where the Orchestrator pattern while providing some benefits, can introduce complexities and challenges that might not make it the preferred solution in a few contexts. Let us have a look.

1. **Tightly Coupled Services**<br>
In the Orchestrator pattern, all services are linked inseparably to the orchestrator. Each service must explicitly receive commands and report back to the orchestrator. This interdependence mandates that the orchestrator possess some domain knowledge about the responsibilities and intricacies of each service.<br>
Now, envision introducing a new feature to this current business flow. A change in the flow might require the rewire orchestrator, as precise coordination is essential, potentially affecting the existing logic. While a well-structured orchestrator can facilitate this process, such implementations often become complicated to manage over time.

2. **Tightly Coupled Teams**<br>
One of the primary reasons for adopting a microservices architecture is to expedite development by releasing teams from interdependencies. However, when employing the Orchestrator pattern, an interesting dynamic emerges. Consider a scenario where Team A is busy enhancing a feature while Team B is making updates to their service. Simultaneously, Team C is adding new functionality to the solution. <br>
Since the orchestrator service tightly couples with all these teams' services, all three teams must collaborate to ensure that their respective changes to the orchestrator service integrate seamlessly with existing and upcoming flows. This coordination often feels like working with monolithic applications, where the teams are bound by release cycles for significant changes.

By proactively recognizing these challenges within the Orchestrator pattern, we can equip ourselves to make informed architectural decisions, ensuring that our microservices ecosystem remains agile, scalable, and efficient. With this anti-pattern, we will encounter all the problems of the monolithic architecture and all the issues of the microservices architecture. Now, the question arises: Is there a pattern that can guide us in designing a solution capable of addressing these complex scenarios?

## Exploring the Choreography Pattern
Imagine the Choreography pattern as a graceful dance choreography. In a dance, performers receive instructions on how to execute their steps, yet each dancer retains individual responsibility for their performance. Similarly, in microservices, this pattern directs seamless business processes, where each microservice takes its cue, much like a dancer following their choreographer.

In contrast to the Orchestrator pattern's centralized conductor, the Choreography pattern utilizes a dumb message broker. This shift promotes loose coupling among microservices, untangling the web of interdependencies. Now, each microservice becomes an autonomous dancer, deciding when and how to execute its steps.

In this collection of microservices, every participant subscribes to relevant events broadcasted by the message broker. When a client initiates a request, the request seamlessly finds its way to the appropriate microservice, which processes it and emits the result as another event.

This chain of events continues, each microservice gracefully taking its turn until the entire transaction is completed. Think of it as an elaborate dance performance where every step leads to the next. To dive deeper into the Choreography pattern, let's walk through a practical example that will illuminate its workings and benefits.


## Use Case

## Implementation Details

## Why use Orchestration Pattern

## Important Considerations

## Summary


