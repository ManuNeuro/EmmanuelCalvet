---
title: "Publication of my Second Article in Frontiers"
layout: post
categories: Reservoir Computing
comments: true
image: /assets/article_images/2024-03-14-article-publication/cover.png
---

Here it is, my [second publication](https://www.frontiersin.org/articles/10.3389/fncom.2024.1348138/full), yet again, in *Frontiers in Computational Neurosciences*, in collaboration with Jean Rouat and Bertrand Reulet. This one is the prolongation of the work initiated in my [first paper](./2023-05-15-article-submission.md).

Here is the scope of this manuscript:

Reservoir Computing (RC) is a compelling field in artificial intelligence, where only the output of the reservoir is trained, significantly reducing learning time and costs. Constructing cost-efficient neuromorphic chips or physical reservoirs using the Random Boolean Network (RBN) model could mark a significant stride towards more sustainable AI systems. This development is particularly pertinent considering the high costs associated with Large Language Models. While RBN may not directly replace LLMs, it certainly offers a promising direction for achieving lower energy consumption in AI devices. However, the lack of a unifying design theory necessitates empirical design recommendations to deduce reservoir design recipes. Our study contributes to understanding the relationship between these reservoirs' connectivity and topology parameters, by conducting extensive simulations, we provide clear guidelines for optimizing these systems for optimal computation, including memory and prediction of time series.

And here's the technical abstract:

Reservoir Computing (RC) is a paradigm in artificial intelligence where a recurrent neural network (RNN) is used to process temporal data, leveraging the inherent dynamical properties of the reservoir to perform complex computations. In the realm of RC, the excitatory-inhibitory balance $b$ has been shown to be pivotal for driving the dynamics and performance of Echo State Networks (ESN) and, more recently, Random Boolean Network (RBN). However, the relationship between $b$ and other parameters of the network is still poorly understood. This article explores how the interplay of the balance $b$, the connectivity degree $K$ (i.e., the number of synapses per neuron) and the size of the network (i.e., the number of neurons $N$) influences the dynamics and performance (memory and prediction) of an RBN reservoir. Our findings reveal that $K$ and $b$ are strongly tied in optimal reservoirs. Reservoirs with high $K$ have two optimal balances, one for globally inhibitory networks ($b < 0$), and the other one for excitatory networks ($b$ > 0). Both show asymmetric performances about a zero balance. In contrast, for moderate $K$, the optimal value being $K = 4$, best reservoirs are obtained when excitation and inhibition almost, but not exactly, balance each other. For almost all $K$, the influence of the size is such that increasing $N$ leads to better performance, even with very large values of $N$. Our investigation provides clear directions to generate optimal reservoirs or reservoirs with constraints on size or connectivity.

Now I am in the process of writing my thesis, and my PhD should end soon!
