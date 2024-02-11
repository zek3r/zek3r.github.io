---
layout: post
title:  "Energy Based Models II - Hopfield Network"
date:   2021-01-1 21:10:02 -0400
categories: jekyll update
---

## Introduction

This is the second blog post in a mini-series on Energy Based Models (EBMs). In today's post I will describe the classic deterministic EBM studied in neural-ai: the Hopfield model. This will ground the previous, introduction to EBMs, blog post in a concrete example and build on the last post by discussing not only how memories are retrieved but how they might be learned.

## Hopfield Model - Intuition

As with any artificial neural network, the Hopfield model can be conceptualized as a collection of neurons, which process information, connected to one another by synapses, which transmit information from neuron to neuron. The core components of the network are the neuron activations, a list (vector) of numbers, one for each neuron, describing how active a given neuron is at the given moment, another list (matrix) of numbers containing the strength of every connection in the network, and, lastly, a third list (vector) of numbers that gives the baseline activity of each neuron (which can alternatively be viewed as a constant input to each neuron). As with any mathematical model, the Hopfield model makes a bunch of abstractions; these are simplifications that only partially capture the features of a real biological network, made so that one can construct a concise mathematical description that is tractable (can actually be analyzed).

# Model Structure and Connections to Biology

Diving deeper into the structure of the model, the first thing to note is that it is recurrent, meaning that there are loops in the connection pattern between the neurons such that if you follow the synapses leading out of one neuron you could eventually trace a path back to that same neuron through other synapses. This is true to form biologically because many brain circuits are recurrent. The pattern of connections, or *connectivity*, in the model is symmetric, meaning that if neuron $$a$$ has a synapse leading to neuron $$b$$, neuron $$b$$ has a connection of equal strength leading to neuron $$a$$. This does not necessarily occur biologically, but simplifies the mathematical analysis in a big way (see Chapter 7 of Dayan and Abbott - CITE). Notably, the Hopfield network fails to satisfy Dale's law (CITE): the synapses leading out of a neuron can be inhibitory, decreasing the activity of the downstream, post-synaptic, neuron or excitatory, increasing its activity. In the brain, neurons can only be excitatory or inhibitory, but not both (as with everything there is actually an exception to this rule (CITE)). At the single neuron level, a each neuron linearly sums the inputs it receives from other neurons, a ubiquitous modelling choice in artificial neural networks that, again, frequently fails to hold true in the brain (CITE nonlinear summation). Finally, the network activations can only take two values: $1$, meaning the neuron is firing an action potential, or $-1$, meaning it is at rest. The AI community usually replaces $-1$ with $0$, a difference that affects the math in a minor way (as I will discuss below) but not the behaviour of the network. In spite of the many simplifications inherent in the Hopfield model, it has provided a wealth of theoretical insights that have driven forward the fields of neuroscience and AI (CITE).

# Retrieving Memories

