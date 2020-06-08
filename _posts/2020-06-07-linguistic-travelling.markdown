---
mathjax: true
layout:  post
title:   "Linguistic Travelling"
date:    2020-06-07 12:48:00 -0800
---

ELMo

GPT

BERT

GPT-2

GPT-3

<!--
ELMo: deep contextualized word representation, a word representation that depends on the entire input sequence. ELMo (Embeddings from Language Model). pretrain optimization to get ... is cool. how to use in downstream task is pretty ugly.

GPT: We demonstrate that large gains on these tasks can be realized by generative pre-training of a language model on a diverse corpus of unlabeled text, followed by discriminative fine-tuning on each specific task.

effective way to transfer these learned representations to the target task. Existing techniques involve a combination of making task-specific changes to the model architecture [43, 44], using intricate learning schemes [21] and adding auxiliary learning objectives [50].
GPT is a good paper!

BERT: BERT alleviates the previously mentioned unidi- rectionality constraint by using a “masked lan- guage model” (MLM) pre-training objective, The masked language model randomly masks some of the tokens from the input, and the objective is to predict the original vocabulary id of the masked word based only on its context.
In addi- tion to the masked language model, we also use a “next sentence prediction” task that jointly pre- trains text-pair representations.

pretraining tasks and strategies


task specific architectures in ELMo is ugly.

token leve _and_ sentence level

Figure 1: Overall pre-training and fine-tuning procedures for BERT. Apart from output layers, the same architec- tures are used in both pre-training and fine-tuning. The same pre-trained model parameters are used to initialize models for different down-stream tasks. During fine-tuning, all parameters are fine-tuned. [CLS] is a special symbol added in front of every input example, and [SEP] is a special separator token (e.g. separating ques- tions/answers).

A distinctive feature of BERT is its unified ar- chitecture across different tasks. There is mini-mal difference between the pre-trained architec- ture and the final downstream architecture.
For a given token, its input representation is
constructed by summing the corresponding token, segment, and position embeddings.

pre-training有两个task, 2-task learning

BERT vs GPT only added bidireational
input pretrain is aways concatentaed pairs of sentences
fine tuning 1 additiona layer

GPT-2:
When a large language model is trained on a sufficiently
large and diverse dataset it is able to perform well across
many domains and datasets.  - this is also pragmatic

Despite of the benefit, current byte-level LMs still have non-negligible performance gap with the SOTA word-level LMs.

Our representations differ from traditional word type embeddings in that each token is assigned a representation that is a function of the entire input sentence.

Using in- trinsic evaluations, we show that the higher-level LSTM states capture context-dependent aspects of word meaning (e.g., they can be used with- out modification to perform well on supervised word sense disambiguation tasks) while lower- level states model aspects of syntax (e.g., they can be used to do part-of-speech tagging).

ELMo is a task specific combination of the in- termediate layer representations in the biLM.

More direct context embeddings
helpful using pre-training



model architectures
pre-training
how pretrained and how used
training objective
application
what gain?



-->
