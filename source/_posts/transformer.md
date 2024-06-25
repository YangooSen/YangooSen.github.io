---
title: transformer
katex: true
date: 2024-05-13 09:08:42
excerpt: NLP with Transformer 笔记
tag: LLM
---
[《NLP with Transformers》](https://github.com/nlp-with-transformers/notebooks)的读书笔记，主要是学hugging face的一些东西和练英语,[The Annotated Transformer](https://nlp.seas.harvard.edu/annotated-transformer/)是模型的分步代码

# ch1 Hello Transformer

ULMFiT and Transformer were the catalysts for BERT and GPT

![timeline](chapter01_timeline.png)

![RNN](chapter01_rnn.png)

![encoder-decoder](chapter01_enc-dec.png)

The main idea behind attention is that instead of producing a single hidden state for the input sequence, **the encoder outputs a hidden state each step that the decoder can access**.

![attention](chapter01_enc-dec-attn.png)



The computation of RNN are inherently sequential and cannot be parallelized across the input sequence.

Compared with conditional attention, **both** the encoder and the decoder have their own self-attention mechanisms in Transformer and it dispenses with recurrence altogether.

![self-attention](chapter01_self-attention.png)



