---
title: AI Everday Basics - Part 1 <TODO>
description: <TODO>
tags: [<TODO>]
category: ["ai"]
date: 2025-05-24
permalink: '2025/ai/TODO/'
image:
  path: https://raw.githubusercontent.com/Gaur4vGaur/traveller/refs/heads/master/images/random/2025-01-07-the-art-of-possible/cover.jpg
  width: 800
  height: 500
---

## Inroduction
Artificial Intelligence(AI) is clearly having a moment. I was surprized to learn that AI and machine learning are not new concepts, rather first appeared in the 1950s. Back then, researchers were already trying to have machines that would think independently. But they struggled with language understanding and common-sense reasoning. As time went by, more and more data became easily available. And Scientists were able to forge statistical models through which machines could trace patterns and learn rules by themselves. This was a significant advancement in the field[^footnote].

And now AI seems to be appearing everywhere, and its applications are only getting more powerful. That got me thinking—it’s time for me to learn more about these concepts. I do not just want to understand AI better; I also want to reiterate and share what I have learned. The more I know, the better prepared I am for my next role.

But as soon as I start reading about AI, I came across many new terminologies. Some clicked right away, and others left me scratching my head. So, my first goal is to wrap my head around these everyday AI basics.

## Machine Learning
When I was in high school, I was fond of chess and wanted to get better at it. So, my father bought me a gaming console with [Battle Chess](https://en.wikipedia.org/wiki/Battle_Chess){:target="_blank"}. I played that game over and over again for a week, and each time it beat me. I started to wonder if the thing in the box was smarter than me. As it turns out, the console was not necessarily smarter than me; it was just a whole lot faster. A smart developer had coded `IF-ELSE` boolean statements. All these statements were pre-programmed. Whenever I made a move, the console would speed through its logic to calculate its next best position. The logic was good enough to beat me but was just [rule-based](https://en.wikipedia.org/wiki/Rule-based_system){:target="_blank"}. The developers would need a chess master to guide them writing their logic.

Decades later, Google developed a computer program [AlphaZero](https://en.wikipedia.org/wiki/AlphaZero){:target="_blank"}. The program can learn games without being taught by humans. The algorithm can do much more than just playing chess. It has mastered Shogi and Go, games so complex that they cannot be programmed using simple `IF-ELSE`. AlphaZero uses a method of deep learning called artificial neural networks, which is a form of machine learning.

Machine learning is now not only restricted to these games; we are experience it every day, even if we do not realize it. Websites like Amazon suggest new products you might want to purchase, or Netflix recommends movies you might like-that is machine learning in play. Unlike traditional programming, where developers must tell a machine exactly what to do, these advanced algorithms can learn by analysing data. The more data we provide them, the better they are at making predictions and decisions.

Newer machine learning systems are far more superior than simple logic statements. These deep learning systems are inspired from human brains, which consists of interconnected neurons. As a general-purpose technology, machine learning can potentially be used in various fields, from politics to DNA sequencing.

## Scaled up data - Big Data
Towards the end of my explanation above about machine learning, I briefly mentioned about a couple of new concepts. The first one is about analysing data, where I was pointing towards the rise of big data.

Let me try to explain the concept with an example. I was working on a website where I released a new feature of feedback. During the peak season, we got feedback from thousands of customers, however, it was humanly impossible to read each feedback. By the time we could have digested the feedback collected, there was another wave of feedback. Now, this was a website which was operating with in a country. Consider worldwide web at global scale. We are talking about trillions of words, billions of images, and billions of hours of video and audio. Now the challenge was how can organisations get value from that data?  This is commonly called as __“big data”__ problem. It is like a you are in a middle of a rainstorm, but you have no containers to catch the water.

## Deep Learning: Learning from massive datasets
Towards the end of 2011, people began to talk about deep learning. It is a machine learning technique that uses deep layers of artificial neural network. Imagine a sandwich making factory with assembly line. Each layer of workers is responsible for a specific task: slicing bread, adding meat, adding salad, adding sauce and packaging. The layers do not know the final order, they simply build on the top of previous layer, until the sandwich is complete as per the customer. If a customer complains – “I asked for a chicken, not pork”, the chain works backward to find the flaw and retrains the workers. Over time, the assembly line gets better to consistently produce the perfect sandwich for any customer. 

That is deep learning in a nutshell. Each layer within a neural network treats a specific portion of the input, such as edges in an image, then shapes, and finally objects, based on knowledge passed on by the previous layer. Then, the last layer makes what is called a decision or prediction and may, for example, identify a cat in a photograph or translate a sentence in language A into language B.

If prediction is off and the system does not moves on from there. Instead, what it does is use backpropagation—like the complaints in our sandwich factory—to trace that error back through the layers. The network then modifies the internal weights (how much one "worker" listens to the others), thus retraining itself to perform better and produce the right output next time. Given enough data, deep networks can be remarkably good at things like speech recognition, fraud detection, medical diagnosis, and even generating text and art that feel human. Over time, these models become very accurate and capable.

At first, deep learning neural networks were used mostly to classify things. Later, in 2017, Google announced [AlphaGo](https://en.wikipedia.org/wiki/AlphaGo){:target="_blank"}, the system it created to play “Go” mentioned previously. AlphaGo could taught and learn itself to play and develop incredibly complex strategies to win. Since then, has continued to evolve, these deep learning algorithms are used to perform all kinds of predictive machine learning. You could classify customers based on their buying behaviour. Forecast inventory to predict when a product might sell out. And not even that, now we are heading towards self-driving cars. The cards that can “see” and analyse their surroundings to predict how to best drive. Artificial neural networks remain a dominant form of AI.[^fn-nth-2]

I will continue this discussion in part 2 of the article.


## References

[^footnote]: [Machine, Platform, Crowd Free Review by Andrew McAfee and Erik Brynjolfsson](https://books.google.co.uk/books/about/Machine_Platform_Crowd_Harnessing_Our_Di.html?id=zh1DDQAAQBAJ&redir_esc=y){:target="_blank"}
[^fn-nth-2]: [LeCun, Y., Bengio, Y. & Hinton, G. Deep learning. Nature 521, 436–444 (2015)](https://doi.org/10.1038/nature14539){:target="_blank"}
