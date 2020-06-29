---
mathjax: true
layout:  post
title:   "A Rear Mirror View at Contemporary NLP"
date:    2020-06-28 18:12:00 -0800
---

If figures below cannot be rendered, use this [link][link].

[ELMo (Embeddings from Language Models), Peters et al. 2018][Deep Contextualized Word Representations]: A well-known paper. The word representation differs from traditional word type embedding in that it is context aware and is a function of the entire input sequence. This can be achieved by pretraining a task in a model architecture with sequences as inputs. ELMo pretrains biLM in biLSTM. Application-wise, ELMo fixes the representations and relies on the application task to absorb them. ELMo may fine tune, but once fine tuned, the representations are fixed during task training. So ELMo is feature-based.

[GPT (Generative Pre-Training), Radford et al. 2018][Improving Language Understanding by Generative Pre-Training]: This is the paper where Transformer became canonical. It uses 1. generative pre-training of a language model on a diverse corpus of unlabeled text, followed by 2. discriminative fine-tuning on each specific task.

### generative pre-training
<center><img src="../assets/gpt-pretraining.png" width="300"/></center>
The pre-training task is left-to-right next word prediction using a size $k$ context window. The model architecture is multi-layer Transformer decoder. GPT uses a diverse corpus with long stretches of contiguous text as well as the Transformer architecture to build capacity for transfer.

### discriminative fine-tuning
Unlike ELMo, GPT uses fine-tuning for downstream tasks. It adds minimal task specific architectures and is trained on the downstream tasks by fine-tuning _all_ pre-trained parameters. GPT adds language modeling as an auxiliary objective to fine-tuning.
<center><img src="../assets/gpt-architecture.png" width="300"/></center>
<br />

[BERT (Bidirectional Encoder Representations from Transformers), Devlin et al. 2018][BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding]: BERT brings a couple of new ideas: 1. Multi-task pre-training: masked language model (MLM) & next sentence prediction (NSP). MLM is inspired by the Cloze task and goes really well with the bidirectional architecture. 2. Bidirectional Transformer architecture with boosted model capacity. As a result, the pre-trained BERT model can be fine-tuned with just one additional output layer to create state-of-the-art models for a wide range of tasks.
<center><img src="../assets/bert-1-2-punch.png" width="300"/></center>
<br />

In MLM, the final hidden vectors corresponding to the mask tokens are fed into an output softmax over the vocabulary. In fine-tuning, the token representations are fed into an output layer for token level tasks, such as sequence tagging or question answering, and the [CLS] representation is fed into an output layer for classification, such as entailment or sentiment analysis.


In summary, it looks like pre-training + fine-tuning has been the most common approach and task-specific architectures are no longer necessary. Fine-tuning requires training on a supervised dataset specific to the desired task. Typically thousands to hundreds of thousands of labeled examples are used.


Reference:  
[Deep Contextualized Word Representations][Deep Contextualized Word Representations]  
[Improving Language Understanding by Generative Pre-Training][Improving Language Understanding by Generative Pre-Training]  
[BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding][BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding]  
[Language Models are Unsupervised Multitask Learners][Language Models are Unsupervised Multitask Learners]  
[Language Models are Few-Shot Learners][Language Models are Few-Shot Learners]  

[link]: https://github.com/weiddeng/blog/blob/gh-pages/_posts/2020-06-28-a-rear-mirror-view-at-contemporary-nlp.markdown
[Deep Contextualized Word Representations]: https://arxiv.org/pdf/1802.05365.pdf
[Improving Language Understanding by Generative Pre-Training]: https://openai.com/blog/language-unsupervised/
[BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding]: https://arxiv.org/pdf/1810.04805.pdf
[Language Models are Unsupervised Multitask Learners]: https://openai.com/blog/better-language-models/
[Language Models are Few-Shot Learners]: https://arxiv.org/pdf/2005.14165.pdf
