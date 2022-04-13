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
    * Attention Mechanism **만** 으로  Recurrent, Convolution 연산을 대신한 논문
* Encoder-Decoder 구성을 따르지만, Encoder/Decoder 내에 RNN, CNN 연산 없이 Attention ( + Feed-forward) 으로만 구성되어 있다. --> Fully Connected (FC) layer 집합체라고 볼 수 있겠다.
* 제안된 모델 구조
    * Self-Attention
        *  **동일한 문장 내** 각 단어들간의 유사도를 계산
    * Multi-Head Attention  (and Masked Multi-Head Attention)
        *  **여러 관점 (Multi Head) 반영이 가능토록 같은 문장에 대해 병렬로 Attention 수행**       
        * A head (head 하나) ~ A single attention 을 의미 
        * Masked Multi-Head Attention: Decoder 에서만 사용. Self-Attention 시 아직 예측하지 않은 출력 단어 는 마스킹 처리 (depend only on the known outputs). 
    * Positional Encoding: 
        * 각 단어 벡터에 위치 정보를 더하는 방식
        * RNN의 순차적인 처리 특성을 대체할 수 있다

## Transformer (NIPS, 2017) 3줄 정리
* RNN, CNN 연산 없이, Attention 으로만 구성된 알고리즘
* Self-attention 을 통해 동일 문장 (attention에 input 으로 들어오는 문장) 내 각 단어들의 유사도를 계산
* RNN, CNN 연산이 없어 고려되지 못하는 위치정보는 positional encoding 을 도입하여 해결.

---

# What is a Transformer?
* (2017, NIPS) Attention is all you need 논문에서는 Transformer 방법을 사용하여 번역기를 만듦
* A 언어로 쓰인 문장 -> Transformer (여러 개의 'encoder blocks & decoder blocks' - 논문에서는 각각 6개 사용) -> B 언어로 쓰인 같은 의미의 문장으로 출력
* Similar to the 'Convolutional Seq2Seq model', the Trnasformer does not use any recurrence. It also does not use any convolutional layers. 
* Instead the model is entirely made up of linear layers, attention mechanisms and normalization  

## 주요 개선 Point
* No Recurrent, No Convolution. Only Attention!
* 성능 향상: 기존 RNN 기반 / CNN 기반 알고리즘 대비 성능 향상
* 속도 개선: RNN 의 순차적인 처리 특성을 병렬 처리로 대체한다.
* 문제 개선: 긴 문장에서 성능이 저하되는 RNN 의 Long-Term Dependency 문제 개선

## The Transformer - Model Architecture
* Transformer 모델 구조
![image](https://user-images.githubusercontent.com/98376833/162879880-a7003657-e910-4e18-97e7-8b9ec367bf70.png)

* Transformer 모델 전체 개요
![image](https://user-images.githubusercontent.com/98376833/162950420-430d29f3-beac-4eda-bd96-358f56449b13.png)
    * 출처: https://blog.promedius.ai/transformer/

### Input Embeddings
* Source Tokens (Inputs) x = (x1,..., xn)  -> Standard Embedding Layer -> Positional Embedding Layer -> Embedded vector
* 1) Standard Embedding Layer 
    * The tokens are passed through 'a standard embedding layer'
    * ```python nn.Embedding(input_dim, hid_dim) ```
* 2) Positional Embedding Layer
    * As the model has no recurrent it has no idea about the order of the tokens within the sequence. We solve this by using a second embedding layer called a **positional embedding layer**.
    * (원본 논문) **static positional encoding** 사용 / (Modern Transformer like BERT) learned positional encoding (=positional embedding) 활용 가능: ```python nn.Embedding(max_length, hid_dim) ```
```python
### pytorch 
## __init__(self, ...)
self.tok_embedding = nn.Embedding(input_dim, hid_dim)
self.pos_embedding = nn.Embedding(max_length, hid_dim) # positional embedding: sin, cos 합 (Attention is All you Need, 2017) 방법 대신 학습 가능한 layer 사용 (BERT 에서 사용 한 방법) 할 경우

self.dropout = nn.Dropout(dropout)
self.scale = torch.sqrt(torch.FloatTensor([hid_dim])).to(device) # (paper): sqrt(d_model) where d_model : hidden vector dimension 

## forward(self, src, src_mask):
pos = torch.arange(0, src_len).unsqueeze(0).repeat9batch_size, 1).to(self.device) # pos = [batch size, srclen]

# src: Input of Encoder Layer
src = self.dropout((self.tok_embedding(src) * self.scale) + self.pos_embedding(pos)) # src = [batch size, src len, hid dim]
```

### Encoder Layer
* Embedded vector x' = (x'1, ..., x'n-> (self-attention) a Multi-Head Attention Layer -> (point-wise) a Position-wise fully connected feedforward layer -> (Encoder Output z = (z1, ..., zn))
* Employ '(add) a residual connection' around each of the two sub-layers (multi-head / position-wise feedforward), followed by '(norm) layer normalization'.
* 1) (Self-attention) Multi-Head Attention Layer
* 2) (Feed Forward) Position-wise fully connected feedforward Layer
* (paper) 논문에서는 인코더 블록 (Multi-Head Attention & Feed Forward) 을 6개 사용 (N=6, N is hyperparameter)


