---
layout: post
title:  "Distill Knowledge and Transfer"
date:   2019-08-11 15:25:00 -0800
---
Knowledge distillation and transfer learning is very popular in industry in recent years.


The basic idea is that a complicated model - think about regularization, ensemble - trained on huge amount of data usually captures a lot of information. For a classification problem, the info can be logits, or partitioning the original problem into subproblems. We can then train a simple model using logits from the complicated model as soft labels. This makes hard labels optional. More importantly, by training on the soft labels, the simple model is able to learn on less data yet achieve similar result. A step further is training an ensemble of specialist models to assist the simple model.


Reference: [Distilling the Knowledge in a Neural Network][Distilling the Knowledge in a Neural Network]


[Distilling the Knowledge in a Neural Network]:https://arxiv.org/pdf/1503.02531.pdf
