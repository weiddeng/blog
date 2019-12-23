---
mathjax: true
layout:  post
title:   "Meta Learning 1 - MAML"
date:    2019-11-16 13:47:00 -0800
---
Learn math, physics, chemistry well, and not be afraid to travel the cosmo.  -- Chinese Proverb

This post is my initial mumbling on meta learning. Meta learning is a different way to learn and do tasks. Traditionally, a task is a singleton, learning to do it well end-to-end with minimal human intervention is all we need. Meta learning takes a step back, groups many tasks, and aims to learn all of them and generalize to new tasks. By bringing in structure, meta learning probably can learn many tasks in one go. One of the difficulties is "few shot" - a task provides only a few examples to learn from. In Professor [Nan Lin][Professor Nan Lin's Page]'s words, we need to borrow information.

### Notation
We want to learn not just one task, but a distribution of tasks $p(\mathcal{T})$, where a task $\mathcal{T} \sim p(\mathcal{T})$ has a loss function `$\mathcal{L}_\mathcal{T}$` and a sampling mechanism `$q_\mathcal{T}$`. For example, in a classification problem, we can be given a datapoint $x \sim q_\mathcal{T}(x)$ and asked to classify it; in an RL problem, we can get a trajectory from an initial observation $x_1 \sim q_\mathcal{T}(x_1)$ and successive observations $x_{t+1} \sim q_\mathcal{T}(x_{t+1}|x_t, a_t)$. Our goal is to obtain $\theta$ for a model $f_\theta$ (assume $f$ is predetermined), so that when given a generic task $\mathcal{T} \sim p(\mathcal{T})$ and K training datapoints, $f_\theta$ can quickly adapt and become a new model $f_{\theta_{\mathcal{T}, K}}$ that does well on $\mathcal{T}$. For simplicity, assume the adaptation process is through one gradient update:
$$
\theta_{\mathcal{T}, K} = \theta - \alpha \nabla_{\theta}\mathcal{L}_\mathcal{T}(f_\theta).
$$
Then, our goal is to obtain $\theta$ so that $f_{\theta_{\mathcal{T}, K}}$ does well on a generic task $\mathcal{T} \sim p(\mathcal{T})$. Namely,
$$
\begin{align}
\theta &= \underset{\theta}{\arg\min}\:\mathbb{E}_{\mathcal{T} \sim p(\mathcal{T})}[\mathcal{L}_\mathcal{T}(f_{\theta_{\mathcal{T}, K}})]\\
&= \underset{\theta}{\arg\min}\:\mathbb{E}_{\mathcal{T} \sim p(\mathcal{T})}[\mathcal{L}_\mathcal{T}(f_{\theta - \alpha \nabla_{\theta}\mathcal{L}_\mathcal{T}(f_\theta)})].
\end{align}
$$
$\theta$ can be obtained via stochastic gradient descent
$$
\theta \leftarrow \theta - \beta\nabla_\theta\mathcal{L}_\mathcal{T}(f_{\theta - \alpha \nabla_{\theta}\mathcal{L}_\mathcal{T}(f_\theta)}).
$$
This is essentially **Algorithm 1** in [Finn et al. 2017][Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks].

### Bayesian Hierarchical Modeling
Meta learning has a Bayesian perspective. Let $\theta$ be the task general random variable with prior $p(\theta)$, and $\phi \sim p(\phi|\theta)$ be the task specific random variable. During meta learning, we sample $\mathcal{J}$ tasks $\{\mathcal{T}_j | j \in \mathcal{J}\}$, and for each task $\mathcal{T}_j$, we sample $K$ training datapoints
$$
\mathbf{X}^{\text{train}}_j = \{\mathbf{x}^{\text{train}}_{j_k} = (x^{\text{train}}_{j_k}, y^{\text{train}}_{j_k}) : k = 1, ..., K\},
$$
and $M$ test datapoints
$$
\mathbf{X}^{\text{test}}_j = \{\mathbf{x}^{\text{test}}_{j_m} = (x^{\text{test}}_{j_m}, y^{\text{test}}_{j_m}) : m = 1, ..., M\}.
$$
Let $\mathcal{L}$ be the neg likelihood loss function, then
$$
\mathcal{L}(\phi) = -\log p(\mathbf{X}|\phi).
$$
Recall the meta learning objective is to minimize test loss.
Let $\mathbf{X}^{\text{test}} = \cup_{j \in \mathcal{J}}\mathbf{X}^{\text{test}}_j$, then
$$
\begin{align}
p(\mathbf{X}^{\text{test}}|\theta) &= \prod_{j \in \mathcal{J}}p(\mathbf{X}^{\text{test}}_j|\theta)\\
&= \prod_{j \in \mathcal{J}}\int p(\mathbf{X}^{\text{test}}_j|\phi_j)p(\phi_j|\theta)d\phi_j.
\end{align}
$$
And
$$
-\log p(\mathbf{X}^{\text{test}}|\theta) = -\sum_{j \in \mathcal{J}}\int p(\mathbf{X}^{\text{test}}_j|\phi_j)p(\phi_j|\theta)d\phi_j.
$$
Using $\hat{\phi}_j$ as an estimator for $\phi_j$, then
$$
-\log p(\mathbf{X}^{\text{test}}|\theta) = -\sum_{j \in \mathcal{J}} p(\mathbf{X}^{\text{test}}_j|\hat{\phi}_j).
$$
If set
$$
\begin{align}
\hat{\phi}_j &= \theta - \alpha \nabla_{\theta}\mathcal{L}_j(f_\theta)\\
&= \theta + \alpha \nabla_{\theta} \log p(\mathbf{X}^{\text{train}}_j|\theta)
\end{align}
$$
as in the adaptation process above, then maximizing the likelihood $p(\mathbf{X}^{\text{test}}|\theta)$ becomes minimizing $-\sum_{j \in \mathcal{J}} p(\mathbf{X}^{\text{test}}_j|\theta + \alpha \nabla_{\theta} \log p(\mathbf{X}^{\text{train}}_j|\theta))$, which is exactly
$$
\begin{align}
\theta &= \underset{\theta}{\arg\min}\:\mathbb{E}_{\mathcal{T} \sim p(\mathcal{T})}[\mathcal{L}_\mathcal{T}(f_{\theta_{\mathcal{T}, K}})]\\
&= \underset{\theta}{\arg\min}\:\mathbb{E}_{\mathcal{T} \sim p(\mathcal{T})}[\mathcal{L}_\mathcal{T}(f_{\theta - \alpha \nabla_{\theta}\mathcal{L}_\mathcal{T}(f_\theta)})]
\end{align}
$$
above. This shows **Algorithm 1** has an empirical Bayesian perspective. Taking a step further, because $posterior(\theta) \propto p(\theta) \times likelihood$, we can obtain a sampling mechanism for $\theta$ that addresses the ambiguity in MAML.

Reference:  
[Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks][Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks]  
[Recasting Gradient-Based Meta-Learning as Hierarchical Bayes][Recasting Gradient-Based Meta-Learning as Hierarchical Bayes]  
[Learning to Learn with Gradients][Learning to Learn with Gradients]

[Professor Nan Lin's Page]: https://pages.wustl.edu/nlin
[Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks]: https://arxiv.org/pdf/1703.03400.pdf
[Recasting Gradient-Based Meta-Learning as Hierarchical Bayes]: https://arxiv.org/abs/1801.08930
[Learning to Learn with Gradients]: https://ai.stanford.edu/~cbfinn/_files/dissertation.pdf
