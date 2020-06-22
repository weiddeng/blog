---
mathjax: true
layout:  post
title:   "FaceNet, Triplets, Curriculum Learning"
date:    2020-02-16 21:30:00 -0800
---
How to embed images of faces to a Euclidean space, so that faces of the same person are close and of different person are far away? In [Schroff et al. 2015][FaceNet: A Unified Embedding for Face Recognition and Clustering], the authors presented a nice solution, using off-the-shelf high capacity CNN architectures and batch learning. In each batch, the authors used a curriculum to select a couple of effective triplets to do gradient descent. See [Weinberger et al. 2009][Distance Metric Learning for Large Margin Nearest Neighbor Classification]. A triplet consists of an anchor image, a positive image, and a negative image. The embedding is to a hypersphere and uses only 128-bytes (32x32 bilevel image) per face.

<center><img src="/assets/facenet.png" width="450"/></center>

### Triplet Loss
Want the anchor image $x^a$ to be closer to (squared L2 distance) all positive images $x^p$ than any negative image $x^n$. I.e.,

$$
L = \sum\limits_i^N\left[||x_i^a - x_i^p||_ 2^2 + \alpha - ||x_i^a - x_i^n||_ 2^2\right]_ {+}
$$

where $\alpha > 0$.


Reference:  
[FaceNet: A Unified Embedding for Face Recognition and Clustering][FaceNet: A Unified Embedding for Face Recognition and Clustering]  
[Distance Metric Learning for Large Margin Nearest Neighbor Classification][Distance Metric Learning for Large Margin Nearest Neighbor Classification]  
[DeepFace: Closing the Gap to Human-Level Performance in Face Verification][DeepFace: Closing the Gap to Human-Level Performance in Face Verification]  
[Deep Face Recognition][Deep Face Recognition]

[FaceNet: A Unified Embedding for Face Recognition and Clustering]: https://arxiv.org/pdf/1503.03832.pdf
[Distance Metric Learning for Large Margin Nearest Neighbor Classification]: http://jmlr.csail.mit.edu/papers/volume10/weinberger09a/weinberger09a.pdf
[DeepFace: Closing the Gap to Human-Level Performance in Face Verification]: https://www.cs.toronto.edu/~ranzato/publications/taigman_cvpr14.pdf
[Deep Face Recognition]: https://www.robots.ox.ac.uk/~vgg/publications/2015/Parkhi15/parkhi15.pdf
