---
title: 'Understanding Immutable Objects in Java'
description: 
tags: ["advancedjava"] 
category: ["advanced java", "programming", "tutorial"]
date: 2024-12-22
permalink: '/advancedJava_understandingImmutability'
image:
  path: https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/java/2024-12-22-advancedJava_understandingImmutability.jpg
  width: 800
  height: 500
---

## What is Immutability?
Before I discuss records and why they are needed, I need to articulate the concept of immutability. Immutability is a key aspect of clean and safe programming.

Let us define immutability - an immutable object is an object whose state cannot be changed once instantiated, where the state is the data contained in the object instance. When an object's state is set, it stays the same throughout its lifetime. In Java, for example, immutable objects do not have any setter methods to guarantee their state never changes.
