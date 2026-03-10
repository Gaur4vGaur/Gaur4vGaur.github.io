---
title: Essential Monitoring Metrics for Cloud Native Systems 
description: 
tags: ["cloud", "cloud native", "monitoring"]
category: ["cloud native"]
date: 2025-07-18
permalink: '2026/cloud_native/essential-monitoring-metrics-part1/'
image:
  path: https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/ai/2025-07-18-ai_vibe-coding-conversational-software-development-part4/coverImage.png
  width: 800
  height: 500
---

## Monitoring is not a dashboard only problem 

In the last couple of years, I have moved across a few product teams. Every time I walk into an engineering team and ask how monitoring works. I get a standard response.

> There is a dashboard

Usually, Grafana. Sometimes Kibana or Splunk. Team members have passed me links buried in wiki pages and nobody updates them anymore.

But dashboards don't make a system observable. I have worked on large scale cloud projects. All of them have dashboards but still struggle to answer basic questions during an incident. 

## What can fail?

The answer to this question can change how you think about monitoring. To me, monitoring is a system thinking problem. Any cloud solution that I work on is not a single application anymore.  With cloud solutions, I am talking about API Gateways, event streams, microservices, containers, managed cloud services, and external integrations. 

Something is always lagging somewhere, and all users are going to tell you is _"The system is slow"_.

It might mean that either an API timing out or queue is backing up, or a container restarting or network path degraded.

Diagram - Monitoring // TODO: Add diagram

After years of working on enterprise solutions, I can tell you that bugs do not cause major production incidents. They are often caused by lack of visibility, meaning missing the right metrics.

In this article, I try to put down some of the monitoring metrics that I find useful. These can help you diagnose incidents and keep the system reliable.
