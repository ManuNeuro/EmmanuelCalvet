---
title: "White-paper: Hash Function with Chaotic Artificial Neural networks  (Part 3)"
layout: post
categories: Quantum, Crypto
comments: true
image: /assets/article_images/2022-09-24-whitepaper-p3/whitepaper-cover.jpg
---

# 

# Hash Function with a Chaotic Neural Network

Hi again everyone, I am glad you are here, because in this article we will dive further into an amazing topic: chaotic cryptology.

Since this is the third article of the series on a potential Whitepaper in crypto, I really encourage you to read the first two articles on that subject:

- Part1: [From ellipitic curve, to neural networks]()
- Part2: [Artificial neural networks and percolation]()

In any case, I'm going to make a short recap of what we did in the previous articles.

## Hey, can you recap a bit for me?

Previously, we built a neural network, and identified a regime above the percolation threshold, that gave us this nice phase transition, coming from quiscent to alive. 

![The architecture of an ANN, you can specify the size of the input and output layers, as well as the number of hidden layers and their respective sizes.]({{ '/assets/article_images/2022-09-01-whitepaper-p1/ann.jpg' | relative_url }})

The picture above was obtained by playing with the variance of the weight distribution randomly generated. Each dot represented the average output of the network, over 10 realizations of the same distribution parameters with different seeds. Now in conclusion of the last article, I said that the noise we observe is due to the sensitivity on initial conditions, a.k.a. **chaos**, which is exactly what we need to accomplish our goal.

A bit from it, and mostly this is it.

## Sorry, what was our goal again?

Alright, since I am so kind, I'm going to quickly summarize what was our motivation:

We wanted to design a function $F$, that could produce a deterministic input/output, relationship, for the creation of a $public\_key$, given a $private\_key$:

$$public\_key=F(private\_key)$$

But we can't just choose any function, because we want it to be secure, so we want it to be nearly impossible to find the inverse mapping $F^{-1}$:

$$F^{-1}(private\_key) \neq public\_key$$

Now you remember that we decided to use an Artificial Neural Network (ANN) to encode $F$, so basically we choose $F_{ANN}$ instead of say, a typical function in cryptography the $SHA256$.

Now, there are many more considerations to take into account for the design of $F$, such as *confusion* and *diffusion*, but essentially, this was our starting point, and rest assure that we will get to them as we study artificial neural network in more depth. 

So, shall we start now?

### Artificial neural network

Alright let's jump right in now. One thing that I didn't mention in the previous article is the structure of the bit stream $Y$ we get at the output of the ANN, depending on the the weights $W$ and input $X$. So recall that we took the binary input vector $X$, and submit it in the input layer such that we obtain $Y=F_{ANN}(X)$. In the plot above, we measured $\langle Y \rangle$ the average overall output neurons. This just gave us a sense of how much neurons were activated, but it doesn't say anything about the usefulness of the this system for encryption, nor with the accurate termonlogy: the *confusion*.

To obtain a good *confusion*, the output should gives no clue whatsoever about structure of the input. Generally it does it in a way that each bit of the output is a composition of the whole input sequence, rather than specific part of it, hiding any relationships between the two bit sequences. 

Interestingly, and for the sake of clarity I will plot below an image of the ANN architecture we were using previously, ANN are feedforward architecture are extremely good candidates for such feat, since this, by design exactly what they are doing, composing, layer after layer, each of the inputs, recursively. 

In a way, we could see each layer of the ANN as a function $F_l$ with given layer index $l$. Then we could rewrite the equation of $F_{ANN}$ as:

$$Y=F_{ANN}(X)= F_{100}(F_{99}(...F_2(F_1(X))...))$$

Which is a sucession of composition of function ultimately all acting on $X$. This is nice, but this alone is not sufficient to generate a good confusion, since of course it is dependant on what $F_l$ is actually doing. Speaking of which, remember that these functions also takes another parameter, the weights at each layer $W_l$. Now as always in machine learning, this is exactly what we need to fine tune, but this time, not using machine learning per say, because this by thinking a bit we can an elegant solution. 

## Confusion

To be able to evaluate if our ANN performs well, we need something to measure the quality of the *confusion*, which shows how well the output is going to supress any types of patterns that could exist in the input signal. To do so, the optimal output should actually looks like noise, no structure at all, so that it doesn't give any information about the input. 

