---
title: "White-paper: an exploration of IA in crypto (Part 1)"
layout: post
categories: Quantum, Crypto
comments: true
image: /assets/article_images/2022-09-01-whitepaper-p1/whitepaper-cover.jpg
---

As I was reading the book: "Mastering Bitcoin" [1] and learning about the ECDSA algorithm, I was very captivated by the elliptic curves (see picture below). I couldn't stop thinking: "I can do that with neural networks!"

![Image taken from bitcoin wiki.]({{ '/assets/article_images/2022-09-01-whitepaper-p1/pic0.png' | relative_url }})

So, I wanted to see what I could do and let myself daydream. As I explored this new territory, I decided I would use what I know to try to do something that I don't, in a subject I've been interested recently: bitcoin.

*NB: in the rest of the article, `bitcoin` will refer to the algorithmic implementation of the `Bitcoin`: the coin used as means of exchange.*

***

For those who don't know the ECDSA algorithm, I will simplify it as much as possible. In bitcoin, it is an algorithm that creates a *public key*, from a *private key*. No one should ever know your private key otherwise, your funds could be stolen! So when you are sharing your public key to receive some Bitcoins, this address should not provide any hint as to what is your private key. 

When you generate a public key, you need to design a deterministic function $F$, such that:

$$public\_key = F(private\_key)$$

This way, you can always regenerate your public key. However, this function should not be invertible, meaning that $F^{-1}$ should not exist:

$$F^{-1}(public\_key) \neq private\_key$$

One solution adopted for bitcoin is the use of the elliptic curve. It renders the task of finding the inverse impossible because it is a non-injective, surjective function. Even if you know the curve, one public key will map back to several private keys (see picture below).

![You can imagine that a desirable public key (set Y) is C. Because then you can't know for sure if the private key (set X) is either 2 or 3. Image taken from wikipedia.]({{ '/assets/article_images/2022-09-01-whitepaper-p1/pic1.png' | relative_url }})

There's technically no reason why you would replace the elliptic curve with something else, as it is a pretty solid solution to the problem. But as I was reading the book, I couldn't help myself; I must try to design the function $F$ with a neural network, just for the beauty of it. 

Okay, what about you, you ask. Well, there could be one or two reasons why this might interest you too. Let's say I will present artificial neural networks in a very non-orthodox way if you dare go down the rabbit hole.

***

![Machine Learning is everywhere Neo, it is in predatory journals, and it is in Nature's papers. It is on all your social media platforms. It is here when you watch cat videos. It is here when you work. It is here even when you find yourself alone at night. Do you want to know the truth, Neo? Machine Learning is the Matrix. Image taken from the rabbit hole of the internet; More seriously, I can't tell you the source, so I invite you to check it by yourself with TinEye.]({{ '/assets/article_images/2022-09-01-whitepaper-p1/matrix.png' | relative_url }})


***

# Neo: follow the white-paper, ...

To design such a function, I had to keep two essential properties in mind:
- $F$ needs to be deterministic
- $F$ needs to be highly non-linear

For me, neural networks are all about non-linearities. Each neuron consists in applying a non-linear function to its inputs. The picture below shows that a neuron consists of a mathematical functon, $f(x, W)$, here displayed as a step function. The neuron receives a weighted sum of inputs. As it is a mathematical function, this neuron is purely deterministic. This is good because it is exactly what you need to design $F$. Second, the state of the neuron $y$ is binary because below a specific value of $x$, it's going to give $0$, while above this value, it's going to give $1$. The output of this function is binary and, as such, non-invertible. Indeed, for the two possible output values $y\in\{ 0, 1 \}$, there is an infinite amount of possible values $x$! 

![The artificial neuron is a mathematical simplification of a biological neuron, composed of an activation function f (here, we use the step function). The neuron receives a weighted (W) sum of inputs (x), such that its output is y=f(x, W). Note that the action of the neuron is completely deterministic. Moreover, one neurone alone is also non-invertible. Image made by the author.]({{ '/assets/article_images/2022-09-01-whitepaper-p1/pic2.png' | relative_url }})


Let's assume that each input (x1, x2, x3) of the neuron is the output of three other neurons, themselves receiving other neurons' outputs as input. Eventually, we can add many layers of neurons in a cascade to obtain a deep neural network. These neurons create a complex succession of filters, blurring the input space. Hence, there is no chance to deduce the input:

At least, in theory, one could imagine that with enough neurons and complexity in the network, the hyperspace -meaning the encoding space resulting from the aggregation of all these neurons- is virtually infinite. You could potentially create a mapping from any received input to any output you want. Thus you could obtain a deterministic mapping of the two numbers while keeping their resemblance to zero. Well, in theory, yes, and as we will see in practise, it's a bit more complicated than one might expect. 

In general, the Machine Learning (ML) expert's task is to *reduce* this hyperspace. He does that by finding the appropriate parameters $W$ of the model, such that the network outputs closely match the targets. Typically, this involves minimizing the error produced by the network's reconstruction through a process called backpropagation. The adapting mechanism tries to find the most efficient way of storing the information in the hyperspace by filtering out everything that is not related to the statistical input structures. Put simply; it tries to find correlations among the inputs. 

Interestingly, it is the exact opposite thing we need to do here. 
> We want to design a system that creates an output pattern that is maximally uncorrelated from its input. 

Practically, we could frame the problem as if we wanted the network to "learn by heart" a silly list of gibberish and uninteresting words. And who forgot how much difficult is this task?

To spice things up, though, I will restrain myself by not using any ML techniques. So no backprop., no cost function minimization, and nothing adaptative at all. We are not taking the blue pill this time, sorry. 

To solve this problem, we will get our hands dirty, with some obscure percolation theory, by riding critical phase transitions to end into pure chaos, aka the red pill. 

***

<center> To be continued, ... </center>