### Decoder Layer
* Similar to Encoder, however it now has two multi-head attention layers.
* Similar to Encoder, Employ '(add) residual connections' around each of the sub-layers, followed by '(norm) layer normalization'
* Target token y(m) (- shifted right; 한칸씩 순방향으로 이동해가며 하나 토큰씩 모델의 input으로 들어감) -> (self-attention) Masked Multi-Head Attention Layer (over the target sequence) -> (encoder-decoder attention) a Multi-Head Attention Layer (which uses _the decoder representation as the query_ and _the encoder representation as the key and value_) -> (Output of Decoder Layer) 
* 1) (Self-attention) Masked Multi-Head Attention Layer
    * self-attention 시 현재 위치의 이전 위치에 대해서만 attention 할 수 있도록 이후 위치에 대해서는 '-무한대'로 마스킹
    * (=) (paper) This masking, combined with fact that the output embeddings are offset by one position, ensures that _the predictions for position i can depend only on the known outputs at positions less than i_
* 2) (Encoder-Decoder attention) Multi-Head Attention based Encoder-Decoder Attention Layers
    * * 1)의 layer의 통과된 vector 중 query vector 만 가져오고, key , value vector 는 encoder block의 output에서 가져옴.
* 3) (Feed Forward) Position-wise fullly connected feedforward Layer
* (paper) 논문에서는 디코더 블록 (Self-attention & Encoder-Decoder attention ~ Feedforward) 을 6개 사용 (N=6, N is hyperparameter)

```python 
### Pytorch

# def __init__(self,...):
self.self_attn_layer_norm = nn.LayerNorm(hid_dim)
self.enc_attn_layer_norm = nn.LayerNorm(hid_Dim)
self.ff_layer_norm = nn.LayerNorm(hid_dim)

self.self_attention = MultiHeadAttentionLayer(hid_dim, n_heads, dropout, device)
self.encoder_attention = MultiHeadAttentionLayer(hid_dim, n_heads, dropout, device) # encoder-decoder attention
self.positionwise_feedforward = PositionwiseFeedforwardLayer(hid_dim, pf_Dim, dropout)


# def forward(self, trg, enc_src, trg_mask, src_mask):
## self attention
_trg, _ = self.self_attention(trg, trg, trg, trg_mask)

## dropout, residual connection and layer norm
trg = self.self_attn_layer_norm(trg + self.dropout(_trg)) # trg: [batch size, trg len ,hid dim]

## encoder-decoder attention
_trg, attention = self.encoder_attention(trg, enc_src, enc_src, src_mask) # MultiHeadAttentionLayer 클래스의 forward(query, key, value, mask = None) 함수  

## dropout, residual connection and layer norm

## position wise feedforward & dropout, residual connection and layer norm

# return trg, attention
```


### Softmax
* Output of Decoder Layer -> (linear) FCN (Fully Connected Network) & Softmax -> y(m+1) (predicted next-token probabilities)
* (paper) We also use the usual learned linear transformation and softmax function to convert the decoder output to predicted next-token probabilities.

## Attention (in Transformer)
### Self-Attention
* Input (of Self-Attention) 내 각 단어 (token) 의 vector 들끼리 **서로 간의** 관계가 얼마나 중요한지 집중 (attention)
* (0) Linear (내적)
    * query, key, value 로 사용할 vector 와 Weight vector (W(Q), W(K), W(V)) 의 곱을 통해 (Query, Key, Value) vector 생성
    * Query: (query 로 사용할 vector) @ W(Q)
    * Key: (key 로 사용할 vector) @ W(K)
    * Value: (value 로 사용할 vector) @ W(V)
* (1) Attention score (energy 라고도 표현) : (paper) Scaled Dot Product Attention Score 사용 
    * (kaye) Score(Q, K(j)) 
    * Query vector 는 문장 내 다른 단어들의 Key vector 와 곱해짐 (내적)
    * 이 때 자기자신과의 key vector 와도 내적 <-- Self-Attention
    * (scaled-dot product) 값 scaling을 위해 key vector의 크기의 제곱근으로 나눠줌. 이렇게 하면 softmax를 적용했을 때 합이 1이 된다.
    * ==> Score(Q, K(j)) : ( Q @ Transpose(K) ) / (sqrt (dimension of keys)) 
* (2) Attention distribution 구함 (attention 라고도 표현): softmax 취하기
    * (kaye) α(Q,K)
    * 주어진 query 에 대해 모든 key 와의 유사도를 각각 구하는 것.
    * ==> α(Q,K): softmax( (1)에서 구한 scaled-dot product )
    * *참고: Visualization of attention layer 볼 때 주로 확인하는 값이 이 값. α(Q,K)
* (3 - 최종) Attention Value: 
    * (kaye) Attention (Q,K,V)
    * (2)에서 구한 값과 Value vector 를 곱한 뒤 모두 더해줌
    * ==> Attention (Q,K,V): Sum of { α(Q, K) * V }

