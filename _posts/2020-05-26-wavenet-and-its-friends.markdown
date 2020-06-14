---
mathjax: true
layout:  post
title:   "WaveNet and its Friends"
date:    2020-05-26 08:31:00 -0800
---

If figures below cannot be rendered, use this [link][link].

[Osindero et al. 2008][Modeling Image Patches with a Directed Hierarchy of Markov Random Fields]: Use Deep Belief Nets to stack a bunch of Restricted Boltzmann Machines (without lateral connections), or rather, Semi-Restricted Boltzmann Machines (with lateral connections and the visible units form a conditional MRF). Layer-by-layer energy-based learning, sampling a hidden vector $h$ from the conditional posterior $p(h \mid v)$ in the lower-level and feed it as the input data to the higher-level that learns the next layer of features. After learning, data can be sampled top-down all the way to the input data layer to generate input images. The generation process uses MCMC and is intractable.

[Larochelle et al. 2011][The Neural Autoregressive Distribution Estimator]: Neural Autoregressive Distribution Estimator (NADE). It models the distribution of high dimensional bilevel data tractably by decomposing the joint distribution into conditional distributions,
$$
\begin{align}
p(v)&=\prod_{i=1}^D p(v_i \mid (v_1, ..., v_{i-1}))\\
&=\prod_{i=1}^D \frac{p(v_1, ..., v_i)}{p(v_1, ..., v_{i-1})}\\
&=\prod_{i=1}^D \frac{\sum\limits_{(w_{i+1}, ..., w_D)}p(v_1, ..., v_i, w_{i+1}, ..., w_D)}{\sum\limits_{(w_i, ..., w_D)}p(v_1, ..., v_{i-1}, w_i, ..., w_D)}\\
&=\prod_{i=1}^D \frac{\sum\limits_{(w_{i+1}, ..., w_D)} \sum\limits_h \exp(-E((v_1, ..., v_i, w_{i+1}, ..., w_D), h))} {\sum\limits_{(w_i, ..., w_D)}\sum\limits_h \exp(-E((v_1, ..., v_{i-1}, w_i, ..., w_D), h))}.
\end{align}
$$
Each conditional distribution is intractable. However, the authors draw inspiration from an approximated distribution where there is an iterative solution. The authors then just use one iteration and come up with a 1-hidden-layer-feed-forward-neural-network-like approximation
$$
\begin{align}
p(v_i=1 \mid \mathbf{v}_{<i}=(v_1, ..., v_{i-1}))&=\text{sigmoid}(b_i + (\mathbf{W}^\mathsf{T})_ {i,\cdot}\mathbf{h}_i), \\
\mathbf{h}_i&=\text{sigmoid}(\mathbf{c} + \mathbf{W}_ {\cdot, <i} \mathbf{v}_{<i}).
\end{align}
$$
This is an autoregressive model and learning is based on MLE.
<center><img src="../assets/nade.png" width="250"/></center>
<br />

[Theis et al. 2015][Generative Image Modeling Using Spatial LSTMs]: Recurrent Image Density Estimator (RIDE). Combine two ideas: 1. mixtures of conditional Gaussian scale mixtures (MCGSMs), 2. spatial LSTM (SLSTM). SLSTM is a special case of the multi-dimensional LSTM where each memory unit has two preceding states, $c_{i,j−1}$ and $c_{i−1,j}$, and two forget gates, $f_{i,j}^r$ and $f_{i,j}^c$. We sequentially read a neighborhood of pixels to produce a hidden representation at every pixel, $p(x_{i,j} \mid x_{<ij}) = p(x_{i,j} \mid h_{ij})$, and use MLE learning.
<center><img src="../assets/ride.png" width="350"/></center>
<br />

[Gregor et al. 2015][DRAW: A Recurrent Neural Network For Image Generation]: DRAW has the following ideas: 1. generate image iteratively rather than in a single pass (similar to boosting?), 2. use autoregressive variational autoencoder architecture, 3. use dynamically updated attention mechanism to restrict both the input region observed by the encoder and the output region modified by the decoder.  
Essentially this is a variational autoencoder algorithm. The loss function used is a blend of the reconstruction loss and the latent loss. MNIST and Street View House Number generations are pretty good. CIFAR-10 generation is moderate.
<center><img src="../assets/draw.png" width="300"/></center>
<br />