To do so, we will analyze the structure of said output $Y$ of our ANN's. One typical metric for analyzing binary string is the entropy, but as we will see in a second, this is not the best way. Instead we are going to use a very nice metric, the binary entropy $H_b$ [], or BiEntropy of bit streams. This function has very interesting features, that distinguished it from the well known Shanon entropy. So let's get ourselves some intuition of what it does. Let's say we obtain in output the two following 16-bit sequences:

$$X_1 = 0101010101010101$$
$$X_2 = 0110101001100101$$

If you were to compute the probability of obtaining a 1, and 0, in these two sequences, they would be the same, $P(1)=0.5$. This means that, the Shanon entropy of both sequences are the same $H(X_1)=H(X_2)=$. Intuitively, we rapidly feel that this is a problem, because these two sequences are not the same at all. One is purely periodic ($X_1$), and the other is rather difficult to predict ($X_2$). Fortunately, smart people worked on that issue, and proposed a nice way of differentiating these two sentences, using you guessed it, the binary entropy. Now it would be too long to explain how it is computed, so I am just going to give you the Binary entropy of these two bit sequences:

$$H_b(X_1)=0$$
$$H_b(X_2)=0.75$$

Okay, this comes handy, because now we can differentiate between ordered and disoreded bit sequences. One very nice property of $H_b$ is that is comprised in $[0, 1]$: giving 0 for fully periodic signals, and 1 for totally disored ones. 

Now why does it matter ? Well, remember, we need something to measure the quality of the *confusion*. To avoid giving away information about the input, the optimal output be as close as possible to noise. With $H_b$, the higher the disoreder, the closer to one, the better. This is why I propose to use this metric as a measure of performance for the confusion. 

## Chaos

The picture below shows the same phase transition as the plot in the [previous article](), but this time, as captured by the BiEntropy of the output. For each value of $\sigma(W)$ we ran 100 networks with different seeds. First, we see that the average of BiEntropy coincides perfectly with the average activity, while we transition from a quiescent and fully ordered regime with both $\langle Y \rangle$ and $\langle H_b\rangle$ equal to zero. The percolation threshold $\sigma_p$ as we called it in the previous article, is the point in parameter space $\sigma(W)$, where activity starts rising above zero. In our case it is around $\sigma_p \sim 1.7$. For values far higher than $\sigma_p$ we obtain a regime of chaos, where outputs are extremely disordered $\langle H_b\rangle \sim 1$.

![image]({{ '/assets/article_images/2024-04-01-whitepaper-p3/bientropy.png' | relative_url }})

<center>
The statistics of the BiEntropy $H_b$ versus the standard deviation of weights $\sigma(W)$. The BiEntropy is computed on the output $Y$ of the ANNs. (upper) the average over 100 networks output generated with different seeds. (lower) the variance over the same 100 networks; Image generated by the author.
</center>

Interstingly, when we look at the variance of BiEntropy, we see a sharp spike, centered around the transition from order to disorder. This transition marks a regime where we obtain a variety of outputs structures, composed of a mixture of order and disorder. This regime is also known as the critical regime, and in the context of neural computation, it has been shown to be the perfect spot for memory [], seperability [], and information transmission []. 

However, in our context of encryption, this is the worst spot one can choose, as it is going to improve the mutual information betwen input and output []. In the quiscent and chaotic regime however, all outputs structures are the same, as indicated by a variance of zero. Hence it is now very trivial to decide the region where we should choose the weight statistics: the one that both maximize the average of H_b, and minimizing its variance. 

### Okay what is happening here ?

Remember we started with a distribution that has a negative mean, $\mu(W)=-0.5$. As such, for low values of $\sigma(W)$ only negative weights exist. Now as you increase the variance of $\sigma(W)$ you recruit more and more excitatory synapses, until you actually get a perfect balance between negative and positive weights, when $\sigma(W) \rightarrow \infty$. In turns, for high weights variance you have a competition between excitation and inhibition of random weights, projcted a in cascades of non-linear activation functions for each hundred neurons and a hundred layers, hence chaos. 

### What's next ?

