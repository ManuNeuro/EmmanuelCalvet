---
layout: post
title: "Computing with a Bucket of Water: Why I Study Reservoir Computing"
date: 2021-09-11
image:	/assets/article_images/2023-05-15-article-submission/cover.png
---

*Thesis title: From circuit to dynamics and performance in tasks, how to design spiking neural network reservoirs without plasticity?*

Ph.D. student in Electrical and Computer Engineering (Université de Sherbrooke).

- Director: Jean Rouat ([NECOTIS](https://www.gegi.usherbrooke.ca/necotis/?lang=en))
- Co-director: Bertrand Reulet ([IQ](https://www.usherbrooke.ca/iq/en/))

## Introduction

I am a Ph.D. student at the intersection of three disciplines: computational neuroscience, artificial intelligence, and physics. My goal is to use physics concepts to better understand and design machine learning models.

I am working in the field of reservoir computing. It is a niche branch of artificial intelligence compared to ubiquitous deep learning and artificial neural networks (ANNs). Still, it is very promising, especially when considering the intrinsic limitations of these techniques: computational and energy cost.

Deep networks run on giant GPU clusters, which are extraordinarily costly in technology, power consumption [1], and ecological impact [2]. It is striking that our brains only cost 10 to 25 watts while being able to perform probably more than 10^15 floating-point operations per second [3]! In other words, we have vast room for improvement. Moreover, not only is energy scarce, but resources are too, while most of our digital tech relies on rare metals, primarily found in China's mines [4]. You have a pretty fragile chain of interdependence, subject to many issues ranging from political to climatic, and well, now we'd have to add sanitary to this non-exhaustive list!

In that context, I see reservoir computing as a serious competitor for building extremely cheap and potentially eco-friendly computational devices. You will understand why in a second, after I show you what it is!

## So how does it work?

1. Take a bucket of water.
2. Take a camera looking at the surface of the water.
3. Take a computer program analyzing the camera output.
4. Now drop things into the bucket...
5. Have fun!

![Taken from [5].](https://manuneuro.github.io/EmmanuelCalvet/assets/article_images/about/pic0.png)

Tadaa! There you are: you have a computational device, ready to classify various input data and capable of performing computation!

You don't believe me?! Check this out [6]. In this 2003 study, titled *Pattern Recognition in a Bucket*, the authors performed an XOR function with a simple bucket of water and, of course, a computer analyzing the wave patterns at the water's surface!

![Taken from [6].](https://manuneuro.github.io/EmmanuelCalvet/assets/article_images/about/pic1.png)

The trick is that you never "program" the water. The ripples already mix and transform the inputs in a rich, nonlinear way, and the only part you train is a simple readout that interprets what the camera sees. This is the core idea of reservoir computing, which emerged in the early 2000s under two names: echo state networks (Jaeger) and liquid state machines (Maass).

## AI or not AI, that is the question!

Right now, you should be wondering what on earth I am doing concretely. Am I playing with weird physical systems to perform computation? ...

![Physical systems used as an actual reservoir.](https://manuneuro.github.io/EmmanuelCalvet/assets/article_images/about/pic2.png)

Well, not really. Before everyone carries their computer inside a bottle of water, I'd like to say there is still a lot of ongoing improvement with neural networks. The next evolution in AI may well be spiking neural networks. No wonder, since they mimic real biological ones. They use current only during the short electrical discharge called an "action potential," while their favoured state is silence. The significant advantage is that information can be encoded in the time between spikes, the interval of silence between discharges. This is a drastic advantage over ANNs, which encode the rate of discharge in time, which means they require current at each time step!

Now I've given two, I believe good, reasons to study reservoir computing with spiking neural networks.

## The actual question I am asking

Before even creating a physical substrate for a reservoir, we decided to perform a theoretical and simulation-based study of how to design a good network of spiking neurons for different types of computational tasks.

My central questions are the following:

- How do we design a spiking neural network optimally tuned to a specific task, giving the best performance without any plasticity mechanism in the reservoir?
- And the corollary: how do we design a reservoir that is optimally tuned to solve many tasks, while not necessarily being the most accurate at any specific one?

### And how?

For those who are not familiar with AI, these questions can seem a little abstract, so let me explain what they mean and why they are pertinent in AI.

### 1. Design of spiking neural networks

First off, we must understand what is meant by designing a spiking neural network. There are many possible ways of seeing this, but I will narrow it down to two essential parts:

- You have an architecture, a directed graph, with nodes and edges. (We call them neurons and synapses.)
- You have weighted edges, also called synaptic weights, that act as amplification and/or inhibition of the input received from other neurons via the edges.

**The problem:** Most studies in reservoir computing generate circuits randomly. Many papers have studied the dynamics associated with architectures such as small-world and scale-free topologies. On the other hand, the impact of the initialization of weight matrices is still poorly understood. In fact, most weight matrices are initialized randomly, with very high variability in the results obtained!

*NB: Many more elements have to be considered, such as the equation of the neuron, the non-linearity, the threshold, and much more. To reduce complexity, I decided to use the simplest neural model, one that possesses none of these parameters: the **binary neural model**.*

### 2. Plasticity

Second, plasticity is everywhere in neural networks. It is the sacrosanct principle behind any classification and regression in deep learning, ANNs, LSTMs... This is a consequence of 40 years of thriving neuroscience, showing how everything is plastic *in vitro* and *in vivo*: circuitry, neurons, and synapses are constantly adapting.

**The problem:** One of the common problems in reservoir computing is the random search of the weight matrices. To avoid that, people tended to use learning rules. However, learning takes time, energy, and a lot of work to figure out which learning rules to apply with which parameters.

Yet reservoirs do not need learning if they are properly initialized. Using learning rules thus seems counterproductive, as it removes one of the reservoir's most exciting features: it is inherently computationally rich. In other words, you don't have to train a bucket of water!

According to a ground principle in physics, if you can reproduce a result with less complexity in your model or computational system, you have probably found a better one. Hence the question: is plasticity always necessary?

> Practically, can we design a reservoir of spiking neurons without the use of plasticity?

## What do I do?

My thesis will try to answer these questions by exploring many instantaneous weight matrix initializations and establishing a systematic link between circuit properties, dynamics, and performance in specific tasks.

## Where am I right now?

I am writing my first paper. We have decent results, and we are almost done with experiments. Soon enough, I will submit it for peer review!

---

**Update:** I wrote this post during my Ph.D., and the thesis is now complete. The results ended up in two papers: [the first](https://manuneuro.github.io/EmmanuelCalvet//2023/05/15/article-submission.html) and [the second](https://manuneuro.github.io/EmmanuelCalvet//reservoir/computing/2024/03/14/article-frontiers.html), both in *Frontiers in Computational Neuroscience*.

## References

[1] https://www.openphilanthropy.org/blog/new-report-brain-computation

[2] https://analyticsindiamag.com/deep-learning-costs-cloud-compute/

[3] https://towardsdatascience.com/compute-and-environmental-costs-of-deep-learning-83255fcabe3c

[4] https://www.azomining.com/Article.aspx?ArticleID=49

[5] Kohei Nakajima, Physical reservoir computing: an introductory perspective, 2020 Jpn. J. Appl. Phys. 59 060501

[6] C. Fernando and S. Sojakka, Pattern Recognition in a Bucket, Lecture Notes in Computer Science 2801 (Springer, Berlin, 2003), p. 588.
