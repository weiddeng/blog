---
mathjax: true
layout:  post
title:   "FaceNet, Triplets, Curriculum Learning"
date:    2020-02-16 21:30:00 -0800
---
How to embed images of faces to a Euclidean space, so that faces of the same person are close and of different person are far away? In [Schroff et al. 2015][FaceNet: A Unified Embedding for Face Recognition and Clustering], the authors presented a nice solution, using off-the-shelf high capacity CNN architectures and batch learning. In each batch, the authors used a curriculum to select a couple of effective triplets to do gradient descent. See [Weinberger et al. 2009][Distance Metric Learning for Large Margin Nearest Neighbor Classification]. A triplet consists of an anchor image, a positive image, and a negative image. The embedding is to a hypersphere and uses only 128-bytes (32x32 bilevel image) per face.

<center><img src="https://lh3.googleusercontent.com/ibdevThrFRAeCamYXzDDLsQvzcPAXrbXooIjcp8VAMDhpD0ukyI_1SxyEuKZL_dv9kdhUyFKqddu6hKMLY3oz06wkjMLpjY9D-X8bCxYwePxTMPYMed6-YqBI_-izQKh8XjqUqDNgH6-l7HIHpDF7PUtQKU5FAopEiYxOBCEA1_96dsikJE8oMkRmJ6xDBGzauIerh1kKwV7c0OQOTorpnfFv7sEVlrAUdAiU-lGy20b9IHCK8amEHFK7uW_KAKuyxNt_6E2uvTELaPRrO7SLajp6Wl_TK9a2lzaxMJIMa5x4DOx89mBLiPb8dApaB2rKsMI4EqbFqA7K_6NsSBld1UhOWpoOwCdtuEJaa_6T-NQpXzAjrn8KzPtnpIqSl0a_fLW2YF6iPuQ-ZKS98dGZmDykDMbihDc5zNpBL7d5c2LXNe1flEHjh4IVApO0iegHgzPVXyYri1TAUVex8Lyz0C7f_7aM0ij29fTZshq1cRtHEqB8O9zVVUnLVkl1w3Rqulhi1_Pd2jQz_gF0-A6a5sceVkIm48rSG5JNUV5RS18gVKwlBPsVFrz5SRidJPkAgSJZXP4QV9UIn_5CnzeACMhONZdNNWGNkyLmH4Ymy03p2B4i02-_OaIm8wYFnwv09I0wLhvzQI3FwkR6mJ_Z_iEZKuUOuycQIKUi_Q9eh7Gy41q4dzIAVjzT5oW=w1058-h898-no?authuser=0" width="450"></center>


### Triplet Loss
Wanna anchor image $x^a$ to be closer to (squared L2 distance) all positive images $x^p$ than any negative image $x^n$. I.e.,

$$
L = \sum_i^N[||x_i^a - x_i^p||_2^2 + \alpha - ||x_i^a - x_i^n||_2^2]_{+}
$$

where $\alpha > 0$.

Reference:
[FaceNet: A Unified Embedding for Face Recognition and Clustering][FaceNet: A Unified Embedding for Face Recognition and Clustering]
[Distance Metric Learning for Large Margin Nearest Neighbor Classification][Distance Metric Learning for Large Margin Nearest Neighbor Classification]
[Deep Face Recognition][Deep Face Recognition]

[FaceNet: A Unified Embedding for Face Recognition and Clustering]: https://arxiv.org/pdf/1503.03832.pdf
[Distance Metric Learning for Large Margin Nearest Neighbor Classification]: http://jmlr.csail.mit.edu/papers/volume10/weinberger09a/weinberger09a.pdf
[Deep Face Recognition]: https://www.robots.ox.ac.uk/~vgg/publications/2015/Parkhi15/parkhi15.pdf
