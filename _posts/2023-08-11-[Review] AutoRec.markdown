---
layout: post
title:  "[Review] AutoRec"
excerpt: "Paper review - Autoencoders Meet Collaborative Filtering"
date:   2023-08-11T2:49:00Z
categories:
    - Paper Review
tags:
    - RecSys
use_math: true

---

## ◽ Introduction
---
협업 필터링(Collaborative filtering, CF) 모델은 아이템에 대한 유저 선호도(e.g. 별점) 정보를 이용하여 개인화된 추천을 제공하는 것을 목표로 한다. 넷플릭스의 도전 덕분에 지금까지 다양한 협업 필터링 모델들이 제안되어 왔으며, 그 중 행렬 분해와 이웃 모델이 가장 많이 선택되었다.  
해당 Paper에서는 *AutoRec*이라는, 오토인코더 패러다임을 이용한 새로운 협업 필터링 모델을 제안한다. 오토인코더는 이미 vision과 speech 등의 태스크에서 우수한 성능을 보인 바 있다. AutoRec은 출력과 계산 방식에서 기존 모델보다 더 나은 성능을 보인다는 것이 입증되었다.  
(Abstract에서 AutoRec이 행렬 분해나 RBM-CF, LLORMA보다 낫다고 소개한다.)
- RBM : Restricted Boltzmann Machine - Boltzmann Machine을 좀 더 수월하고 덜 엄격하게 만든 모델  
- LLORMA : Local Low-rank Matrix Approximation - 행렬 분해에 앙상블 기법을 도입한 모델  

---

## ◽ The AutoRec Model
---
평점 기반 협업 필터링은 $m$명의 유저와 $n$개의 아이템, 부분적으로 관찰되는(partially observed) 유저-아이템 평점 행렬 $R \in \mathbb{R}^{m \times n}$ 을 가진다. 각 유저 $u \in U = \lbrace 1...m \rbrace$ 은 부분적으로 관찰되는 벡터 $r^{(u)} = (R_{u1}, ... R_{un}) \in \mathbb{R}^n$로 표현될 수 있다. 이와 유사하게, 각 아이템 $i \in I = \lbrace 1...n \rbrace$ 은 부분적으로 관찰되는 벡터 $r^{(i)} = (R_{1i}, ... R_{mi}) \in \mathbb{R}^m$로 표현될 수 있다.  
이번 연구의 목표는, 부분적으로 관찰된 $r^{(i)}$ 혹은 $r^{(u)}$를 입력값으로 받아 저차원의 잠재 공간으로 투영한 다음, 누락된 별점 값을 예측하여 재구성한 $r^{(i)}$ 혹은 $r^{(u)}$을 출력하는 아이템 기반/유저 기반 오토인코더를 디자인하는 것이다.  
$\mathbb{R}^d$ 차원의 벡터 집합 $S$와 특정 값 $k \in \mathbb{N}_{+}$가 주어졌을 때, 오토인코더는 이를 아래와 같이 풀 수 있다.  

수식이왜안돼애애애ㅐ애애애ㅐㅐㅐㅐㅐㅐㅐㅐㅐㅐㅐ


\displaystyle\sum_{r \in S} r-h(r; \theta)

$h(r; \theta)$는 입력 벡터 $r \in \mathbb{R}^d$이 복원된 모습이다.  
W는 복원 과정의 가중치, V는 인코더의 가중치, 뮤와 베타는 bias 값이 된다.  

![AutoRec](https://github.com/SubinJin98/SubinJin98.github.io/assets/116137904/9b68a492-1fe3-4981-83c1-9bddbc0838fe)

아이템 기반 AutoRec 그림으로, 각 $r$은 $W$와 $V$를 통해 신경망에 투영된다.  
$h(r; \theta)$는 입력 벡터 $r \in \mathbb{R}^d$이 복원된 모습이다.  
W는 복원 과정의 가중치, V는 인코더의 가중치, 뮤와 베타는 bias 값이 된다.  
파라미터 $\theta$는 역전파를 통해 학습된다.
위에서 본 AutoRec model은 아이템 벡터 r(i)에 오토인코더를 적용한 것과 비슷하지만, 두 가지 중요한 차이점이 있다.  
1. 아이템 벡터 r(i)는 오직 관찰된 input값과 연관된 가중치를 통해서만 역전파된다. 이는 MF와 RBM 기법에서 사용된 방법이다.  
2. 학습된 파라미터를 정규화해서 관찰된 평점의 과적합을 막는다. 공식을 봤을 때, 아이템 기반 AutoRec(I-AutoRec)의 목적 함수는 정규화 강도 $\ramda$가 0보다 클 때,  
수식붙여넣기....... 람다/2로 해서 정규화되는 부분이 중요함...  

왼쪽 행렬식의 뜻은 오직 관찰된 평점의 조합만 고려한다는 뜻이다. 유저 기반 AutoRec(U-AutoRec)은 r(u)에서 파생된다. 종합적으로, I-AutoRec은 2mk + m + k 파라미터 추정치를 요구한다. 주어진 학습 파라미터 세타햇에서, I-autorec은 유저 u와 아이템 i에 대해 아래처럼 예측한다:  
수식수식...  
그림을 봤을 때, 색칠된 노드들은 관찰된 평점에 해당하고, 실선 연결들은 입력값 r(i)에서 업데이트된 가중치에 해당한다.  
AutoRec은 기존의 CF 방식과 차별화된다. RBM 기반 협업 필터링 모델과 비교했을 때, 몇 가지 다른 점이 있다.  

나머지는.....저녁에하자