[van den Oord et al. 2016][Pixel Recurrent Neural Networks]: Firstly check out [PixelCNN, PixelRNN Talk][PixelCNN, PixelRNN Talk] for a great overview. PixelCNN uses feature maps and PixelRNN uses hidden states and the overall idea is similar to [Theis et al. 2015][Generative Image Modeling Using Spatial LSTMs]. The main ideas include: 1. use masked convolutions, 2. use residual connections, 3. play with different spatial LSTMs.
<center><img src="../assets/pixelcnnrnn.png" width="300"/></center>
<br />

[van den Oord et al. 2016][Conditional Image Generation with PixelCNN Decoders]: Overall this paper is similar to the previous one. New ideas include: 1. replace ReLU between the masked convolutions in the original PixelCNN with gated activation unit to model more complex interactions:
$$
\mathbf{y} = \tanh(W_{k,f} \ast \mathbf{x}) \odot \sigma(W_{k,g} \ast \mathbf{x}),
$$
and 2. stack 2 CNNs to eliminate blind spots.  
CIFAR-10 generation is pretty good.  
A keyword of this paper is 'conditional', where a global latent vector $\mathbf{h}$ is cleverly added to each layer:
$$
\mathbf{y} = \tanh(W_{k,f} \ast \mathbf{x} + V_{k,f}^\mathsf{T} \mathbf{h}) \odot \sigma(W_{k,g} \ast \mathbf{x} + V_{k,g}^\mathsf{T} \mathbf{h}).
$$

[van den Oord et al. 2016][Wavenet: A Generative Model For Raw Audio]: Use dilated convolution to obtain a large receptive field:
<center><img src="../assets/wavenet.png" width="300"/></center>
<br />
Text-To-Speech: Transform the text into a sequence of linguistic and phonetic features (which contain information about the current phoneme, syllable, word, etc.) and feed it to WaveNet. The network’s predictions are conditioned not only on the previous audio samples, but also on the text. The result is kind babbling though.


Reference:  
[Modeling Image Patches with a Directed Hierarchy of Markov Random Fields][Modeling Image Patches with a Directed Hierarchy of Markov Random Fields]  
[U of Toronto CSC2535 Spring 2013 Lecture 4][U of Toronto CSC2535 Spring 2013 Lecture 4]  
[The Neural Autoregressive Distribution Estimator][The Neural Autoregressive Distribution Estimator]  
[Generative Image Modeling Using Spatial LSTMs][Generative Image Modeling Using Spatial LSTMs]  
[DRAW: A Recurrent Neural Network For Image Generation][DRAW: A Recurrent Neural Network For Image Generation]  
[Pixel Recurrent Neural Networks][Pixel Recurrent Neural Networks]  
[PixelCNN, PixelRNN Talk][PixelCNN, PixelRNN Talk]  
[MADE: Masked Autoencoder for Distribution Estimation][MADE: Masked Autoencoder for Distribution Estimation]  
[Conditional Image Generation with PixelCNN Decoders][Conditional Image Generation with PixelCNN Decoders]  
[Generating Images from Captions with Attention][Generating Images from Captions with Attention]  
[Wavenet: A Generative Model For Raw Audio][Wavenet: A Generative Model For Raw Audio]  

[link]: https://github.com/weiddeng/blog/blob/gh-pages/_posts/2020-05-26-wavenet-and-its-friends.markdown
[Modeling Image Patches with a Directed Hierarchy of Markov Random Fields]: https://papers.nips.cc/paper/3279-modeling-image-patches-with-a-directed-hierarchy-of-markov-random-fields.pdf
[U of Toronto CSC2535 Spring 2013 Lecture 4]: http://www.cs.toronto.edu/~hinton/csc2535/lectures.html
[The Neural Autoregressive Distribution Estimator]: http://proceedings.mlr.press/v15/larochelle11a/larochelle11a.pdf
[Generative Image Modeling Using Spatial LSTMs]: https://arxiv.org/pdf/1506.03478.pdf
[DRAW: A Recurrent Neural Network For Image Generation]: https://arxiv.org/pdf/1502.04623.pdf
[Pixel Recurrent Neural Networks]: https://arxiv.org/pdf/1601.06759.pdf
[PixelCNN, PixelRNN Talk]: https://www.youtube.com/watch?v=-FFveGrG46w
[MADE: Masked Autoencoder for Distribution Estimation]: https://arxiv.org/pdf/1502.03509.pdf
[Conditional Image Generation with PixelCNN Decoders]: https://arxiv.org/pdf/1606.05328.pdf
[Generating Images from Captions with Attention]: https://arxiv.org/pdf/1511.02793.pdf
[Wavenet: A Generative Model For Raw Audio]: https://arxiv.org/pdf/1609.03499.pdf
