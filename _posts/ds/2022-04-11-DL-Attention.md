---
layout: post
title:  "[DL] Attention: Neural Machine Translation by Jointly Learning to Align and Translate (ICLR, 2015)"
excerpt: "Deep Learning - NLP - Attention"

categories:
  - ds
tags:
  - [DL] 

toc: true
toc_sticky: true
 
date: 2022-04-11
last_modified_at: 2022-04-12


---

# Attention 1줄 정리
* 단어 간의 유사도를 계산하여 관계성이 깊은 단어에 가중치를 부과한다.
* (=) 주어진 Query 에 대해 모든 Key 와의 유사도를 계산 후, 해당 유사도와 Value 를 가중평균한다. 이 방법을 통해, 최종적으로 Query 에 대한 영향력이 높은 (Key, Value) 를 고려하게 된다.
 
## 세부 Summary 
* seq2seq with attention: (2015, ICLR)  Neural Machine Translation by Jointly Learning to Align and Translate
* 'y(t)의 feature vector s(t)' 와 각 x = (x1, ..., x(Tx)) 의 feature vector h = (h1, ..., h(Tx)) 들과의 유사도 (attention score) 를 계산해서 
* 해당 유사도와 각 x의 feature vector (hj)와의 weighted sum을 context vector 로 활용한다.
* 참고: 최종 예측 시, '< s(t), h (모든 h)> 와의 유사도를 반영하고 있는 context vector c(t)' 와 s(t) 를 함께 활용하여 y'(t)를 예측. 
    * e.g. softmax (activate (W @ activate([W @ CONCAT[c(t), s(t)] + b) + b))) *@: matrix product

# What is a Attention
## 기본 IDEA
* 단어 간의 유사도를 계산하여 관계성이 깊은 단어에 가중치를 부과한다.
* decoder에서 출력 단어를 예측 (y_t) 하는 매 시점 time step 마다, encoder에서의 전체 입력 문장 (all x = (x1, ..., x(Tx))을 참고. 
* 이 때, 전체 입력 문장 참고시 x1, ..., x(Tx)를 동일 비율로 참고하는 것이 아닌 해당 시점 t에서 예측해야할 단어 y(t) 와 연관이 있는 입력 단어 부분을 좀 더 집중 (attention) 해서 참고해온다.

## 참고) Key-Value 자료형
* e.g. python dictionary {key: value}

## Attention Function
> Attention (Q, K, V) = Attention Value

![image](https://user-images.githubusercontent.com/98376833/162962704-aa92ba39-13f2-4579-8884-a70876d87296.png)
    * 그림출처: 딥 러닝을 이용한 자연어 처리 입문 15. 어텐션 메커니즘

* (1) 주어진 query 에 대해 모든 key 와의 유사도를 각각 구함
* (2) 구해낸 유사도 (1) 를 key 와 맵핑되어있는 각각의 Value 에 반영
* (3) 유사도가 반영된 Value (2) 를 모두 더하여 리턴 **== Attention Value**

## Seq2Seq with Attention 모델 (Bahdanau et al., 2015, ICLR) 
* Q (Query): t 또는 i (paper 에서는 i-1 이지만 여기서는 편의를 위해 i 라고 표기) 시점의 decoder cell의 hidden state $s_t$ OR $s_i$ *(이하 s(i)라고 표기)
* K (Keys): _모든 시점_ 의 encoder cell의 hidden states h(j) where j=1,...,Tx
* V (Values): _모든 시점_ 의 encoder cell의 hidden states h(j) where j=1,...,Tx

### Score(Q, K(j)) OR Energy: Attention Score a(s(i), h(j)) 
* energy 라고도 표현
* (paper) CONCAT 방식: a(s(i), h(j)) = $W_a @ tanh(W_b@[s(i); h(j)]) = W_a @ tanh(W_b@s(i) + W_c@h(j))
* Dot-product 방식: a(s(i), h(j)) = Transpose(s(i))@h(j) 

### α(Q, K) OR Attention: Attention Distribution (softmax 함수 사용) α(i)
* α(i) = softmax(e(i)) where e(i) = { a(s(i), h(1)), ..., a(s(i), h(Tx)) }
* α(i,j) = softmax{ e(i,j) | e(i,1), .., e(i,(Tx)) }
* ---> (1) 주어진 query 에 대해 모든 key 와의 **유사도**를 각각 구함
* target token i 와 연관이 있는 각 source token j 에게 얼마나 집중 (attention) 할지를 표현 (유사도; Attention value를 계산하기 위한 일종의 weight 역할)
* *참고: Visualization of attention layer 볼 때 주로 확인하는 값이 이 값. α(Q,K) 

### Attention (Q, K, V) OR Context Vector: Attention Value c(i)
* 인코더의 문맥을 포함하고 있다하여 context vector 라고도 불림
* c(i) = Sum of { α(i,j) * h(j) }, j=1,..., Tx
* --->  α(i,j) * h(j):  (2) 구해낸 유사도 (1) 를 key 와 맵핑되어있는 각각의 Value 에 반영
* ---> Sum of (2): (3) 유사도가 반영된 Value (2) 를 모두 더하여 리턴

## Seq2Seq vs Seq2Seq with Attention
* Seq2Seq: encoder 의 마지막 hidden state 를 context vector 로 사용
* Seq2Seq with Attention: (hidden state s(i) of decoder for y(i)), (hidden state h(j) of encoder for x(j)) 에 대한 유사도와 h(j) 와의 (모든 h = (h1,..., h(Tx)) 에 대한) weighted sum 을 context vector 로 사용

---
#### Reference
* (2015, ICLR)  Neural Machine Translation by Jointly Learning to Align and Translate
* (Attention mechanism, + Dot-product Attention) 딥 러닝을 이용한 자연어 처리 입문 15. 어텐션 메커니즘 :https://wikidocs.net/22893
