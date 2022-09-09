---
title: "Whitepaper: an exploration of IA in crypto (Part 1)"
layout: post
categories: Quantum, Crypto
comments: true
image: /assets/article_images/2022-09-01-whitepaper-p1/whitepaper-cover.jpg
---

As I was reading the book: "Mastering Bitcoin" [1], and learning about the ECDSA algorithm, I was very captivated by the the elliptic curves (see picture below). I couldn't stop thinking: "I can do that with neural networks!"

![Image taken from bitcoin wiki.]({{ '/assets/article_images/2022-09-01-whitepaper-p1/pic0.png' | relative_url }})

So, I wanted to see what I could do, and allow myself to daydream a bit. As I explored this new territory, I decided I will use what I know, to try to do something that I don't, in a subject I've been interested recently: bitcoin.

***

For those who don't know the ECDSA algorithm, I am going to simplify it as much as I can. In bitcoin, it is an algorithm that creates a *public key*, from a *private key*. Your private key should never be known by anyone, otherwise you funds could be stolen! So when you are sharing your public key to receive some btcoins, this adress should not provide any hint, as to what is your private key. 

When you generate a public key, you need to design a deterministic function $F$, such that:

$$public\_key = F(private\_key)$$

This way you can always regenerate your public key. However, this function should not be inversible, meaning that $F^{-1}$ should not exist:

$$F^{-1}(public\_key) \neq private\_key$$

One solution adopted for Bitcoin, is the use of the elliptic curve. It renders impossible the task of finding the inverse, because it is a non-injective, surjective function. Even if you know the curveand try to map it back, to one public key always correspond several private keys (see picture below).

![You can imagine that a desirable public key (set Y) is C. Because then you can't know for sure if the private key (set X) is either 2 or 3. Image taken from wikipedia.]({{ '/assets/article_images/2022-09-01-whitepaper-p1/pic1.png' | relative_url }})

Now, there's technically no grounded reason why you would replace the elliptic curve by something else, as it is a pretty solid solution to the problem. But as I was reading the book, I couldn't help myself, I needed to try to design the function $F$ with a neural networks, just for the beauty of it. 

Okay what about you, you ask. Well their could be one or two reasons why this might be of interest for you too. Let's say I am going to present you artifical neural networks, in a non-trivial way, if you dare going down the rabit whole.

***


<center> Ready ? Follow the white-paper </center>
![Honestly, do I have to justify myself if I want to put a Morpheus picture into a scientic vulgarization article?! Image taken from the rabit hole of the internet; More seriously, I can't tell you the source, so I invite you to check it by yourself with TinEye.]({{ '/assets/article_images/2022-09-01-whitepaper-p1/matrix.png' | relative_url }})


***

To design such a function, I had to keep two important properties in mind:
- $F$ needs to be deterministic
- $F$ needs to be highly non-linear

For me, neural networks are all about non-linearities. Each neuron consist in applying a non-linear function to its inputs. From the picture below, we see that a neuron consist in a mathematical functon, $f(x, W)$, displayed as a step function. The neuron receives a sum of inputs, vector $x$, weighted by the matrix $W$. First thing, one carn clearly see that the function of this neuron is purely deterministic. This is good, because it is exactly what you want for the design of $F$. Second, the state of the neuron $y$ is binary, because below a certain value of $x$, it's going to gives $0$, while above this value, it's going to give $1$. This function insures that you have a binary output, while providing you non-inversible function. Because for the two possible output value $y\in\{ 0, 1 \}$, there is litteraly an infinite amount of possible values $x$! 

![The artificial neuron is a mathematical simplification of a biological neuron, composed of an activation function f (here we use the step function). The neuron receive a weighted (W) sum of inputs (x), such that it's output is y=f(x, W). Note that the action of the neuron is completely deterministic. Moreover, one neurone alone is also non-inversible. Image made by the author.]({{ '/assets/article_images/2022-09-01-whitepaper-p1/pic2.png' | relative_url }})


Now let's assume that each input (x1, x2, x3) of the neuron are in fact the output of three other neurons, themselves receiving other neuron's outputs as input. Eventually, we can add many layers of neurons in cascade to obtain a deep neural network. These neurons create an intricate and very complex successions of filters, which totally blurry the input space. Meaning, there is no chance to deduce the input:

At least in theory, one could imagine that with enough neurons and complexity in the network, the hyperspace -meaning the encoding space resulting of the aggregation of all these neurons- is virtually infinite. You could potentially create a mapping from any received input, to any output you want. Thus you could obtain a deterministic mapping of the two numbers, all the while keeping their resemblance to zero. Well in theory yes, and as we will see in practise it's a bit more complicated than one might expect. 

In general, the task of the Machine Learning (ML) expert, is in fact to *reduce* this hyperspace. He does that by finding the appropriate parameters $W$ of the model, such that the network outputs match closely the targets. Generally this involves minimizing the error produced by the network reconstruction, through a process called backpropagation. The adapating mechanism tries to find the most efficient way of storing the information in the hyperspace, by filtering out everything that is not related to the input statistical structure. Put simply, it tries to find correlations among the inputs. 

Interstingly, it is the exact opposite thing we need to do. 
> We want to design a system that creates an output pattern that is maximally uncorelated from it's input. 

Practically, we could frame the problem as if we wanted the network to "learn by heart" a silly list of gibberish and unintersting words. And who forgot how much difficult is this task?

In order to spice things up though, I am going to restrain myself by not using any ML technics. So no backprop., no minimization of the cost function, nothing adaptative, at all. 

No, to solve this problem, we are going to get our hands dirty, with some obscure percolation theory: aka critical phase transitions. 

***

<center> To be continued, ... </center>


