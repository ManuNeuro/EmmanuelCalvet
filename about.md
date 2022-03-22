---
layout: page
title: About me
permalink: /about/
image: /assets/article_images/about/author_full.jpg
---

## Summary
*Thesis title* : From circuit to dynamics and performance in tasks, how to design spiking neural networks reservoir without plasticity ?

Ph.D student in Electrical and computing engineering (Université de Sherbrooke).

- Director : Jean Rouat (NECOTIS)
- Co-director : Bertrand Reulet (IQ)

## Introduction
I am a Ph.D. student at the intersection of three disciplines, namely computational neurosciences, artificial intelligence and physics. My goal is to use concepts from physics to better understand and design machine learning models.

I am working in the field of reservoir computing. It is a somewhat niche branch of artificial intelligence compared to the omnipresent deep learning and artificial networks, but it nonetheless very promising especially when having in mind the intrinsic limitations of these techniques : computational and energy cost.

Deep networks are run on giant GPU clusters, extremely costly both in terms of the technology it uses, its power consumption [1], and the ecological impact [2]. It is striking to see that our brains, only cost 10 to 25 watts, while being able to perform probably more than 10^15 flop operations [3] ! ... e.g., we have a huge room for improvement ! Moreover, not only energy is scarce, resources too are, while most of our digital techs rely on extremely rare metals, and mostly found in China's mine [4]. You have a pretty fragile interdependence chain, subject to many issues, ranging from political to climatic, and well, now we'd have to add sanitary to this non-exhaustive list !

In that context, I see reservoir computing as a serious concurrent for building extremely cheap and potentially ecofrendly computational devices, and you will understand why in a second, after I showed you what it is !

So let's see how it can work

1. Take a bucket of water.
2. Take a camera looking at the surface of the water.
3. Take a computer program analyzing the camera output.
4. Now drop things into the buckets, ...
5. Have fun !

![Taken from [5].](/assets/article_images/about/pic0.png)

Tadaa ! There you are, you have a computational device, ready to classify various input data, and capable of performing computation !

You don't believe me ?! Check this out [6], this is a 2003 study, actually they performed a XOR function with a simple bucket of water and, of course, a computer performing an analysis of the waves pattern at the surface of the water !

![Taken from [5].](/assets/article_images/about/pic1.png)

## AI or not AI, that is the question !
Right now you should be wondering what on earth am I doing concretely ? Am I playing with weird physical systems to actually perform computation ? ...

![Physical systems that have been successfully used as a reservoir to perform complex computation.](/assets/article_images/about/pic2.png)

Well, not really, and before everyone carries its computer within her or his bottle of water, I'd like to say there is still a lot of ongoing improvement with neural networks. The next evolution in AI is currently spiking neural networks. No wonder, they are mimicking real biological ones. They use current only during the short electrical discharge called "action potential,” while their favoured state is silence. The big advantage is that information can be encoded in the time between spikes, the interval of silence between discharges. This is a drastic advantage over the ANN that encodes for rate of discharge in time, which means they require current at each time step !

Now I've given two, I believe good reasons, to study reservoir computing with spiking neural networks.

## The actual real question I am asking
Well, in fact, before even creating a real physical substrate for a reservoir, we decided to perform a theoretical and simulation-based study of how to design a good reservoir of spiking neural networks, for different types of computational tasks.

My central questions are the following :

- How to design a spiking neural network that is optimally tuned to some specific tasks giving best performances, without any plasticity mechanism in the reservoir ?
- And the corollary, how to design a reservoir that would be optimally tuned to solve many tasks, while not necessarily being the most accurate in each respective tasks ?

### And how ?

So for those how are not familiar with AI, this question can seem a little bit abstract and I will explain to you what it means, and even how this question is relevant in AI :

### 1. Design of spiking neural networks
First off, we have to understand what is meant by designing a spiking neural network. There are many possible ways of seeing this, but I will narrow it down to two essential parts :

- You have an architecture, which is a directed graph, with nodes and edges. (We call them neurons and synapses.)
- You have weighted edges, also called synaptic weights, that act as amplification and or inhibitions of the input signals.

#### Problematic :

- Most if not all studies in the reservoir computing field generate circuits randomly, if a good lot of studies have shown the impact of the various architectures, such as small world, and scale-free topologies, few work has been focusing on the impact of the initialization of weights matrices.
- In fact, most weight matrices are initialized randomly with very high variability in obtained results !

NB : Now you would have many more things to take into account, like the equation of the neuron, the non-linearity, the threshold, and much more, but to reduce complexity, I decided to use the simplest neural model that possesses none of these parameters, the binary neural model).

### 2. Plasticity
Second, plasticity is everywhere in neural networks, it is the sacro-saint principle behind any classification and regression, in deep learning, ANN, etc, ... And, of course, the brain itself and it's 40 years of thriving neurosciences are showing how everything is plastic in the circuitry, neurons and synapses.

#### Problematic :

- Learning takes time, energy, and lot of work, to design and know what learning rule to apply, with which parameters, etc.
- People prefer to use learning update rules in reservoirs computing field in order to avoid the random search of these weights matrices, hence removing one of the initially really interesting features of these reservoirs, i.e., you don't have to train a bucket of water !

So, according to the ground principle in physics, if you can reproduce a result with less complexity in your model/computational system, then you'd probably have a better one. Hence the question, is plasticity always necessary ?

> Pratically, can we better design reservoir of spiking neurons without it ?

## What do I do ?
My thesis will try to answer to these questions by exploring many instantaneous weights matrices initialization, and giving a systematic link between circuit properties, dynamics, and performance in specific tasks.

## Where am I right now ?
I am writing my first paper. We have good result and we are almost done with experiments, and soon enough I will submit it for peer reviewing !

## References
[1] https://www.openphilanthropy.org/blog/new-report-brain-computation

[2] https://analyticsindiamag.com/deep-learning-costs-cloud-compute/

[3] https://towardsdatascience.com/compute-and-environmental-costs-of-deep-learning-83255fcabe3c

[4] https://www.azomining.com/Article.aspx?ArticleID=49

[5] Kohei Nakajima, Physical reservoir computing—an introductory perspective, 2020 Jpn. J. Appl. Phys. 59 060501

[6] C. Fernando and S. Sojakka, Lecture Notes in Computer Science 2801 (Springer, Berlin, 2003), p. 588.

