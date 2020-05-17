---
mathjax: true
layout:  post
title:   "FaceNet, Triplets, Curriculum Learning"
date:    2020-02-16 21:30:00 -0800
---
How to embed images of faces to a Euclidean space, so that faces of the same person are close and of different person are far away? In [Schroff et al. 2015][FaceNet: A Unified Embedding for Face Recognition and Clustering], the authors presented a nice solution, using off-the-shelf high capacity CNN architectures and batch learning. In each batch, the authors used a curriculum to select a couple of effective triplets to do gradient descent. See [Weinberger et al. 2009][Distance Metric Learning for Large Margin Nearest Neighbor Classification]. A triplet consists of an anchor image, a positive image, and a negative image. The embedding is to a hypersphere and uses only 128-bytes (32x32 bilevel image) per face.

### Triplet Loss
Want anchor image $x^a$ to be closer to (squared L2 distance) all positive images $x^p$ than any negative image $x^n$. I.e.,

$$
L = \sum_i^N[\norm{x_i^a - x_i^p}_2^2 + \alpha - \norm{x_i^a - x_i^n}_2^2]_{\plus}
$$

[FaceNet: A Unified Embedding for Face Recognition and Clustering]: https://arxiv.org/pdf/1503.03832.pdf
[Distance Metric Learning for Large Margin Nearest Neighbor Classification]: http://jmlr.csail.mit.edu/papers/volume10/weinberger09a/weinberger09a.pdf