As an EBM, the Hopfield model takes an initial network state--a set of neuron activations (recall this is a list of $$1$$s and $$-1$$s denoting which neurons are active and which aren't)--and iteratively updates this network state by changing the activations of the neurons in such a way as to slide down the contour of the energy function, retrieving a previously stored memory. Thus, through updates, the initial network state is associated with the stored memory, the state that the system converges to. A single neuron updates its state by summing its own constant input (its baseline activity) and the input provided to it by all the other neurons with which it is connected. The latter input is given by a sum over the connected neurons' activations, with each activation weighted according to the strength of the connection between the given neuron and the neuron who's activity we are updating. For example, if the neuron we wish to update is connected with neuron $$a$$ and neuron $$b$$ only, neuron $$a$$ has activity $$1$$, neuron $$b$$ has activity $$-1$$, and the weights (synaptic strengths) between these neurons and the updating neuron are $$1/2$$ and $$1/4$$ respectively, the input to the updating neuron will be $$1/2 \times 1 + 1/4 \times (-1) = 1/4$$. To update the entire network, one can either update all neurons at once or a single neuron at a time, referred to as *synchronous* and *asynchronous* updating respectively. It can be shown that, through performing these updates, the network will eventually converge to a *fixed point* (CITE), meaning that if one applies the update rule again it will not change the pattern of activations. This fixed point is a low point of the energy function and, thus, represents a stored memory.

# Learning Memories

At this point I have described the network structure and how to retreive memories. The natural follow up question is how to learn a memory; stated alternatively, how to embed a low point in the energy function of the Hopfield network. Just like in the brain, memories are stored in the strengths of the connections between the neurons, which makes sense as the synapses control how information is passed between neurons in the network and, thus, the path that the updates in the network will follow towards a fixed point in the retrieval process. To "learn" a single memory, the network connection strengths are changed so that each is equal to the product of the activations of the two connected neurons, when the network activations are set to the memory pattern. If two neurons have equal activations for the memory pattern then the synapse strength between them will be $1$; if they have opposite activations the synapse strength will be $-1$. To store more than one memory, one simply takes the average over the synapse strengths dictated by each of the memories one wishes to store. This learning rule might sound familiar to those who have taken a first year psych or neuroscience course, as it dictates that neurons that have the same activations for many memories will have "strong", positive, synapses while neurons that tend to have opposing patterns for many memories will have "weak", negative, synapses. Stated more concisely, this is the classic Hebbian learning rule: "neurons that fire together wire together" (CITE).


## Hopfield Model - Math

The Hopfield network is a network of $$N$$ neurons with activations (see above section) given by $$x \in \{1, -1\}^n$$ (the set of all $$n$$-dimensional vectors of $$1$$ and $$-1$$). Assume the neurons are connected by synapses in an undirected graph structure such that the strength of the connection from neuron $$j$$ to neuron $$i$$ is the $$i$$-$$j^\mathrm{th}$$ element of the matrix $$W$$. Let neuron $$j$$, $$j \in \mathbb{R}^N$$, have a fixed baseline firing rate (perhaps given by a constant input), defined by the $$j^\mathrm{th}$$ element of the vector $$b \in \mathbb{R}^N$$. Finally, we denote the sign function $$\sigma : \mathbb{R} \mapsto \mathbb{R}$$ by

$$
    \sigma(x_j) = 
    \begin{cases} 
      \:\:\: 1 & x_j \geq 0  \\
      -1 & x_j < 0 \\
   \end{cases}.
$$

In an abuse of notation we will apply $$\sigma$$ to vectors, by which we mean that the function is applied element-wise. 

With the above notation, the Hopfield network admits the following energy function

\begin{align}
    E(x) = - \sum_{i, j} x_i W_{ij} x_j - \sum_j x_j b_j.
\end{align}

The astute reader will notice that, for appropriate choices of $$W$$ and $$b$$, this energy function can be viewed as a second order Taylor approximation of any twice differentiable function mapping $$\mathbb{R}^n$$ to $$\mathbb{R}$$. Noting that the gradient of a first or zeroth order Taylor approximation would yield unchanging dynamics, it becomes clear that the Hopfield model is effectively the simplest, if we take "simple" to mean having the lowest order energy function, EBM yielding nontrivial dynamics.

The dynamics of the EBM are given by

$$
    x_{t+1}^i = \sigma\Big(\sum_j W_{ij}x_t^j + b_i\Big),
$$

where $$t \in \mathbb{Z}$$, we have used subscript to denote time and superscript neuron index.

Lastly, to embed $$M$$ memories, $$\{v_m\}_{m \in \{1,...,M\}}$$ in the energy function one defines the weight matrix as follows:

$$
    W = \frac{1}{M}\sum_{m=1}^M x_m x_m^\top,
$$

that is, one takes the average of the outer products of the state vectors representing memories.


&nbsp;

In the last blog post we introduced EBMs, and their deterministic and stochastic flavours, and in this post we explored a classic example of a deterministic EBM. The next blog post will discuss the classic stochastic EBM known as the Boltzmann machine. As we will see, these two models are two sides of the same coin in so far as they are deterministic and non-deterministic versions of exactly the same neural network architecture. $$\sim$$ Zeke

## Bonus Material



## References

1. Du, Y. & Mordatch, I. Implicit generation and generalization in energy-based models. (2019)
2. Ramsauer, H.et al.Hopfield networks is all you need. (2020)
3. Pouget, A., Beck, J. M., Ma, W. J. & Latham, P. E. Probabilistic brains: knowns and unknowns. (2013)
4. Fischer, A. & Igel, C. Training restricted Boltzmann machines: An introduction. (2014)
5. Haarnoja, T., Tang, H., Abbeel, P. & Levine, S. Reinforcement learning with deep energy-based policies. (2017)
6. LeCun, Y., Chopra, S., Hadsell, R., Ranzato, M. & Huang, F. A tutorial on energy-basedlearning. (2006)
7. Brush, S. G. History of the Lenz-Ising model. (1967)
8. Hopfield, J. J. Neural networks and physical systems with emergent collective computational abilities. (1982)
9. Ackley, D. H., Hinton, G. E. & Sejnowski, T. J. A learning algorithm for Boltzmann machines. (1985)
10. Hinton, G. E. Training products of experts by minimizing contrastive divergence. (2002)