Now, the last element that we must analyze is the symmetry of 1's and 0's in the output. Ideally we would like our output word to be composed of 50% of ones, and 0, such to avoid bias in any direction, that could otherwise give hints as to the internal frabric of our hash function. This can be achived again by looking at the plot of output average $\langle Y \rangle$ as a function of $\sigma(W)$. As one can see, as $\sigma(W) \rightarrow \infty$, the average output seems to converge towards a given value. This value, is in fact $0.5$. This is because, only when a perfect balance between excitation and inhibition is obtained, one can have a perfect symetry of ones and zeros. In fact, we can go further by stating that the ratio of positive and negative weights controls the ratio of ones and zeros in the output. 

In theory then, our best parameter is obtained when $\sigma(W) \rightarrow \infty$, which is, well, *problematic*. In practice, however, there is a much simpler way to obtain a symetric distribution, and you probably guessed it already: $\mu(W)=0$. This is obvious since the gaussian distribution is symetric itself, so when the mean is zero, we have the same amount of positive and negative weights on both side. 

In fact, we can show that the case $\mu(W)=0$ and $\sigma(W) \rightarrow \infty$ are strictly equal. We do that empirically on the next (upper) plot:

![image]({{ '/assets/article_images/2024-04-01-whitepaper-p3/diffusion.png' | relative_url }})

<center>
The statistics of the BiEntropy $H_b$ versus the standard deviation of weights $\sigma(W)$. The BiEntropy is computed on the output $Y$ of the ANNs. (upper) the average over 100 networks output generated with different seeds. (lower) the average over 100 networks of the hamming distance between two outputs, as the inputs have exactly one bit which is flipped; Image generated by the author.
</center>

To do so, this time we increased the range of $\sigma(W)$ up to $10^{10}$! The green dashed line represents the values obtained with $\mu(W)=0$, and as you can see, it fit perfectly with the asymptotic values obtained above $\sigma(W)>10^6$. 

## Diffusion

### The Hamming distance

Now, the reader who is familiar with this stuff might already object: this is not enough to characterize confusion. We also need to study the *diffusion*, which is the relationship between the input and output, to answer the question: are there any correlations, betweem $X$ and $Y$? To do so, another metric that we will use is the trivial *Hamming distance* between the two bit streams:

$$D(X, Y) = 1/N \sum_i mod(X_i+Y_i, 2)$$

First, $N$ is the length of the bit stream. Second, remark that inside the sum, $X_i$ and $Y_i$ are binary $\{0, 1\}$, the modulo $2$ will then get you zero each time they are equal, and one when they are different. In turns, $D$ is simply just counting the number of time we have a diffrent bit in the two sequences. This is the simplest measure one can think of to check the difference between bit sequences. Since $D$ is normalized, it will get zero when the two sequences are the NOT of the other, and it will give one, when they are equal. On the other hand, the target goal for uncorrelated sequence is $0.5$. If you think about randomly throwing two binary coin, there's a 50% chance that each coin will be 1, or 0, so equally, there's a 50% chance that they will have the same value. 

### Input and output

Now in the previous picture (lower panel), we computed $\langle D(input, output)\rangle$, the average over a hundred trials, of the hamming distance between the input and output of the neural network. Here, for illustration, the input is a fully regular vector filled with only zeros, and we compare this, for values of the control parameter close to the asymptote. As one can see, below $10^2$, the rapidly decreases, which means there some correlation still exist between the input and output. Now the asymptot is nicely poised very close to 50%, which is what you want for a perfectly random relationhsip. Intruigingly, the hamming distance shows that the fully chaotic neural network (solid green line) have some small bias towards flipping a bit more that 50% of the bits. One could make the case that the most desirable value for the parameter is $\sigma$ a bit above $10^2$, which seems closer to $0.5$. And another potentially interesting feature is that you will have a variance there, and not this fixed and predictible value of $0.53$, which could potentially be harnessed by a malicious actor. 

### Flipping bits

The diffusion between input and output is not sufficient to prove that the hash function is solid, we also need to check that there is no relationship between various outputs that could be exploited. 
So, we make the following experiment, we randomly select one bit that we will flip, to see how the two outputs are correlated. For a perfect collision score, we would again have an average hamming distance of $0.5$, if it's more, or less, it means there might be some correlation between the input and output.

