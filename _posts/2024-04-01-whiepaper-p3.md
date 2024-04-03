---
title: "White-paper: Hash Function with Chaotic Artificial Neural networks  (Part 3)"
layout: post
categories: Quantum, Crypto
comments: true
image: /assets/article_images/2022-09-24-whitepaper-p2/whitepaper-cover.jpg
---
 

# Hash Function with a Chaotic Neural Network

Hi again, everyone! I'm thrilled you're here because we're about to dive deeper into an incredible topic: chaotic cryptology.

Since this is the third article in our series potentially leading to a whitepaper in cryptography, I strongly encourage you to read the first two articles on this subject:

- Part1: [From ellipitic curve to neural networks](https://manuneuro.github.io/EmmanuelCalvet//quantum,/crypto/2022/09/01/whitepaper-p1.html)
- Part2: [Artificial neural networks and percolation](https://manuneuro.github.io/EmmanuelCalvet//quantum,/crypto/2022/09/24/whitepaper-p2.html)

However, let's do a quick recap of our journey so far.

## A Quick Recap, Please?

In our previous discussions, we constructed a neural network with with an input layer $X$, a hundred hidden layers, and an output layer $Y$ (see picture below).

![The architecture of an ANN, you can specify the size of the input and output layers, as well as the number of hidden layers and their respective sizes.]({{ '/assets/article_images/2022-09-24-whitepaper-p2/ann.jpg' | relative_url }})

By playing with the weights connecting neurons, we identified a regime above the percolation threshold, which led us to observe a fascinating phase transition from quiescent to active states:

![We plot the output of the neural network as a fonction of the control parameter, which is the standard deviation of the weight distribution. The mean of the weights is -0.5, so for a low variance, the ouput only gives zero, since there is only negative weights in the network. As the variance increases, more and more weights becomes positive, and the output starts to have more and more active neurons]({{ '/assets/article_images/2022-09-24-whitepaper-p2/output.png' | relative_url }})

The image above was produced by manipulating the variance of a randomly generated weight distribution. Each dot represents the average output of the network over 10 realizations with different seeds but the same distribution parameters. Concluding our last article, I mentioned that the noise we observed is attributable to sensitivity to initial conditions, a phenomenon known as **chaos**. This chaos is precisely what we aim to harness for our objectives.

"And what was that objective again?"

## Refreshing Our Goal

To refresh our memories kindly, our goal was to design a function $F$ that could establish a deterministic input/output relationship for creating a $public\_key$ from a given $private\_key$:

$$public\_key = F(private\_key)$$

However, we couldn't settle for just any function because we needed one that would be secure, meaning it should be nearly impossible to find the inverse mapping $F^{-1}$:

$$F^{-1}(private\_key) \neq public\_key$$

We decided to use an Artificial Neural Network (ANN) to encode $F$, opting for $F_{ANN}$ over traditional cryptographic functions like $SHA256$.

Essentially, this was our starting point, and now, there are many more considerations to take into account for the design of $F$, in order to make the encryption process more secure by making it harder to predict or reverse-engineer. Essentially we will talk about two key elements, the *confusion*, which tries to hides any relation and *diffusion* and rest assure that we will get to them as we study artificial neural network in more depth. In the following, we will define both terms, and our related objectives for the design of the neural network:

- **Confusion** is property that makes the relationship between the input key and the ciphertext as complex as possible. 
	- In other word **confusion** would be achieved by ensuring that the relationship between the input (e.g., a private key) and the output (e.g., a public key or hash) is highly nonlinear and sensitive to initial conditions, making it difficult to reverse-engineer the input from the output.
- **Diffusion**, is the property that ensures that the influence of one plaintext symbol is spread over many ciphertext symbols, disguising the statistical properties of the plaintext.
	- **Diffusion** would be implemented by designing the network such that small changes to the input (or changes in one part of the input) lead to significant and widespread changes in the output, ensuring that the output does not reveal patterns or structures of the input.

Shall we begin?

### Artificial Neural Network and the Essence of Confusion

In our journey through the realm of chaotic cryptology, one pivotal aspect we've yet to fully explore is the intricate structure of the bit stream $Y$, which emerges from the ANN based on the weights $W$ and the input $X$. Recall that we introduced a binary input vector $X$ into the input layer, aiming to achieve an output $Y=F_{ANN}(X)$. Previously, we visualized $\langle Y \rangle$, the average output across all neurons, to gauge the level of neuron activation. However, this measure alone doesn't illuminate the system's potential for encryption, particularly regarding the critical concept of *confusion*.

#### Achieving Optimal Confusion

For a cryptographic system to be effective, its output must obscure any discernible structure of the input. Ideally, each bit of the output is an amalgamation of the entire input sequence, rather than being attributable to specific segments of it. This obfuscation ensures that the relationship between the two bit sequences remains concealed.

Feedforward architectures of ANNs are particularly adept at this task. By design, they integrate inputs layer by layer, recursively, which aligns perfectly with our objective. To clarify, consider the following diagram of the ANN architecture we've been discussing:

![]({{ '/assets/article_images/2024-04-01-whitepaper-p3/ann_layers.png' | relative_url }})

In essence, we can view each layer of the ANN as executing a specific function $F_l$ with a given layer index $l$. This allows us to express the operation of $F_{ANN}$ as a succession of function compositions:

$$
Y=F_{ANN}(X)= F_{100}(F_{99}(...F_2(F_1(X))...))
$$

This representation underscores the layered complexity inherent in ANNs, which is fundamental to generating effective confusion. However, the efficacy of this approach is contingent upon the specific operations performed by each function $F_l$, which are, in turn, influenced by the weights $W_l$ at each layer.

#### Looking Ahead

As we venture further into the application of ANNs in cryptography, our next steps will involve a deeper examination of the functions $F_l$ and the strategic tuning of weights $W_l$. This exploration will be pivotal in harnessing the full potential of ANNs for cryptographic purposes, ensuring that our approach to creating confusion is not just theoretically sound but practically viable.

#### Tuning Weights for Cryptographic Elegance

The tuning of weights $W_l$ is a critical aspect of machine learning. Yet, in our context, we seek not just to optimize these weights for predictive accuracy but to sculpt them in a manner that maximizes cryptographic security. This endeavor requires a departure from conventional machine learning methodologies, prompting us to consider more nuanced, perhaps even theoretical, approaches to weight adjustment.

### Evaluating Confusion in ANNs

To accurately assess the effectiveness of our Artificial Neural Network (ANN) in achieving cryptographic confusion, we require a robust method for measuring how well the output conceals any patterns present in the input signal. Ideally, the output should resemble pure noise, devoid of any discernible structure, thereby ensuring that it reveals nothing about the input.

#### The Limitation of Traditional Entropy

A common approach to analyzing the structure of binary sequences is through entropy, specifically Shannon entropy. However, for our purposes, Shannon entropy may not provide the full picture. Consider two 16-bit sequences:

$$
X_1 = 0101010101010101
$$

$$
X_2 = 0110101001100101
$$

At a glance, both sequences have an equal probability of producing a '1' or a '0', leading to the same Shannon entropy value. Yet, intuitively, we recognize a significant difference between the two: $X_1$ is perfectly periodic, while $X_2$ appears more random and unpredictable.

#### Introducing Binary Entropy ($H_b$)

To address this discrepancy and more accurately measure the quality of confusion, we turn to binary entropy ($H_b$), or BiEntropy, a metric specifically designed to evaluate bit streams. Unlike Shannon entropy, binary entropy excels at distinguishing between ordered and disordered sequences. Let's consider the binary entropy values for our example sequences:

$$
H_b(X_1) = 0
$$

$$
H_b(X_2) = 0.75
$$

These values reveal that binary entropy can effectively differentiate between periodic and unpredictable sequences, assigning a value of 0 to fully periodic signals and a value closer to 1 for highly disordered ones. Now you might wonder how this works, and without being too technical, the Bientropy roughly computes the normalized entropy of the $n^th$ derivative of the binary signal, in other word, it quantifies how much predictible is the regularity of the variously spaced patterns in the signal. 

#### The Significance of $H_b$ for Measuring Confusion

The beauty of $H_b$ lies in its range of $[0, 1]$, where 0 indicates complete order and 1 signifies total disorder. This characteristic makes it an ideal metric for evaluating the *confusion* of an ANN's output. To prevent the leakage of information about the input, the output should be as indistinguishable from noise as possible. Thus, a higher $H_b$ value, indicating greater disorder, correlates with more effective confusion.

By adopting $H_b$ as our performance metric, we can more accurately gauge the ANN's ability to obfuscate the input signal, ensuring that the output provides no clues about the original data. This approach not only enhances our understanding of the ANN's cryptographic robustness but also guides us in fine-tuning the network to maximize security.

## The Role of Chaos in Cryptographic Security

In our exploration of artificial neural networks (ANNs) for encryption, understanding the transition from order to chaos—captured through the lens of BiEntropy ($H_b$)—is key. This section delves into how varying the standard deviation of weights, $\sigma(W)$, influences ANNs' output behavior, crucial for optimizing cryptographic security.

The picture below shows the same phase transition as the plot in the [previous article](https://manuneuro.github.io/EmmanuelCalvet//quantum,/crypto/2022/09/24/whitepaper-p2.html), but this time, as captured by the BiEntropy of the output. For each value of $\sigma(W)$ we ran 100 networks with different seeds. 

![]({{ '/assets/article_images/2024-04-01-whitepaper-p3/bientropy.png' | relative_url }})
<center>
The statistics of the BiEntropy $H_b$ versus the standard deviation of weights $\sigma(W)$. The BiEntropy is computed on the output $Y$ of the ANNs. (upper) the average over 100 networks output generated with different seeds. (lower) the variance over the same 100 networks; Image generated by the author.
</center>

### Phase transition, from order to chaos

First, we see that the average of BiEntropy (upper panel) coincides perfectly with the average activity showed in the recap section. The graph reveals a critical phase transition at the percolation threshold, $\sigma_p \sim 1.7$. Below $\sigma_p$, outputs remain ordered with minimal activity While we transition from a quiescent and fully ordered regime with both $\langle Y \rangle=0$ and $\langle H_b\rangle=0$. As $\sigma(W)$ crosses $\sigma_p$, the system enters a chaotic regime, characterized by highly disordered outputs ($\langle H_b\rangle \sim 1$), ideal for encryption due to the maximized confusion and minimized predictability.

### The Critical Regime: A Double-Edged Sword

The spike in theBiEntropy variance (lower panel) around $\sigma_p$ marks a "critical regime," where outputs blend order and disorder. This regime offers potential for neural computation, enhancing memory, separability, and information transmission. However, for encryption, this variability increases mutual information between input and output, potentially compromising security by making the encryption more predictable under certain conditions.

### Optimizing Weight Selection for Encryption

To leverage ANNs for robust cryptographic applications, selecting weight statistics that maximize $H_b$ while minimizing its variance is essential. This strategy ensures outputs remain as disordered as possible—enhancing security—while maintaining consistency across network initializations to reduce predictability.

### Okay what is happening here ?

Remember that we started with a distribution that has a negative mean, $\mu(W)=-0.5$. This initial setting ensures that, at lower values of $\sigma(W)$, the network is predominantly governed by inhibitory synapses, leading to a relatively ordered and predictable output.

As we incrementally increase the variance $\sigma(W)$, the landscape of the network begins to transform. This increase gradually introduces more excitatory synapses into the mix, creating a richer, more complex interplay between excitatory and inhibitory forces within the network. Now as you increase the variance of $\sigma(W)$ you recruit more and more excitatory synapses, the pivotal moment occurs when the variance reaches a level where a perfect balance between negative and positive weights is achieved, marking the threshold beyond which the system begins to exhibit chaotic behavior.

This is not magic, initially, the variance is negative and centered around -0.5, initially the distribution is more on the negative side. Now when the variance of $\sigma(W)$ increases, there's point where the distribution go above zero, and you recruit more and more excitatory synapses. When $\sigma(W) \rightarrow \infty$ it is reaching as far in the negative and positive, and it end up symetric around zero instead of the actual mean, which become insigniciant! In turns, for high weights variance you have a competition between excitation and inhibition of random weights, projcted a in cascades of non-linear activation functions for hundred of neurons in a hundred of layers, hence chaos. 


### What's next ?

Now, the last element that we must analyze is the symmetry of 1's and 0's in the output. Ideally we would like our output word to be composed of 50% of ones, and 0, such to avoid bias in any direction, that could otherwise give hints as to the internal frabric of our hash function. This can be achived again by looking at the plot of output average $\langle Y \rangle$ as a function of $\sigma(W)$. As one can see, as $\sigma(W) \rightarrow \infty$, the average output seems to converge towards a given value. This value, is in fact $0.5$. This is because, only when a perfect balance between excitation and inhibition is obtained, one can have a perfect symetry of ones and zeros. In fact, we can go further by stating that the ratio of positive and negative weights controls the ratio of ones and zeros in the output. 

In theory then, our best parameter is obtained when $\sigma(W) \rightarrow \infty$, which is, well, *problematic*. In practice, however, there is a much simpler way to obtain a symetric distribution, and you probably guessed it already: $\mu(W)=0$. This is obvious since the gaussian distribution is symetric itself, so when the mean is zero, we have the same amount of positive and negative weights on both side. 

In fact, we can show that the case $\mu(W)=0$ and $\sigma(W) \rightarrow \infty$ are strictly equal. We do that empirically on the next (upper) plot:

![]({{ '/assets/article_images/2024-04-01-whitepaper-p3/diffusion.png' | relative_url }})
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

![]({{ '/assets/article_images/2024-04-01-whitepaper-p3/bitflip.png' | relative_url }})
<center>
The average over 100 networks of the hamming distance $D$ versus the standard deviation of weights $\sigma(W)$. (upper) The hamming distance is computed on the input and the output $Y$ of the ANNs (lower) The hamming distance between two outputs, as the inputs have exactly one bit flipped; Image generated by the author.
</center>

In the picture above, here again we show the phase transition, but this time, from the perspective of the diffusion. In the upper shows the diffusion happening between the input and output. And the lower panel shows what is happening when we check the correlation between two output with exactly one bit flip in input. 

Yet again, this is very interesting to note that the phase transition pushes the same neural network, from a regime where the output is an attractor that pull the activity towards the same value, even when we flip the input, while increasing the  value of the control parameter pushes the network into a more and more chaotic regime, where the single bit difference creates more and more of an impact on the output, and repel the output from it's original attractor. 

## What about the 256SHA function ?

Okay, so this analysis wouldn't be complete without a proper comparison to at least one of the standard hash function. For this test I'll used the pretty well known SHA256 function, you can find it in the standard library of python. 

So, this time we compare the hash function diffusion against the one of a very chaotic artificial neural network. Since my tests showed a very similar BiEntropy value, close to 9.8 with the SHA256, I wanted to show a more interesting analysis, by making an a comparison with more bit flips, and along the way analyze the neural network diffusion when I choose a close to optimal parameter $\sigma=10^3$. 

![]({{ '/assets/article_images/2024-04-01-whitepaper-p3/comparison.jpg' | relative_url }})
<center>
The average over 100 networks of the hamming distance $D$ versus the number of bit flip in the input, for the ANN (blue line) and the SHA256 (orange line). Variance is not shown, but is around 0.001 for both hash function.
</center>

It's very interesting to witness that the ANN and SHA256 performs very similarly. In fact, it's quite impressive that the neural network gives the same hamming distance for 1 bit flip, and 50, and we can conclude that the number of bit flip has no influence whatsover on the hamming distance, which is what you want for the ANN to be considered a candidate for a Hash function. Keep in mind that this is only true because we carefully selected the weights distribution to elicit chaos. Logically, for less chaotic networks, you will see that for 1 to 10 bit flips the hamming distance tends to be very low, and it will increase as the number of flip increases.

### Conclusion

We explored the potential of ANN to be used as Hash functions. We conduced a collision and diffusion test, in order to check the validity of our neural network as a potential candidate for a hash function. These tests proved to be conclusive, especially regarding the last comparison with the SHA256 function, however, these are far from enough to state that our neural network is sufficient. Many more tested could be conducted, along with mathematical proof that should be worked on. For example, we could conduce a collision attack, to check how many often it's possible that a different input leads to the same ouput. This is a typical attack that allows the hacker to identify malicious code as being the legitiame. 

At first, it might seem surprising that these AI algorithms could be use for this seemingly unrelated task. However, keep in mind that the Hash function is just a map with very complex duffusion processes iterated many times, which is exactly what neural networks are, as we show their mathematical operations. 

On the other hand, if you where to build your hash with a neural network, there is one interesting thing you can harness: the random genweration of synpatic weights. You can in fact generate random weights for each specific task you want, which make it even more difficult to guess how the neural network was built. For instance, in the next article we will build a package for generating a pair of private and public key, and we will use the private key, as a seed for generating the weigths. Changing the weights for each usecase create a space of parameter which is so big, that even a quantum computer could not guess it.  
