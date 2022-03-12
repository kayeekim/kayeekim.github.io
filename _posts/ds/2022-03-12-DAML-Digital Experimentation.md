---
layout: post
title:  "[DA/ML] Data Science 실험 계획에 대한 고찰 - What is a Digital Experimentation"
excerpt: " Data Science 문제 정의에 대한 생각/ DS 실험 계획법 "

categories:
  - ds
tags:
  - [DA/ML]

toc: true
toc_sticky: true
 
date: 2022-03-12
last_modified_at: 2022-03-12

---

우연히 DS 서비스를 기획하고 있는 선배와 교류할 기회가 생겼다. 같은 DS 산업 종사자로서, 선배가 하는 일은 일종의 실험 계획 방법 이라고 느꼈다.  복기할 수 있도록 들었던 내용과 개인적으로 이해한 내용을 바탕으로 글을 남겨 보았다.

_출처를 밝힐 수 없어 , Copy를 엄격히 금지합니다_

## Summary
1. 고찰
2. Digital Experimentation 을 계획하는 방법을 알아보자
* (1) 가설 정의
* (2) Objective 정의
* (3) Target Metric 
* (4) Test
* + (5) 가설검정


## 고찰
본인이 해왔던 업무는 데이터분석 문제를 풀기위한 방법, 즉 분석 문제 해결방법에 집중해있는 경우가 많았다. User로 부터 해결이 필요한 Problem 을 전달받으면 (배경목적), "이 배경목적을 구체화하고 관련된 데이터를 정의/수집/전처리하여 알맞은 분석알고리즘을 찾아 분석을 수행하고 결론을 내는 cycle"로 업무를 수행하였으다 (본인이 암묵적으로 가지고 있던 Cycle로, 개인 멘토링을 진행할 때 제안해주는 Cycle이기도 하다). 특히 본인의 역할은 전처리와 적절한 분석알고리즘을 찾고 이를 검증하는데 집중해 있었다. 

오늘 선배로부터 들은 건 이보다 더 general 한, 좀 더 포괄적인 문제 해결 접근방법이다. DS/AI 서비스를 기획하기 위한 접근방법, User 가 무엇을 원할까부터 정의 하기 위해선 어떤 순서로 접근하는지의 방법을 설명해주셨다고 생각한다. 매우 흥미로운 주제였고 본인이 ds업무를 하면서 base로 깔고가야할 지식이라는 생각이 들었기에, 언제든지 다시 찾아보기 위해 필기한 내용을 위주로 한번 정리해 보았다.  


## Digital Experimentation 계획 순서
### (1) 가설 정의 - Hypothesis를 세우자. 

### (2) Objective 정의 - Objective 는 무엇일까 __Objective Function__ 
* Increase revenue/profit
* Increase conversion rate
* Increase customer sign-ups
* Increase repeated visits
* Decrease ...

### (3) 정량적 지표 정의 - Target Metric 은 무엇일까
-. Objective 를 달성하기 위해 Measurable 한 Metric을 define 해보자

-. Metric 은 아래 관점으로 고려해볼 수 있다. 
local optimization 이 아닌 global optimization 을 찾기 위해 여러가지 관점으로 고려해보자. 

* Primary metric : 가장 우선적으로 목표로하는 정량적 지표
* Secondary metric : 차선책으로 고려해야할 것은?
* Guardrail : trade-off 를 고려. Primary metric 은 하나 더라도 guardraild 은 여러개 일 수 있다. 
* Composite metric : Primary metric OR Secondary metric 의 balancing 을 어떻게 맞출까? 
e.g. f1-score : precision/recall을 balancing 한 measure

### (4) TEST 수행 
#### (4)-(1) A/B test (Randomized Experiments)
*  Find 'causal' relationship between treatment and outcome. 
e.g. Font size 를 키웠더니 (treatment) ->  Sales가 늘었다 (outcome). 
* 기능: Randomization minimizes the impact of confounding variables; 실제 영향을 끼치는 숨겨진 변수의 영향력을 최소화 할 수 있다.
* 장점: Low Cost - Very cheap to opterate in digital environment.

----
### (5) Hypothesis testing
-. how confident can we be about the causal relationship X->Y?

* 가장 쉬운 방법은 Probablity를 활용하는 것. i.e. Prob(X->Y is true)?
* Probablity도 중요하지만 Effect 도 중요함. i.e. Expected Value, Variance 

