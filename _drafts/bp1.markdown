---
layout: post
title:  "Energy Based Models"
date:   2021-09-10 21:10:02 -0400
categories: jekyll update
---

# Welcome to Computational Cognition

Very excited to start blogging! Check out the About page to learn the purpose of this blog and its intended audience(s), and the Author page to learn about me.

## Mini-Series on Energy Based Models

I am starting this blog with a mini-series of posts on Energy Based Models (EBMs). The profound impact of Energy Based Models (EBMs) on neuroscience and artificial intelligence, along with their mathematical elegance, are reason enough to make them the opening topic. However, I am further motivated to write on this topic because the literature on EBMs bridges many cool domains (will elaborate on these in future posts), there is a revived interest in this class of models \cite{du2019implicit, ramsauer2020hopfield}, and they are highly relevant to my own PhD work. There were also several questions related to EBM resources at the recent \hyperlink{https://www.main2021.org/}{Montreal AI and Neuroscience (MAIN) conference}. Perhaps the MAIN attendees behind these questions will find some insight on Computational Cognition :)

## How to Read This Blog

I aim to provide material relevant to a wide readership. That in mind, I will write posts in self-contained modules, each module being geared towards readers who want either minimal or substantial mathematical detail. As such, I intend this to be a choose-your-own adventure where you can pick out what you want to learn. I also aim to make each blog post \emph{short}, only 2-3 pages including figures, so that even if you read the whole thing you can do it on your coffee break.


# What are Energy Based Models?

<figure>
    <img src="https://zek3r.github.io/assets/bp1/fig1.png" title="zeke spent all night making this webpage" width="300" >
    <figcaption>Memories are as low points in the energy function: An energy function (red line) storing two memories as low points (blue arrow and yellow arrow). Blue/yellow highlight on x-axis denote model states that are associated with blue/yellow memory (attractor basins), and blue/yellow highlighted red line denote related portions of energy function. Y-axis is energy; x-axis is system state.</figcaption>
</figure>

An EBM is a mathematical object that uses an energy function (described below) to perform various cognitive tasks. These tasks could include learning, storing and retrieving memories, or representing a probability distribution (I call the latter a cognitive task because the brain can do this at least approximately--see \cite{pouget2013probabilistic}). In the context of neural-ai, EBMs are usually a form of artificial neural network. In addition to being a mathematical model for biological neural circuits, EBMs have many applications in AI, including image recognition \cite{fischer2014training}, language modelling (if one views transformers as EBMs \cite{ramsauer2020hopfield}), and robotics \cite{haarnoja2017reinforcement}. EBMs can be divided into two classes: deterministic, lacking randomness, and stochastic, using randomness to perform computations. Today's blog post will introduce the two categories of EBMs.


## Intuitive Description
A deterministic EBM is composed of two parts: a system (e.g. a raindrop on a lily pad) that can take on any configuration from a set of states (e.g. position of rain drop on lily pad), and an energy function (e.g. gravity on the rain drop), that assigns a value, the energy, to each configuration of the system. \textbf{It makes sense to think of the system states as points on a topographical map and the energy function as representing the elevation of each point}. The system 'prefers' to be in states of low energy just as a rock prefers to roll downhill. To store memories, one manipulates the energy function (the contours of the topographic map) so that the state that you want to memorize is at the bottom of a basin in the function and the system states you wish to associate with, or use to evoke, the memory are all inside of the basin. Then, to recall the memory from a memory-evoking state all you have to do is slide down the function, from the location of the evoking-memory, to the bottom of the basin, the location of the evoked memory. You can store multiple memories by making multiple, non-overlapping basins in your energy function.

\begin{figure}[h]
\centering
\includegraphics[width=0.98\textwidth]{files/bp1_2.png}
\caption{\label{fig2}\textbf{Recalling a memory is done by following the energy function 'downhill':} Given a starting state (white triangle), associated with a point on the energy function (white arrow), the dynamics of the EBM (green arrows by x-axis) follow the energy function 'downhill' (green arrows by energy function) to find the memory state (blue triangle), associated with the low point on the energy function (blue arrow), resulting in memory recall. Everything else is as in Fig.\ref{fig1}.}
\end{figure}

A stochastic EBM uses system and energy function in the same way as the deterministic one; the difference is the way in which the system transitions from one state to another. Given a starting state (position on the topographic map) of the system, the deterministic EBM will always move downhill to low points. Conversely, the stochastic system contains a little randomness: on average it will prefer to spend time in lower energy states but it will sometimes transition to higher states. The physical analogy here is to imagine 3D printing your topographic map so that it forms a surface with little hills and valleys on it. Now imagine shaking the map. The rock will usually stay in the valleys of the 3D map but it will jump around in a random-looking fashion as you shake it and will sometimes move up a hill or out of one valley and into another.

## Mathematical Description

Assume you have a model system parameterized by a vector $w \in \mathbb{R}^p$ and taking values in a set $\mathbb{R}^n$. An energy based model is one that uses an energy function $E : \mathbb{R}^n \times \mathbb{R}^p \mapsto \mathbb{R}$, to map model states (these could be inputs, outputs, and latent variables in the case of a neural network) to a single, scalar, energy value. A deterministic EBM is a dynamical system on $\mathbb{R}^n$ such that for $s \in \mathbb{R}_{\geq 0}$ or $\in \mathbb{N}$ and $\{x_t\}_{t \in \Omega}$ (where $\Omega$ could be $\mathbb{R}$ or $\mathbb{Z}$), $E(x_t) \geq E(x_{t + s})$. For example, if $E$ is differentiable in $x$, one could take the dynamics to simply descend the gradient of the energy function

\begin{align}
    x_{t+1}^{(i)} &= x_t^{(i)} -\eta \frac{\partial E(x_t, w)}{\partial x_t^{(i)}} \\
    \dot{x}^{(i)} & \propto - \frac{\partial E(x, w)}{\partial x^{(i)}}
\end{align}

for the descrete and continuous time cases respectively, where $\eta \in \mathbb{R}$ is a parameter and $i \in \{1,...,n\}$ denotes an element of $x$. The energy function is thus a Lyapunov function for the EBM's dynamical system. 

A stochastic EBM replaces the dynamical system with a probability distribution such that the model system samples states randomly. Hypothetically, given the energy function, one could define a probability distribution in any number of ways. In the literature the probability distribution is almost always taken to be the Gibbs distribution associated with the energy function:

\begin{align}
    p(x; w) &= \frac{1}{Z}\exp^{-\beta E(x,w)}
\end{align}

where $Z$ is the appropriate partition function and $\beta$ is a parameter with the physical meaning of inverse temperature. This makes sense because as $\beta$ increases the probability distribution becomes more and more peaked around values of low energy until, at zero temperature ($\beta = \infty$) the probability distribution becomes a dirac at the low point of the energy function, just as a physical system at zero energy would exhibit zero movement.

(MENTION CONNECTION WITH OTHER ML MODELS)

&nbsp; &nbsp; &nbsp;

Next blog post will describe the two classic neural-ai EBMs: the deterministic Hopfield model and the stochastic Boltzmann machine. $\sim$ Zeke

# Bonus Material

## Brief History

EBMs were first studied outside the neural-ai domain as models of the interactions between atoms in a magnetic substance in mathematical physics \cite{brush1967history}. John Hopfield introduced EBMs to neuroscience in 1982 by establishing their utility as associative memory models \cite{hopfield1982neural}, after which these models were heavily studied in both neuroscience and AI until the early 2000s (see, e.g. \cite{ackley1985learning, hinton2002training}). Around this time, difficulties with training large EBMs (EBMs with many neurons) relegated them to a more pedagogical role in the fields, but recent advances in training \cite{du2019implicit} coupled with connections to state-of-the-art machine learning models \cite{ramsauer2020hopfield} has ushered in a renewed interest in this classic model in recent times.

## Of EBMs and Amphibians
Imagine you are a stylish toad. Every Saturday you need to remember that it's time to start dressin' in preparation for a night on the town. However, you are lackadaisical and hate to have to remember much, let alone what weekday it is. Your friend frog is a phenomenal meteorologist and tells you that a single rain drop will fall on your Lily pad every Saturday in roughly the same region that a rain drop fell last Saturday. You'll never forget the position of last Saturday's rain drop (last Saturday was quite memorable), but you don't want to have to associate the sight of a droplet at any other point on your lily pad with the memory of last week's drop (way too much to remember). If only the rain would always fall on last Saturday's spot, than you could just remember that a rain drop at that location means it's time to start dressin'! Thankfully, you are well versed in AI: you press down on your lily pad around the region where last Saturday's drop fell, making a little depression. Now, when the rain drop falls near your memory of last Saturday's drop, the shape of the pad will make the drop run down to the position of your memory, telling you it's Saturday! You just turned your lily pad into an energy based model to associate the stimuli of the rain drop at different parts of your pad with your memory that it's time to start dressin'.

