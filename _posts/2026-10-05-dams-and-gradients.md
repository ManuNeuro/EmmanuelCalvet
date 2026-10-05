---
layout: post
title: "Dams and Gradients: A Free-Energy Model of Trauma"
date: 2026-10-05
image: /assets/article_images/2026-10-05-dams-and-gradients/psycho_theory.png
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

*A free-energy, Hopfield and percolation sketch of why defenses cost so much, and why the nice guy explodes. This is thinking out loud: the equations are heuristics, not measurements.*

## The landscape is a memory

Picture Self Energy as water: vital, expressive, relational drive, running downhill and minimizing free energy. The terrain is not a generic hillside. It is an associative memory, with habits, schemas and remembered affordances as its valleys. The modern Hopfield energy makes this precise:

$$\LARGE E(\xi) = -\beta^{-1}\log\sum_i \exp(\beta x_i \cdot \xi) + \tfrac12\|\xi\|^2$$

Here $\xi$ is the current state and the $x_i$ are stored patterns. Ramsauer et al. (2020), *Hopfield Networks is All You Need*, showed that the retrieval step $\xi \leftarrow X\,\mathrm{softmax}(\beta X^\top\xi)$ is exactly transformer attention. So "flowing downhill" and "attending" are the same motion, and $\beta$, the inverse temperature, is precision. High $\beta$ means winner-take-all, stereotyped retrieval. Low $\beta$ means blended, flexible retrieval. Trauma plausibly raises $\beta$ in the regions it touches.

## A dam is a mask on attention

When flowing toward some region is repeatedly punished, the system learns to stay out. In attention terms, pattern $i$ gets a barrier $B_i$:

$$\LARGE a_i = \mathrm{softmax}_i\big(\beta x_i \cdot \xi - B_i\big)$$

The pull of the pattern is untouched. Only its access is blocked. The pressure behind the dam is the gap between what attention would give pattern $i$ and what it gets, $P_i = a_i^{(0)} - a_i$.

## The expensive part: defending the topology

A dam also resists *reshaping*. New experience acts as an external field $h$ that deforms the landscape, pulling the stored patterns $X$ toward something new. A defended system pulls them back toward the original $X_0$, the landscape the defense was built to preserve:

$$\LARGE J(X) = E(\xi; X) - h^\top\xi + \frac{\lambda}{2}\,\|X - X_0\|_F^2$$

The last term is an anchor, a spring of stiffness $\lambda$. Setting $\partial J/\partial X = 0$ gives the equilibrium

$$\LARGE X^* = X_0 + \frac{1}{\lambda}\, g, \qquad g = -\frac{\partial (E - h^\top\xi)}{\partial X}$$

where $g$ is the force that experience exerts on the memories. Deformation is push divided by rigidity. A flexible system (small $\lambda$) lets the landscape move. A rigid one barely moves, which is the clinical picture of corrective experiences being discounted and the old shape persisting after the environment has changed.

The spring pushes back with force $F = \lambda\,\Delta X$, where $\Delta X = X^* - X_0$, and must hold that for as long as the push lasts. This is the original $W = F\,d\cos\theta$ made precise:

$$\LARGE W = \lambda\,\|\Delta X\|\,\|d\|\cos\theta, \qquad \theta = \angle(\text{restoring force},\ \text{push})$$

When the defense directly opposes the push ($\theta \approx 0$), everything is paid for: rigid suppression, "just don't feel it". When it works across the push ($\theta \to 90^\circ$), it steers rather than blocks: sublimation, reappraisal, channeling. So redirective defenses should be cheaper than suppressive ones, which fits Gross's emotion-regulation findings. Two consequences:

- **Cost scales with $\lambda$ and with the size of the push.** Rigidity is cheap in a quiet life and ruinous in a changing one. Therapy, a loving relationship or a crisis all raise $g$, and the bill rises with it.
- **Stored energy grows too.** The anchor holds $\tfrac12\lambda\|\Delta X\|^2$ in reserve, and if it lets go, that is released at once.

## Erosion: the body pays

The organism has a finite budget, $E_{avail} = E_{total} - \sum W_{maint}$. As containment takes more, less is left for repair, immunity and growth. This is allostatic load: HPA-axis dysregulation, sustained sympathetic tone, low-grade inflammation. Adverse childhood experiences and chronic stress are associated with later autoimmune and cardiovascular disease, and emotional suppression has been linked to higher mortality in some prospective data.

*A caution: the stronger claim that avoidant or agreeable personality causes cancer (the "Type C" hypothesis) is poorly supported in large prospective studies. The defensible claim is about cumulative wear and vulnerability, not personality-caused disease.*

## The outburst is a percolation threshold

Treat the landscape as a lattice of pathways, each either dammed (probability $p$) or open. Below a critical open fraction, any flow that leaks stays local. Above it, a spanning cluster appears and the reservoir connects to everything. For site percolation on a square lattice this happens at an open fraction of about 0.593, so at a dam density of about 0.41, and the transition is sharp.

That gives the nice guy's explosion a phenomenology. The trigger is trivial because the system sits near criticality and the trigger is only the last bond. The onset is sudden because it is a phase transition. The behavior looks out of character because the person did not change; the connectivity did.

Water enters from the left wall. Brown cells are dams, blue cells are reached by flow, gray cells are open but unreachable. Drag the slider through 0.41 and watch the flooded fraction jump rather than rise.

<iframe src="{{ '/assets/sim/percolation.html' | relative_url }}" width="100%" height="620" style="border:1px solid #ccc;" loading="lazy" title="Dam percolation simulation"></iframe>

## The relaxation oscillator

An outburst is not healthy release. It tends to bring shame and social punishment, which are exactly the negative rewards that rebuild the dams, usually thicker. Press "Re-dam" after a breach and the system is back near the threshold with more stored pressure. The loop is build, breach, re-dam, build. The way out suggested by the model is continuous, small, titrated release that keeps the system well away from $p_c$, together with a gentler $\lambda$, so the landscape is allowed to move.

## Where it could break

- $F$, $\theta$, $\lambda$ and $p$ have no operational definitions yet. Without them this stays a metaphor.
- Not all damming is pathological. Boundaries and situational self-control are dams too.
- Downhill is not automatically good. The goal is wise channeling, not maximal flow.
- Real lattices are not random. Defenses are clustered and hierarchical, which changes both the threshold and how a breach cascades.
