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
As applications move to the cloud, organizations often adopt microservices. This architecture combines multiple services to create an entire business process, bringing benefits such as scalability, reusability, and ease of change. However, the biggest challenge of microservices architecture is managing the communication among these services. Microservices reduce complexity within applications but introduce new complexity in service interactions. Successfully coordinating messages among the services becomes a key aspect of applications using microservices. There are two popular methodologies available to manage these interservice interactions. The first is Service Orchestration, which I described in my last article. The second is Service Choreography, which I am going to illustrate in this post.


## Problem Context
The problem statement is the same as the one we covered in the Orchestration Pattern. That is, we have a collection of microservices, where each microservice is responsible for a specific operation or a particular domain. The microservices are completely decoupled from each other and may not even be aware of each other. Based on a business event, such as a user submitting a form, we need to complete a flow of transactions that span across these multiple microservices. One solution we offered was the use of an orchestrator, a central service that executes a flow by communicating with the relevant services until the flow is complete. But is this pattern well-suited for all scenarios? Let us take a look at the cost of using this pattern.

## What is Orchestration Pattern

## Use Case

## Implementation Details

## Why use Orchestration Pattern

## Important Considerations

## Summary


