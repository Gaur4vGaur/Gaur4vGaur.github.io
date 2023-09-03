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

## What is Orchestration Pattern

## Use Case

## Implementation Details

## Why use Orchestration Pattern

## Important Considerations

## Summary