#### Self-Attention의 장점
* 내가 알고싶은 단어 (Query) 와 같은 문장 내 자기 자신을 _포함한_ 다른 단어들 (Key) 과의 관계를
* Self-Attention 의 query 와 key (& value) 연산을 통해
* 입력된 문장 내 단어들이 멀든 가깝든 관계를 유추할 수 있게 된다. 

#### (kaye) Seq2Seq with Attention (2015, ICLR) vs Self-Attention (2017, NIPS) 의 Q, K, V vector
* Seq2Seq with Attention (2015, ICLR): Query, Key, Value 를 decoder cell의 hidden state s, encoder cell의 hidden state h 값을 사용
    * Query: s(t) of decoder cell
    * Key, Value: all h of encoder cell
* Self-Attention (2017, NIPS): Query, Key, Value 를 attention layer의 'input' 을 통해서만 가져옴. 서
    * 예시 - (Encoder Block) query,key,value 모두 word embedded vector (X') 활용
    * Query vector: X' @ W(Q) where W(Q): weight vector of query, @: Matrix Production
    * Key vector : X' @ W(K)
    * Value vector : X' @ W(V)
    * Attention score 계산 시 Query, Key vector 간 내적이 이뤄지는데, 이 때 Query 는 자기 자신과의 Key 와도 내적

### Multi-Head Attention
![image](https://user-images.githubusercontent.com/98376833/162978531-c34b02d6-876f-4704-922e-b790fe41375b.png)
![image](https://user-images.githubusercontent.com/98376833/162979881-6cc5f934-4589-4de0-a1cc-e4939c1caa15.png)

* 여러 개의 parallel attention layers (heads 라고도 부름) 을 사용. 
    * (paper) We employ h = 8 parallel attention layers, or heads: 8 개의 attention layer 사용
*  각 attention layer 는 weight 를 공유 하지 않으며 (서로 다른 weight initialization 수행)
*  각 attention layer 의 output vector 들은 CONCAT 후 Linear 연산 수행 (또 다른 weight matrix 와 곱해짐) 

#### Multi-Head Attention 의 장점
* 여러 개의 서로 다른 representation subspace 를 가짐으로써 single-head attention 보다 문맥을 더 잘 이해할 수 있게 된다.

## Attention 외 사용한 모델 구조 
### Position-wise Feed-Forward Networks
* 각 Attention layers 를 통과한 값은 fully connected layer 로 들어간다. 
* (paper) FFN(x) = ReLU(X@W1 + b1) @ W2 + b2

### Positional Encoding (in Input Embeddings)
* Transformer (2017, NIPS) 방법에서는 no recurrence, no convolution 이기 때문에 the order of the sequence 를 반영하기 위한 positional encoding 사용
* Transformer (2017, NIPS) 논문에서는 sin 함수와 cos 함수의 합으로 static 하게 표현. 
* 추후 발표된 BERT (Bidirectional Encoder Representations from Transformers) 논문에서는 이 positionoal encoding layer 도 학습 가능하도록 함 (dynamic) 

## 그 외 사용된 Technique
### Beam search
* 항상 높은 예측확률을 갖는 token **만** 을 선택하면서 결과를 보는 것이 아닌. (<- Base Technique)
* Target token 예측 시, 예측할 token position 별로 (y(t)) 
* beam size n 개의 확률이 높은 token 후보 들에 대해 
* top beams m 번째 time step 가지의 경우를 모두 계산하면서 정답을 찾아가는 방법
* 단, beam size n, top beams m은 hyperparameter.

# Transformer based Model 
* BERT (Bidirectional Encoder Representations from Transformers, Google)
* DETR
* GPT-3 (Generative Pre-trained Transformer -3, OpenAI): 미국의 인공지능 연구기관 OpenAI 가 개발한 자연어처리 모델

## GPT-3
* (Origina - NLP 적용) GPT-3
   * e.g. 단어가 주어지면 다음에 나올 가장 적절한 텍스트 (말) 를 예측하여 문장 생성
   * e.g. 질의응답 (대화)
   * e.g. 인간의 텍스트 표현에 대한 레이아웃 디자인 (코딩 구현 등)
* (Image) 이미지 GPT - 자연어 처리 기술과 이미지분석이 만나는과정
   * e.g. 다음에 나올 픽셀을 예측해가며 그림을 완성
### DALL-E: GPT-3 기반의 DALL-E 
* 주어진 텍스트에 맞는 이미지를 그려냄
* DALL-E 의 학습방법
    * DALL-E 는 이미지를 단지 '레이블'로 학습 하지 않고 '설명문'으로 학습
    * 이를 바탕으로, 학습하지 않은 텍스트나 상황을 접해도 이미지를 그려낼 수 있게된다.


--- 

#### Reference LINK.
* (2017, NIPS) Attention is all you need: https://papers.nips.cc/paper/2017/hash/3f5ee243547dee91fbd053c1c4a845aa-Abstract.html
* Transformer pytorch Code: https://github.com/bentrevett/pytorch-seq2seq/blob/master/6%20-%20Attention%20is%20All%20You%20Need.ipynb
* Transformer는 이렇게 말했다, "Attention is all you need.": https://blog.promedius.ai/transformer/
