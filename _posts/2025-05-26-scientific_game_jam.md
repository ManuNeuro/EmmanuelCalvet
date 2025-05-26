---
title: "The Collapse is Near : a Video Game Explaining my Thesis"
layout: post
categories: 
comments: true
image: /assets/article_images/2025-05-26-scientific_game_jam/cover.jpg
---

 # The Collapse is Near : a video game explaining my thesis

Thanks to my dear colleague and organizer, Floriant Sauges from Numana, I participated at the [2025 Scientific Game Jam, in Montreal](https://www.scientificgamejam.org/home/), where I teamed up with game designers to explicits my [research thesis](https://savoirs.usherbrooke.ca/handle/11143/22542) into a ludical experience : *a video game*. After two days of hard work, and many ulterior evenings finetuning the gameplay, we produced "**The collapse is Near**".

- [Itch.io : The Collapse Is Near by CogNoman, ManuNeuro, Mounk, and AlexiaChampain](https://cognoman.itch.io/the-collapse-is-near)

The collapse is near, despite its name, is not a game about the end of civilisation due to the catstrophic meta-crisis we are currently in, no, it's a game that catapults you 5 billion years in the future, to save civilization from the near collapse of the sun. 

The only thing that stands against the desctruction of life, is you, your magnetic canon, and your dexterity at balancing the sun's state, at the rathor's edge of chaos, finding the right amount or order, and discorder to load in the magnetic waves, displayed in the dashboard of your spacechip, ..., each second giving extra time to life on earth to be saved. Obviously, resemblance with any real event is purely accidental.

<details>
<summary>Click here to show a presentation of the game</summary>


 Your mission, should you choose to accept it, is nothing short of celestial heroism: stabilize our aging sun to prevent its cataclysmic transformation into either a black hole or supernova.
  
Fast forward 5 billion years, we find ourselves at the brink of solar collapse. Our mighty sun is erratic, oscillating wildly as it teeters on the edge of chaos. This instability threatens civilization itself. 

Fortunately, your spacecraft is armed with a formidable magnetic cannon capable of influencing the sun’s state. Through its power, you can regulate the sun's size. But tread carefully—there are moments when the sun's instability requires you to intensify your cannon's output to counteract its unpredictable expansions.

The cannon fires waves at the sun, the patterns of which are depicted on your right-hand display. These waves adhere to the non-linear Mackey-Glass equation, capable of producing everything from harmonious consistency to utter chaos, dictated by your input via a keyboard. 

The keyboard is your instrument of influence, transmitting directives to the cannon’s central command. The system quantifies the entropy of your messages—a measure of signal predictability. Consider "AAAAAAAAAA," with an entropy of zero due to its utter predictability. Contrast this with "ASDFGHJKLM," where each character is equally likely and entropy is maximized.

Entropy of your input directly influences the waveform: predictable key patterns yield regular waveforms, while chaotic patterns unlock chaos.

Victory rests in your hands: master the cannon's entropy by balancing order and chaos. This delicate equilibrium counters the semi-random momentum of the sun itself, governed by a sinusoidal cycle initiated at a random phase each game—ensuring no two experiences are identical.
 
For further cosmic inspiration, refer to [Emmanuel Calvet's thesis](https://savoirs.usherbrooke.ca/handle/11143/22542). His work in neural networks mirrors your task—balancing chaos and order to optimize information processing. This approach, known as reservoir computing, paves a promising path toward minimizing AI's energy and hardware demands.

</details>

Now, since this is a scientific blog, I felt like diving a bit into the mathematic of the game, which nicely connect to one of the deeper aspect of my thesis : **the edge of chaos**. 
# Collapse is Near: The Mathematics Behind the Game

## Overview

**Collapse is Near** is an interactive game where players must balance order and chaos to stabilize an unstable sun. This challenge is based on mathematical concepts woven into gameplay, providing an engaging way to explore some principles from my thesis on balancing order and chaos within recurrent neural networks.

## Game Mechanics and Mathematical Concepts

### Input and Shannon Entropy

Players contribute via keyboard inputs, where we analyze the last 10 keys pressed, with the **Shanon entropy**:

- This measures the uncertainty or randomness in the sequence of keys.
  $$
  H(X) = -\sum p(x) \log_2 p(x)
  $$
  
  Where $p(x)$ is the probability of character $x$ appearing in the input. Intuitively, higher entropy indicates more unpredictability in your keystrokes (less repetition), which impacts the game's progression.

In practice, this means that :
1. If you enter the same letter "AAAAAAAAAA", the entropy will be minimum, $p(A)=1$, 
2. And if you only input distinct letters "QWERTYUIO", the entropy will be maximal, $p({Q, W, ...})=1/10$. 

Indeed, in the first case $H(X)=- 1 \times log_2(1)=0$, and in the second case, $H(X)=10 \times (1/10 \times log_2(10))=3.32$.

If we considered a more balanced approach, for instance where we had two equally probable characters, like "AAAAABBBBB", we would have an entropy of $H(X)=2 \times (1/2 \times log_2(2))=1$, a value in between. **Take this into consideration when you try the game!**
### Entropy Normalization

The computed entropy is then centered around zero and normalized:

$$
H_{norm} = \frac{2 \cdot (H(X) - H_{min})}{H_{max} - H_{min}} - 1
$$

This transformation ensures entropy values lie between -1 and 1, reflecting the balance between chaos and order, a perfectly balance entropy have a value of zero. If you look at the gage next to the keyboard in the spaceship, it will reflect the entropy of the last 10 characters, **after you pressed the key ENTER**. 

### Magnetic Cannon and Mackey-Glass Equation

Now, the way you control the magnetic canon is by loading it, with either, ordered, or disordered, messages. This will stack up in the entropy load of the canon. The more loaded, the more disordered wave pattern you send to the sun, and the less loaded, the more ordered the wave pattern will be.

The entropy influences the "**Entropy Cannon load**," $C$, affecting in turns the sun's stability:

- **$C_{t+1}$** = $\text{Clamp}(C_{t} + \text{Sensitivity} \times H_{norm}, 0, 100)$

So the Canon Load is a percentage value between 0 to 100%. The $Sensitivity$ parameter controls how much the entropy entered at the keyboard will affect the Canon Load : 
1. If you take a **very high value,** the last 10 keys will have a huge influence, and it means you will have to fine tune directly at the keyboard the level of order and disorder, **favoring messages with a balanced entropy, close to zero**.
2. If you take a **very small value**, it means the last ten digit entropy doesn't influence much, so if you want to influence the overall canon load, **you should input either very ordered / disordered messages**, to change the dynamic of the magnetic wave.

The next step, consists in converting the Canon Entropy load into a parameter ($\tau$), that controls the non-linear equation **Mackey-Glass**, one of the equation I personnaly used to train and test my neural networks in my two parpers, ([1], [2]). This equation is a well known benchmark in the field of reservoir computing, in part due to its richness, it can ellicit a wide range of dynamics, from regular to chaotic, depending on parameters. 
  
  The Entropy Canon Load, is then converted, to control the Mackey-Glass delay differential equation’s time constant $\tau$:

$$
  \tau=C_t * \tau_{max} / 100
$$
  $$
  \frac{dx(t)}{dt} = \beta \frac{x(t-\tau)}{1 + x(t-\tau)^n} - \gamma x(t)
  $$
  
  - **Intuition**: Lower $\tau$ produces a state $x(t)$ that depends on the recent history, and tends to ellicit regular patterns, essential in stabilizing the sun. Higher $\tau$, on the other hand, means that $x(t)$ will depends on far distant memories, and tends to generate chaotic patterns. In between, $\tau$ ellicit semi periodic wave patterns. 
  - We have empirically set $\tau_{max}=60$, in order to **conserve the monotonicity of the influence of $\tau$** on the dynamic of $x$. Meaning that at some point, even though you continue increasing $\tau$, you will obtain back some regular dynamics. 
  - Remember that there are other parameters, $\beta$, $n$, $\gamma$, all creating very complex and non-linear relations, so controlling the dynamic is not as simple as just pushing a parameter in one or the other direction, hence why we restricted the range of $\tau$ to obtain the desired "linear" control. 

## Sun State Dynamics

Players manipulate wave patterns to regulate the sun's state by managing the Entropy Load of the magnetic cannon, however, the sun has also its own forces at play:

- The sun's **momentum**, representing its internal dynamic, operates as a sinusoid with randomized phase, also influencing its stability, which you can't influence.

The $\text{sun\_state}$ is thus a function of two parameters, its **momentum**, and the **canon influence**. It is a quantity, in percentage, that controls the size of the sun, meaning that 0% the sun has completely shrinked, and at 100%, it is completely inflated.

Achieve stability by maintaining the sun's state between 40-60% for five seconds, three times in succession, to win. Lose if the sun state exceeds 100% or drops below 0%.

- **State Equation**:
  $$
  \text{sun\_state}(t+1) = \text{canon\_influence}(t) + \text{momentum}(t)
  $$
$$
  \text{canon\_influence} = -(\text{sun\_state} - \text{canon\_load}) \times \Delta T
  $$
$$
\text{momentum}(t) =A\sin(\theta t + \phi) \times \Delta T
$$

So, as you can see, the canon influence is not directly the mackey-glass equation, and it's more simply dependant on the $\text{canon\_load}$, in percentage, and the $\text{sun\_state}$ :
- When $\text{sun\_state}$ is bigger than $\text{canon\_load}$, the sign is thus negative, and the canon influence will decrease the value of the sun state, hence favoring shrinking. 
- On the other hand, when $\text{sun\_state}$ is smaller, we have a multiplication of two minus signs, ending up in a positive canon influence, in that case, the canon will make the sun grow. 

So now that we revealed our tricks, we can see that the mechanic is actually very simplistic, and the Mackey-Glass equation is just here for the display of the screen in the spaceship!

## Thesis Connection

This game serves as a playful implementation of the principles from my thesis, focusing on the interplay of chaos and order in recurrent neural networks, akin to reservoir computing models that optimize computational capability. I have written another [article](https://manuneuro.github.io/EmmanuelCalvet//quantum,/crypto/2024/04/01/whiepaper-p3.html) exploring similar concepts from the perspective of cryptopgraphy. There would be so much more to talk about, but this will need a fully dedicated article, so stay tune!


