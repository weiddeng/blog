---
layout: post
mathjax: true
title:  "What I Talk about When I Talk about Variational AutoEncoder"
date:   2019-10-18 15:34:00 -0800
---
VAE learns data, and shows what generates the data. A datapoint in the data is generally high dimensional, and mixes signals and noises, so it is hard to capture what is going on. If we believe there is a low dimensional space that captures the data, then we can use VAE. Let us use $X = \{x_1, ..., x_n\}$ to denote the data, sampled from a manifold $X \subset \mathbb{R}^K$, overloading $X$. VAE introduces $Z \subset \mathbb{R}^k$ and two _global_ arrows, the decoder $f: Z \rightarrow X$ and the encoder $g: X \rightarrow \mathcal{D}(Z)$, where $Z$ is the encoding space and $\mathcal{D}(Z)$ is the set of distributions on $Z$. For simplicity, we can narrow down to the (sub)set of normal distributions, $g: X \rightarrow \mathcal{N}(\mu(X), \Sigma(X))$, where $\Sigma(X)$ is a diagonal matrix. $Z$ is the encoding space. It is latent and captures information. However because $f$ and $g$ are usually represented in neural networks, they also capture nontrivial information.


Like other models, VAE is a computational graph. We need to define the graph and the optimization objective. There is a path $X \xrightarrow{g}  \mathcal{N}(\mu(X), \Sigma(X)) \xrightarrow{\text{sample}} Z \xrightarrow{f} X$, so there is reconstruction error loss. However, the actual optimization objective has another term.


To talk about the optimization problem, I will start it over.


TensorFlow 2.0 has a great notebook on VAE: [VAE Notebook in TF2.0][VAE Notebook in TF2.0]


[VAE Notebook in TF2.0]:https://www.tensorflow.org/tutorials/generative/cvae
