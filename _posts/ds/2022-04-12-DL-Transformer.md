---
layout: post
title:  "[DL] Transformer: Attention is all you need (NIPS, 2017)"
excerpt: "Deep Learning - NLP - Transformer"

categories:
  - ds
tags:
  - [DL] 

toc: true
toc_sticky: true
 
date: 2022-04-12
last_modified_at: 2022-04-12


---

## Summary 
* No Recurrence and No Convolution --> add "Positional Encodings" to the input embeddings at the bottoms of the encoder and decoder stacks (paper)

# What is a Transformer?
* (2017, NIPS) Attention is all you need 논문에서는 Transformer 방법을 사용하여 번역기를 만듦
* A 언어로 쓰인 문장 -> Transformer (여러 개의 'encoder blocks & decoder blocks' - 논문에서는 각각 6개 사용) -> B 언어로 쓰인 같은 의미의 문장으로 출력
* Similar to the 'Convolutional Seq2Seq model', the Trnasformer does not use any recurrence. It also does not use any convolutional layers. 
* Instead the model is entirely made up of linear layers, attention mechanisms and normalization  

## The Transformer - Model Architecture
![image](https://user-images.githubusercontent.com/98376833/162879880-a7003657-e910-4e18-97e7-8b9ec367bf70.png)

### Input Embeddings
* Tokens (Input)  -> Standard Embedding Layer -> Positional Embedding Layer -> ...
    * 1) The tokens are passed through 'a standard embedding layer'
        * ```python nn.Embedding(input_dim, hid_dim) ```
    * 2) as the model has no recurrent it has no idea about the order of the tokens within the sequence. We solve this by using a second embedding layer called a **positional embedding layer**.
        * ```python nn.Embedding(max_length, hid_dim) ```

### Encoder Layer
* ... -> Multi-Head Attention Layer ->



--- 

#### Reference LINK.
* (2017, NIPS) Attention is all you need: https://papers.nips.cc/paper/2017/hash/3f5ee243547dee91fbd053c1c4a845aa-Abstract.html
* Transformer pytorch Code: https://github.com/bentrevett/pytorch-seq2seq/blob/master/6%20-%20Attention%20is%20All%20You%20Need.ipynb
* Transformer는 이렇게 말했다, "Attention is all you need.": https://blog.promedius.ai/transformer/
