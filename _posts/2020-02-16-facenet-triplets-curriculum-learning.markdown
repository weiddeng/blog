---
mathjax: true
layout:  post
title:   "FaceNet, Triplets, Curriculum Learning"
date:    2020-02-16 21:30:00 -0800
---
How to embed images of faces to a Euclidean space, so that faces of the same person are close and of different person are far away? In [Schroff et al. 2015][FaceNet: A Unified Embedding for Face Recognition and Clustering], the authors presented a nice solution, using off-the-shelf high capacity CNN architectures and batch learning. In each batch, the authors used a curriculum to select a couple of effective triplets to do gradient descent. A triplet consists of an anchor image, a positive image, and a negative image. The embedding is to a hypersphere.

[FaceNet: A Unified Embedding for Face Recognition and Clustering]: https://arxiv.org/pdf/1503.03832.pdf
