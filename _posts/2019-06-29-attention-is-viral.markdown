---
layout: post
title:  "Attention is Viral!"
date:   2019-06-29 11:13:18 -0800
---
Attention is a big idea in DL. It mitigates a pain point in RNN/GRU/LSTM but is not just about that.


In the RNN/GRU/LSTM Encoder-Decoder framework ([Cho et al. 2014][Learning Phrase Representations using RNN Encoder–Decoder for Statistical Machine Translation], [Sutskever et al. 2014][Sequence to Sequence Learning with Neural Networks]), an input sequence is encoded as a vector after one pass of the sequence, then conditional on the vector, the decoder generates an output sequence. The pain point is that the encoded vector is inflexible to abstract information effectively. When input sequence is long, quality degrades. Theoretically GRU/LSTM mitigates the problem but practically they still come short.


Here comes the attention mechanism. I will talk about attention [Bahdanau et al. 2014][Neural Machine Translation by Jointly Learning to Align and Translate] and self-attention [Cheng et al. 2016][Long Short-Term Memory-Networks for Machine Reading]. Both can be read in a key, value, query aspect.


[Learning Phrase Representations using RNN Encoder–Decoder for Statistical Machine Translation]: https://arxiv.org/pdf/1406.1078
[Sequence to Sequence Learning with Neural Networks]: https://arxiv.org/pdf/1409.3215.pdf
[Neural Machine Translation by Jointly Learning to Align and Translate]: https://arxiv.org/pdf/1409.0473.pdf
[Long Short-Term Memory-Networks for Machine Reading]: https://arxiv.org/pdf/1601.06733.pdf
