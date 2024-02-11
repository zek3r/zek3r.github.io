---
layout: post
title:  "Decoding Messages in the Brain"
date:   2021-09-10 21:10:02 -0400
categories: jekyll update
---

# Decoding the Language of the Brain
Welcome to my first blog post! My first few posts will be about the peer-review publications that I have contributed to during my Masters and PhD thus far. Today's blog is about my MSc thesis paper: ***Neural Burst Codes Disguised as Rate Codes***, a title which will hopefully make sense by the end of this post!

## The Big Picture (Background)

At a high level, the research question asked by my MSc paper is *how can we understand the language of the brain?*

### The language of brain cells

<p align="center">
  <img src="/Users/ezekielwilliams/documents/website/zek3r.github.io/assets/msc/blogpost_fig1.jpg" />
</p>

![Neuron A messages Neuron B](/_drafts/images/blogpost_fig1.jpg)
*A neuron receives electrical input from other neurons via its 'branches' (known as dendrites) and sends electrical signals to other neurons via its 'trunk' (axon), using a tree analogy. The membrane voltage in the axon of neuron A is plotted as a function of time in the bottom left.*

Your brain is composed of about 86 billion neurons--brain cells which are believed responsible for performing the computations that allow you to see, smell, think, talk, walk, and generally experience cognitive phenomena. To do this, these neurons communicate with each other, kind of like how a group of friends might text eachother to coordinate their work on a school project. However, while text chats are composed of words, neuron conversation is composed of electrical pulses that neuroscientists refer to as *action potentials*, or *"spikes"*.

### Why we want to decode brain cell messages

Modern science doesn't fully understand how to read the neural "spike language". This is an important problem because if we could decode the spike-written messages between neurons, we would be one big step closer to understanding the brain, and could potential solve medical problems--for example being able to control prosthetic devices just by thinking.

### How neuroscientists think about brain cell messages

![Rate codes vs temporal codes](/_drafts/images/blogpost_fig2.pdf)
*For a rate code, the spike sequence in plot A is the same as that of plot B, but not plot C. For a temporal code, the ISI (distance between consecutive spikes) matters (see plot A).

There are many theories for how to read the neural language, or "code". These theories can be divided into two categories: **rate codes** and **temporal codes**. Rate code theories say that the only important aspect of the spike messages that a given neuron, neuron 'A', sends to another, neuron 'B', is the *rate* of spikes. If neuron A sends 1 spike every second for 3 seconds, then neuron B will see that neuron A's spike rate is 3 spikes per second, and this will mean something *different* to neuron B than if A had sent neuron B 2 spikes per second during each of the three seconds--a spike rate of 2 spikes per second--but will have the same meaning as if neuron A had send 3 spikes to neuron B in the first second and no spikes in the next two seconds: 3 spikes in 3 seconds gives the same rate no matter which specific times within the three second window the spikes are sent. 

Conversely, temporal coding theories say that the relative timing of the spikes does matter: that is, when neuron A sends 3 spikes in the first second this is different than if A had distributed its spikes evenly in time, first spike at second 1, the second spike one second later at second 2, the third two seconds later at second 3. In this way, in a temporal code, when A sends 2 spikes separated by 1 second this would represent a different word than if A had sent B two spikes separated by 2 seconds $^\dagger$ .

My research paper looked at addressing a key question that appears in a certain class of temporal codes--how to resolve ambiguity between spike sequences.

### Ambiguity in bursty brain cell messages

![Medium length ISIs can be ambiguous in a burst code!](/_drafts/images/blogpost_fig3.pdf)
*In plot A, spikes close together--small ISIs--are clearly a burst, spikes far apart--big ISIs--a clearly not a burst, but what about 'medium' ISIs? One can plot the frequency of occurence of a given ISI, or probability of an ISI occurring, for a given cell, as in plots B and C. Plot B shows not very many medium ISIs, so it's unambiguous. Alternatively, plot C has many medium ISIs, so it's very ambiguous!*.

Imagine that you have a temporal code where spikes that occur really close together mean something different from spikes that occur farther apart. This kind of code is called a *burst code* because two or more spikes close together in time--bursts of spikes--mean something different than other, more spread out, spike sequences. Messages sent using this code will be easy to read if spikes only occur *very* far apart--making them clearly non-bursts--or *very* close together--making them clearly bursts. But what if they occur intermediate distances apart? If there is no hard threshold between what is considered a *"burst"* and what isn't, then when neuron A sends neuron B spikes with time gaps of an intermediate length, there will be ambiguity when neuron B tries to understand what neuron A said because it won't know if the spikes were a burst or not. 

    An analogy: imagine two friends, Bob and Alice, and that Bob is listening to Alice. Further imagine that Alice has a short attention span and is very rapidly switching back and forth between telling Bob about a performance of the nut cracker and explaining to Bob why almond milk is less sustainable than oat milk. To make it easier to tell when Alice is describing ballet or milk-alternatives, she talks about the former in french and the latter in english. *However*, there are many words that are the same between english and french, so when Alice says "festival", it may not be clear to Bob whether this is a dance festival or a festival of flavours on the tongue brought on by a tasty milk alternative. These words that are the same in french and english are analogous to the spikes that are of intermediate distance apart, which we can't immediately classify as bursts or non-bursts.

