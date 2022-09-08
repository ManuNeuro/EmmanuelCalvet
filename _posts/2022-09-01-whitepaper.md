---
title: "Whitepaper: an exploration of IA in crypto (Part 1)"
layout: post
categories: Quantum, Crypto
comments: true
image: /assets/article_images/2022-09-01-whitepaper-p1/whitepaper-cover.jpg
---

As I was reading the book: "Mastering Bitcoin" [1], and seeing how the hash256 are generated from beautifull elliptic curves (see picture below), I couldn't stop thinking: "I can do that with neural networks!"

![Image taken from bitcoin wiki.]({{ '/assets/article_images/2022-09-01-whitepaper/pic0.png' | relative_url }})

So, I wanted to see what I could do, and allow myself to daydream a bit. As I explored this new territory, I decided I will use what I know, to try and do something that I don't. A subject I've been interested in recently, the crypto world.

***

For those who don't know, the hash256 is an algorithm that create public from a private key in bitcoin. More specifically, it allow you to obtain a deterministic function F, such that 

$$public_key = F(private_key)$$

While the use of elliptic curve render impossible the task of finding the inverse 

$$F^{-1}$(public_key) = private_key$$

As it is a non-injective, surjective function. It means that the function in not inversible, as to one public key, might correspond several private key (see picture below).

![Image taken from wikipedia.]({{ '/assets/article_images/2022-09-01-whitepaper/pic1.png' | relative_url }})

Now, there's technically no grounded reason why you would replace elliptic curve with something else, as they are pretty solid. But anyways, for the design of such a function $F$:
- It needs to be deterministic
- It needs to be highly non-linear

In my mind, neural networks are all about non-linearities. Each neuron consist in applying a non-linear function to its inputs. Put them in cascade, as in deep neural network, and these neurons create an intricate and very complex sort of filtering of your input space. 

![The artificial neuron is a mathematical simplification of a biological neuron, composed of an activation function f (here we use the step function). The neuron receive a weighted (W) sum of inputs (x), such that it's output is y=f(x, W). Note that the action of the neuron is completely deterministic. Moreover, one neurone alone is also inversible. Image from the author.]({{ '/assets/article_images/2022-09-01-whitepaper/pic2.png' | relative_url }})

In theory, with enough neurons, you could potentially encode any received informations, no matter how complex it is. The hyperspace, meaning the encoding space resulting of the aggregation of all these neurons, is virtually infinite. 

In general, the task of the Machine Learning (ML) expert, is in fact to *reduce* this hyperspace. He does that by finding the appropriate parameters $W$ of the model, such that the network encode perfectly the inputs. Generally this involves minimizing the error commited by the network, through a process called backpropagation of the error, but this topic will be for another article. 

***





