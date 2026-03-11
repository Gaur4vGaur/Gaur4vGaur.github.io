---
title: Essential Monitoring Metrics for Cloud Native Systems 
description: 
tags: ["cloud", "cloud native", "monitoring"]
category: ["cloud native"]
date: 2025-07-18
permalink: '2026/cloud_native/essential-monitoring-metrics-part1/'
image:
  path: https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/cloudnative/2026-03-10-monitoring-cloud-native-services-part1/coverImage.png
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

## Monitoring vs Observability – Why it matters?

I can say that monitoring used to mean one thing:

> Is the server alive?

If CPU was below 80% and memory was below 70%, everything looked fine. 

Modern systems are not the same. You can have a healthy-looking infrastructure, a normal CPU utilisation, a healthy memory graph and can still have a production outage. I have seen services returning _timeouts_ for hours while dashboards showed everything green. That can happen as traditional monitoring focuses on resources while failures can occur during interactions between services.

This is where observability comes in. Monitoring answers: _"Is something wrong?"_ and observability answers: _"Why is it wrong?"_ You need both.

### What all you need?
In practice, you need three things. **Metrics** are used to _detect problem_, **logs** to _explain errors_ and **traces** to _discover latency_. If your metrics are wrong, you would never know something is failing. And if you don't know something is failing, you never check logs and traces. Which is why metrics are the entry point of any investigation.

Diagram - Observability // TODO


## Typical monitoring challenges
Most of the time, teams don't have strategies for monitoring. It is the last backlog item to be picked up before the final production release. One service team adds a dashboard, another adds alerts and a third team introduces a different naming convention.

Six months down the line, you get duplicate metrics and inconsistent naming. There are no standard dashboards and alerts that nobody trusts. Eventually, teams ignore alerts, stop relying on monitoring and fall back to guesswork. That is a dangerous place to be.

One pattern I have seen repeatedly is **metric explosion without clarity**. A service exposes 400 metrics, and nobody knows which one matters.

Good monitoring is not about collecting more metrics. It is about collecting the **right metrics**. A production-ready service rarely needs more than 10–20 core metrics and a small number of critical alerts. Everything else is investigation detail. Not operational signal.


## Four signals that every service needs

I recommend every service must expose below four signals. I sometimes refer to them as minimum survival metrics.
- Latency
- Traffic
- Errors
- Saturation

They can help you diagnose most production incidents. Let us discuss them.
