---
mathjax: true
layout:  post
title:   "What I Talk about When I Talk about Variational AutoEncoder"
date:    2019-10-18 15:34:00 -0800
---
VAE and Denoising AutoEncoder [code snippets][code snippets].

VAE learns data, and shows what generate the data. A datapoint in the data is generally high dimensional, and mixes signals and noises, so it is hard to capture what is going on. If we believe there is a low dimensional space from which the datapoint is generated from, then we can use VAE. Above all, VAE is a generative model.

Let us use $\{x_1, ..., x_n\}$ to denote the data, sampled from a manifold $X \subset \mathbb{R}^K$. VAE introduces $Z \subset \mathbb{R}^k$ and two global arrows, the decoder $f: Z \times \Theta \rightarrow X$ and the encoder $g: X \rightarrow \mathcal{D}(Z)$. $Z$ is the encoding space and $\mathcal{D}(Z)$ is the set of distributions on $Z$. We have a sampling mechanism on $Z$ via a prior $p(z)$. We wish to optimize $\theta$ so that we can sample $z$ from $p(z)$ and then $f(z, \theta)$ is similar to our data.

The setup is like this: $p(z)$ is a prior on $Z$, $x \sim p(x \mid z, \theta) \sim \mathcal{N}(f(z, \theta), \sigma^2I)$, $\sigma$ is a hyperparameter, $p(z \mid x, \theta)$ is the posterior. Like other models, VAE is a computational graph. There is a path $X \xrightarrow{g}  \mathcal{D}(Z) \xrightarrow{\text{sample}} Z \xrightarrow{f_\theta} X$, so there is reconstruction error loss. However, the actual optimization objective has another term. A key equation is that for an arbitrary $Q\in\mathcal{D}(Z)$, we have

$$
\begin{align}
D_{KL}(Q(z) || p(z|x, \theta)) &= \mathbb{E}_{z \sim Q}[\log Q(z) - \log p(z|x, \theta)]\\
&= \mathbb{E}_{z \sim Q}[\log Q(z) - \log p(x|z, \theta) - \log p(z)] + \log p(x).
\end{align}
$$

So

$$
\log p(x) - D_{KL}(Q(z) || p(z|x, \theta)) = \mathbb{E}_{z \sim Q}[\log p(x|z, \theta)] - D_{KL}(Q(z) || p(z)).
$$

Replace $Q$ by the encoder $g(x)$, then

$$
\log p(x) - D_{KL}(p_{g(x)}(z) || p(z|x, \theta)) = \mathbb{E}_{z \sim g(x)}[\log p(x|z, \theta)] - D_{KL}(p_{g(x)}(z) || p(z)).
$$

The optimization objective is maximizing $\log p(x)$, and if the decoder can sufficiently approximate the posterior, then the optimization objective is approximately maximizing the RHS. The RHS is a lower bound to $\log p(x)$.

Reference:
[Tutorial on Variational Autoencoders][Tutorial on Variational Autoencoders]

[code snippets]: https://github.com/weiddeng/autoencoders
[Tutorial on Variational Autoencoders]: https://arxiv.org/pdf/1606.05908.pdf
