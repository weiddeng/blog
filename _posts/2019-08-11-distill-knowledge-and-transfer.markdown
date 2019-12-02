---
layout: post
title:  "Distill Knowledge and Transfer"
date:   2019-08-11 15:25:00 -0800
---
Knowledge distillation and transfer learning is very popular in industry in recent years.


The basic idea is that a complicated model - think about regularization, ensemble - trained on huge amount of data usually captures a lot of information. For a classification problem, the information can be logits, or partitioning the original problem into subproblems. We can then use those information to train a simple model. For example, use logits from the complicated model as soft labels. By training on the soft labels, the simple model is able to achieve comparable result using less data. Basically, hard labels are not necessarily of the best representation to learn from.


When there is a large number of classes in a classification problem, the complicated model usually reveals some structure in the problem. We can partition the original problem into an ensemble of subproblems and train an ensemble of specialist models. Usually we can obtain a very good model by combining the complicated model and the ensemble of specialist models.


Reference: [Distilling the Knowledge in a Neural Network][Distilling the Knowledge in a Neural Network]


[Distilling the Knowledge in a Neural Network]:https://arxiv.org/pdf/1503.02531.pdf
