---
layout: post
title:  "Attention is Viral!"
date:   2019-06-29 11:13:18 -0800
---
Attention is a big idea in DL. It mitigates a pain point in RNN/GRU/LSTM but is not just about that.


In the RNN/GRU/LSTM Encoder-Decoder framework ([Cho et al. 2014][Learning Phrase Representations using RNN Encoder–Decoder for Statistical Machine Translation], [Sutskever et al. 2014][Sequence to Sequence Learning with Neural Networks]), an input sequence is encoded as a vector after one pass of the sequence, then conditional on the vector, the decoder generates an output sequence. The pain point is that the encoded vector is inflexible to abstract information effectively. When input sequence is long, quality degrades. Theoretically GRU/LSTM mitigates the problem but practically they still come short.


Here comes the attention mechanism. I will talk about attention [Bahdanau et al. 2014][Neural Machine Translation by Jointly Learning to Align and Translate], self-attention [Cheng et al. 2016][Long Short-Term Memory-Networks for Machine Reading], and the generalized self-attention [Vaswani et al. 2017][Attention is All You Need]. All can be read in a key, value, query aspect.


Attention mechanism is built on top of the Encoder-Decoder framework. In Encoder, the input is encoded into a collection of key, value pairs. The interpretation is that each key, value pair annotates a part of the input. For instance in [Bahdanau et al. 2014][Neural Machine Translation by Jointly Learning to Align and Translate] the input is a sequence, and each word is annotated as h^j using Bi-RNN. We can also use Bi-GRU, Bi-LSTM, or anything. This is the Encoder. In Decoder, using the above example, traditionally the goal is to generate the current word y^i using the previous hidden state s^(i-1) and generated word y^(i-1). The inventive part is that a context vector c^i is introduced to contribute to y^i too. The way c^i is obtained is called the attention function. Attention bridges Decoder and Encoder. In the paper c^i is a linear combination of h^j's based on s^(i-1). We can think of s^(i-1) as the query hitting on each key, value pair, and each h^j is both the key and value. Encoder, Decoder, and attention together specify the model. Andrew Ng's deeplearning.ai [Sequence Models](https://www.coursera.org/learn/nlp-sequence-models) course week 3 has a notebook with code example.


Let's change gear to talk about self-attention. Unlike seq2seq, self-attention involves only 1 sequence. Originally in [Cheng et al. 2016][Long Short-Term Memory-Networks for Machine Reading] self-attention is introduced as an improved version of LSTM, by introducing attention of the current word on *previous* words and introducing the current hidden state context vector and the current memory context vector, and then using these context vectors in the traditional LSTM. In this approach, self-attention like LSTM requires one pass of the sequence. Later, self-attention breaks away from the sequential setting and denotes re-representation of token through its neighbors. The generalized self-attention is parallelizable. We can combine attention and self-attention to get a better Encoder-Decoder framework.


[Learning Phrase Representations using RNN Encoder–Decoder for Statistical Machine Translation]: https://arxiv.org/pdf/1406.1078
[Sequence to Sequence Learning with Neural Networks]: https://arxiv.org/pdf/1409.3215.pdf
[Neural Machine Translation by Jointly Learning to Align and Translate]: https://arxiv.org/pdf/1409.0473.pdf
[Long Short-Term Memory-Networks for Machine Reading]: https://arxiv.org/pdf/1601.06733.pdf
[Attention is All You Need]: https://arxiv.org/pdf/1706.03762.pdf