Importantly, experimental evidence shows that these ambiguous, intermediatly-spaced spikes occur frequently in the brain. It is thus an important question for neuroscientists whether neuron B can decode enough of the message from neuron A even with this ambiguity. If not, then neuroscientists will be able to scratch burst codes from the list of possible theories for reading the language of the brain.

## What we did in my MSc Paper

Our specific research question was whether neuron A can communicate effectively with neuron B, even when there is ambiguity in whether a sequence of spikes can be considered a burst or not.

### Our research methods


To answer this question, we needed a way of quantifying how well neuron B can decode neuron A's spikes as a function of how much ambiguity there is A's spike messages. We took a computational approach to answer this question, meaning that we came up with a mathematical model of neurons A and B--a set of equations describing the spikes that A produces and the machinery that B uses to decode those spikes--and analyzed a computer simulation of this model. In this model, we could vary the amount of ambiguity in A's messages. We then used tools from **information theory** to quantify how well neuron B could, in theory, decode neuron B's messages. Information theory is a branch of mathematical statistics that quantifies the amount of variability in a variable--where a variable could be the message that one neuron sends to another, for example--and how much the variability in one variable says about the variability in another. If the variability in neuron B's decoding of neuron A's message says a lot about A's original message, then neuron A is communicating well with neuron B!

### Our findings and why they are useful

We found that, in certain cases, neuron A can communicate quite effectively with neuron B, even when there is ambiguity between what spike pairs should be considered bursts, and which should be considered non-bursts! Interestingly, in such cases, the statistics of neuron A's spiking look very similar to how they would look if neuron A was using a rate code. This means that if an experimentalist records neuron activity in the brain and finds that a neuron's spiking looks like a rate code it might actually be stealthily using a temporal, bursting code! Hence the name of the paper.

For a brief overview of some of the math used, and for some key references, see below. Thanks for reading!

## Math Overview

### Neural Circuit Model
I'm not including the equations in this section as they are bulky and don't provide too much intution. See (supp of paper) for them.
- For neuron A, we formulated a new bursting model, the Burst Spike Response Model, inspired by the Spike Response Model of Gerstner et al (cite) and previous work by my MSc supervisor Richard Naud (cite). Mathematically, the model is a point process determined by an event rate and a burst probability: the former determines how often spikes occur and the latter determines whether or not a rapidly-occurring burst spike occurs after the original spike. In neuro-theory language, this is a two compartment model with stochastic spiking, where the first (dendritic) compartment encodes the burst probability and the second somatic compartment encodes the event rate
- The inputs to neuron A are Ornstein-Uhlenbeck processes
- Neuron B is modelled as a nonlinear function of the linearly filtered spike train of neuron A

### Info Theory Analysis
To quantify information transmitted we used the mutual information rate, $\mathbb{I}$, a generalization of mutual information from random varaibles to stochastic processes:

$$
\mathbb{I}(X, Y) = \lim_{T \to \infty}\frac{1}{T}I(X_{0:T};Y_{0:T}),
$$

where $X$ and $Y$ are stochastic processes and $I(X_{0:T};Y_{0:T})$ is the mutual information between the vector $X_{0:T} = [X_0, ..., X_T]$ and $Y_{0:T} = [Y_0, ..., Y_T]$ (CITE).

This quantity is a massive hassle to compute (CITE), but one can compute a linear lower-bound for this information rate as:

$$
-\int_0^{\frac{1}{2}}\log_2\big(1 - \Phi_{XY}(f)\big)\mathrm{d}f,
$$

where $\Phi_{XY}$(f) is the coherence between $X$ and $Y$ and $f$ is frequency (CITE). We used this to quantify information transmitted in the paper.


## References
For further reading on a given topic, see below:
- Neural coding (e.g. rate and temporal codes)
- Burst coding
- Modelling neurons
- Information theory
- Information theory applied to neuroscience

### In-blog footnotes
- $\dagger$ : one may notice that the difference between rate and temporal codes is a little more subtle than described above; in particular, when the precision of the temporal encoding/decoding is sufficiently high. Think of a scenario where the time window over which the decoding cell is estimating spike rates is sufficiently small (e.g. spikes per ms instead of spikes per second), and the encoded signal has sufficiently precise temporal structure. Dispite this similaritiy, rate vs. temporal codes end up being a useful distinction in practice (see refs).

### In-blog references


&nbsp; &nbsp; &nbsp;
