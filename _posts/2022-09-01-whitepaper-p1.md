---
title: "Whitepaper: an exploration of IA in crypto (Part 1)"
layout: post
categories: Quantum, Crypto
comments: true
image: /assets/article_images/2022-09-01-whitepaper-p1/whitepaper-cover.jpg
---

As I was reading the book: "Mastering Bitcoin" [1], and seeing how the ECDSA algorithm is using the beautifull elliptic curves (see picture below), I couldn't stop thinking: "I can do that with neural networks!"

![Image taken from bitcoin wiki.]({{ '/assets/article_images/2022-09-01-whitepaper-p1/pic0.png' | relative_url }})

So, I wanted to see what I could do, and allow myself to daydream a bit. As I explored this new territory, I decided I will use what I know, to try and do something that I don't. A subject I've been interested in recently: bitcoin.

***

For those who don't know the ECDSA algorithm, I am going to simplify it as much as I can. In bitcoin, it is an algorithm that creates a public from a private key. Your private key should never be known by anyone, otherwise anyone could potentially access your funds, and steal from you. So when you are sharing your *public key* to receive some btcoins, this adress should not provide any hint, as to what is the private key. 

In other world, when you generate a public key, you need to design a deterministic function $F$, such that:

$$public\_key = F(private\_key)$$

But this function should not be inversible:

$$F^{-1}(public\_key) \neq private\_key$$

The use of elliptic curves render impossible the task of finding the inverse. This is because it is a non-injective, surjective function. Put simply, it means that to one public key, correspond several private keys (see picture below).

![Image taken from wikipedia.]({{ '/assets/article_images/2022-09-01-whitepaper-p1/pic1.png' | relative_url }})

Now, there's technically no grounded reason why you would replace the elliptic curve by something else, as is a pretty solid solution to that problem. But anyways, as I was reading the book, I couldn't help myself, I needed to try to desig this function $F$ with a neural networks, just for the beauty of it.

***

To design such a function, I had to keep two important properties in mind:
- $F$ needs to be deterministic
- $F$ needs to be highly non-linear

For me, neural networks are all about non-linearities. Each neuron consist in applying a non-linear function to its inputs. From the picture below, one carn clearly see that the function of a neuron is purely deterministic, which is what you want for the design of $F$. A given input will always give you the same output. However, if you take only one neuron, you can crack it open, as it is completely reversible. To one input, always correspond one input. 

Now let's say the each input (x1, x2, x3) of the previous figure, is itself the output a neuron. And eventually, put many layers of neurons in cascade, to obtain a deep neural network. Well these neurons now create an intricate and very complex successions of filters, which totally blurry your input space. Meaning, you can't deduce the input anymore.

![The artificial neuron is a mathematical simplification of a biological neuron, composed of an activation function f (here we use the step function). The neuron receive a weighted (W) sum of inputs (x), such that it's output is y=f(x, W). Note that the action of the neuron is completely deterministic. Moreover, one neurone alone is also inversible. Image from the author.]({{ '/assets/article_images/2022-09-01-whitepaper-p1/pic2.png' | relative_url }})

In theory, with enough neurons, you could potentially encode any received informations, no matter how complex it is. The hyperspace, meaning the encoding space resulting of the aggregation of all these neurons, is virtually infinite. 

In general, the task of the Machine Learning (ML) expert, is in fact to *reduce* this hyperspace. He does that by finding the appropriate parameters $W$ of the model, such that the network encode perfectly the inputs. Generally this involves minimizing the error commited by the network, through a process called backpropagation of the error, but this topic will be for another article. 

***