In machine learning, we train the neural networks so that they are robust to such changes, they will be able to filter out the perturbation and get to the core pattern underlying the signal. This is this property that make them interesting for classification for instance. They can recognize the underlying structure nonobstant the variations around it. But here, we want to do exactly the opposite task! A single bit flip should render an output that is totally uncorrelated from the initial one. In other word, we want to build the neural network in such a way that it becomes a detector of change in the signal, where any change would give a completely different class. 

Interestingly, chaos is very fit to the task, because of its core property: *the sensitivity to initial conditions*. In chaotic systems, changing just a tiny bit of the input can create a dramatic effect in the output. This is because of the phenomenon of avalanches, or cascading effect, in a space that is very complex. A small change, iterated a few times, is massively amplified, resulting in a completely different output. 

![image]({{ '/assets/article_images/2024-04-01-whitepaper-p3/bitflip.png' | relative_url }})

<center>
The average over 100 networks of the hamming distance $D$ versus the standard deviation of weights $\sigma(W)$. (upper) The hamming distance is computed on the input and the output $Y$ of the ANNs (lower) The hamming distance between two outputs, as the inputs have exactly one bit flipped; Image generated by the author.
</center>

In the picture above, here again we show the phase transition, but this time, from the perspective of the diffusion. In the upper shows the diffusion happening between the input and output. And the lower panel shows what is happening when we check the correlation between two output with exactly one bit flip in input. 

Yet again, this is very interesting to note that the phase transition pushes the same neural network, from a regime where the output is an attractor that pull the activity towards the same value, even when we flip the input, while increasing the  value of the control parameter pushes the network into a more and more chaotic regime, where the single bit difference creates more and more of an impact on the output, and repel the output from it's original attractor. 

## What about the 256SHA function ?

Okay, so this analysis wouldn't be complete without a proper comparison to at least one of the standard hash function. For this test I'll used the pretty well known SHA256 function, you can find it in the standard library of python. 

So, this time we compare the hash function diffusion against the one of a very chaotic artificial neural network. Since my tests showed a very similar BiEntropy value, close to 9.8 with the SHA256, I wanted to show a more interesting analysis, by making an a comparison with more bit flips, and along the way analyze the neural network diffusion when I choose a close to optimal parameter $\sigma=10^3$. 

![image]({{ '/assets/article_images/2024-04-01-whitepaper-p3/comparison.jpg' | relative_url }})

<center>
The average over 100 networks of the hamming distance $D$ versus the number of bit flip in the input, for the ANN (blue line) and the SHA256 (orange line). Variance is not shown, but is around 0.001 for both hash function.
</center>

It's very interesting to witness that the ANN and SHA256 performs very similarly. In fact, it's quite impressive that the neural network gives the same hamming distance for 1 bit flip, and 50, and we can conclude that the number of bit flip has no influence whatsover on the hamming distance, which is what you want for the ANN to be considered a candidate for a Hash function. Keep in mind that this is only true because we carefully selected the weights distribution to elicit chaos. Logically, for less chaotic networks, you will see that for 1 to 10 bit flips the hamming distance tends to be very low, and it will increase as the number of flip increases.

### Conclusion

We explored the potential of ANN to be used as Hash functions. We conduced a collision and diffusion test, in order to check the validity of our neural network as a potential candidate for a hash function. These tests proved to be conclusive, especially regarding the last comparison with the SHA256 function, however, these are far from enough to state that our neural network is sufficient. Many more tested could be conducted, along with mathematical proof that should be worked on. For example, we could conduce a collision attack, to check how many often it's possible that a different input leads to the same ouput. This is a typical attack that allows the hacker to identify malicious code as being the legitiame. 

At first, it might seem surprising that these AI algorithms could be use for this seemingly unrelated task. However, keep in mind that the Hash function is just a map with very complex duffusion processes iterated many times, which is exactly what neural networks are, as we show their mathematical operations. 

On the other hand, if you where to build your hash with a neural network, there is one interesting thing you can harness: the random genweration of synpatic weights. You can in fact generate random weights for each specific task you want, which make it even more difficult to guess how the neural network was built. For instance, in the next article we will build a package for generating a pair of private and public key, and we will use the private key, as a seed for generating the weigths. Changing the weights for each usecase create a space of parameter which is so big, that even a quantum computer could not guess it.  