#### (5)-(1) Frequentist approach
* Set up a null hypothesis. e.g. $H0: Y_treatment = Y_control$
* Observe data
* Calculate p-value = P(data|H0)  
*P-value: Measures how likely we observe this data assuming H0 is true, or there is no treatment effect.
* Given significance level (normally 0.05)

#### (5)-(2) Bayesian approach
-. Frequentist approach 와 다른 점은 H0 을 가정하지 않는다는 것이다. Bayesian approach는 prior model 을 가정한다.
* Set up a prior distribution (or model) for target metric
* Observe data
* Obtain posterior distribution
* Calculate output metrics like Prob (Y_treatment > Y_control) and expected loss based on the posterior distribution 
*_Posterior distribtuion은 updated model이라고 볼 수 있겠다._ 

-. Ex. conversion rate (전환률) 를 p라고 하자. 
* Prior : p ~ Beta(a,b)
* Data: $X_1, ..., X_N ~ Bernoulli(p)
* Posterior: p |X~ Beta(a+Y, b+N-Y) where Y is SUM(X) or the number of conversions.

#### (5)-(3) Frequentist vs Bayesian
_참고: ML 문제의 경우 실제 문제를 고려할 때, Bayesian approach 접근법을 선호하는 경우가 많다고 한다_

##### Frequentist
* Easy to calculate p-value; p-value 계산이 쉬움
* p-value is difficult understand and as no business value; p-value는 해석이 어려움
* We cannot make an informative conclusion from insignificant test
* Test is not valid without fixing the sample size; sample size 고정이 필요
* Bigger sample size is required; 큰 sample-size를 요구

##### Bayesian
* Requires more expertise in statistics to calculate output results
* Results have clear business value and are easy to understand
* We always get valid results and can make at least an informed decision; data set 크기가 작더라도 정보를 뽑아낼 수 있음
* Fixing the sample size is not required; sample 을 고정하지 않아도 된다. 
* Smaller sample size is sufficient; 적은 sample size로도 충분하다.

----

#### (4)-(2) (TEST 수행) Multi-Armed Bandit (Bandit Selection)
-. _Experimentation 이라기 보단 Optimization Algorithm 에 가깝다고 할수도 있겠다._

-. Exploration vs Exploitation 
* Without any prior knowledge, we start with random exploration of each variation
* As we collect more data get more confident about the variant's performance, we can show better performing variation to more customers
* Too aggressive exploitation can lead to suboptimal solution
* How to balancing exploration vs exploiation?

-. 핵심 Idea
* 잘 모르겠으면 일단 exploration - 일단 다 해보고 (random exploration of each variation), 정보를 collect 하자
* knowledge 가 쌓이면, exploitation - 이 문제의 variation이 보이면 (e.g. 특정 treatment가 가장 잘 작동한다), 해결방법을 찾는다 (e.g. objective 의 balancing 을 부여하기 시작한다.)

##### Multi-Armed Bandit의 Algorithms
-. Thompson Sampling
* 일종의 확률 sampling - Traffic allocation for a variation A=Prob(A is winner)
* Bayesian model is widely used

-. ε-greedy (epsilon-greedy)
* Always explore with probality of ε (e.g. 5%).

-. Upper Confidence Bound (UCB)
* Choose a variation with the highest confidence bound.
* As a result, we either choose the true winning variation or reduce the bound for better decision making for next trial or customer 

#### (4)-(3) (TEST 수행) Contextual Bandits
-. Bandit 에 대한 개념을 좀 더 personalize 하게 만든 것

-. Bandit 이라는 단어가 들어가면 deterministic 하게 본다기 보단, explonary 하게 봐보자. _continuous 한 환경을 좀 더 고려한?. 
우리가 취한 action이 많은데, 처음엔 잘모르니 random으로 update해보고 (explore), 시간에 따라 경험치가 쌓이면 원하는 방향쪽으로 update (exploit) 
-> 또 모르겠으면 explore <-> 경험치 쌓여서 감잡히면 exploit _

-. _최근 hot한 알고리즘이라고_ 

-. 핵심 Idea
* Global 이 아닌 Personalized 한 선택을 하도록 - _환경에 따라 conditional 한 결과를 가져오는 모델을 만들도록_
* e.g RL모델이라고 하면, Reward is **conditional** to the state of the environment.


-----
