---
layout: post
title:  "[Review] GAT"
excerpt: "Paper review - Graph Attention Networks"
date:   2023-08-05T1:25:00Z
categories:
    - Paper Review
tags:
    - GNN
use_math: true

---

## ◽ Introduction
---
CNN(Convolution Neural Networks)은 grid 구조의 데이터 형태를 가진 문제들 - 이미지 분류, 영상 분할, 기계번역 등을 성공적으로 해결해왔다.  
그러나 많은 문제들이 grid로 구성할 수 없는, 불규칙한 데이터로 표현된다. 3D meshes, social networks, telecommunication networks, biological networks 또는 brain connectomes 등이 그 예이다. 이와 같은 데이터는 주로 graph 형태로 표현할 수 있다.  
예전부터 신경망을 확장해 다양한 구조화된 그래프를 처리하려는 시도가 꾸준히 지속되었다. 초기 연구에서는 일방향 비순환 그래프(Directed acyclic graph)를 가진 도메인에 RNN(Recursive Neural Networks)을 사용했다. GNNs(Graph Neural Networks)은 순환 그래프, 일방향/쌍방향 그래프 등 보다 다양한 그래프를 다룰 수 있는 RNN의 일반화된 개념으로 2005년과 2009년에 소개되었다. GNN은 평형 상태(equilibrium)가 될 때까지 신경망을 따라 노드 속성에 따른 산출물을 전파하는 반복적인 프로세스로 구성된다. 이 아이디어는 이후 매 전파 스텝마다 게이트 순환 유닛(GRU, gated recurrent units)을 사용하는 방식으로 개선되었다.  
<br/>
최근 들어서는 Convolution을 그래프 영역에 일반화하는 것에 대한 관심이 증가하고 있다. 이후 발전은 두 가지로 구분된다.  
<br/>

**1. 스펙트럼 접근법(Spectral approaches)**  

그래프를 Spectral Representation로 표현하는 방법이다. 이 접근법에서 합성곱 연산(Convolution operation)은 라플라시안 그래프의 eigen decomposition을 계산하여 푸리에 영역에서 정의된다. 이는 intense한 연산과 non-spatially localized한 필터를 만들 수 있으며 세부적인 구조와 feature 특성을 반영하는 데 특화되어 있다. 그러나 연산의 특성으로 인해 새로운 그래프와 노드에 대응이 어렵다는 단점이 있다.  
<br/>
[Graph Laplacian 설명](https://ahjeong.tistory.com/14)
<br/>

**2. 비 스펙트럼 접근법(Non-Spectral approaches)**  

이 접근법에서는 합성곱을 그래프에 직접적으로 적용하여, spatially close한 이웃 그룹에 대한 연산을 수행한다. 이 접근법의 과제 중 하나는 서로 다른 크기의 이웃들 사이에서 잘 작동하고 CNN의 가중치 공유 속성을 잘 유지하는 operator를 정의하는 것이다. 경우에 따라 각 노드 차수에서 특정한 가중치 매트리스 학습을 요구하거나, 각 input channel과 이웃 정도에 대한 가중치를 학습하면서 전이 행렬(transition matrix)의 거듭제곱을 사용하여 이웃을 정의하거나, 고정된 수의 노드를 포함하는 이웃을 추출하고 정규화해야 하는 경우도 있다.  
대표적인 예시로 **GraphSAGE** 모델이 있다. 이 모델은 귀납적 방식(inductive manner)으로 node representation을 계산한다.
<br/>
해당 논문에서는 Graph 구조의 데이터에 node classification을 수행하기 위한 Attention 기반의 구조를 소개한다. GAT의 아이디어는 Self-Attention 전략에 따라 각 node의 이웃들에도 주의를 기울이며 hidden representation을 계산하는 것이다.  
<br/>
Attention 아키텍처에는 몇 가지 흥미로운 특성이 있다.  
1. node-neighbor 쌍에서 병렬화가 가능하므로 연산이 효율적이다.  
2. 이웃 노드에 임의의 가중치를 지정하여 degree가 다른 그래프 노드에 적용할 수 있다.  
3. 귀납적 학습(Inductive Learning Problem)에 직접적으로 적용이 가능하다. 따라서 본 적 없는, 완전히 새로운 그래프에서도 일반화가 가능하다.  
<br/>

## ◽ GAT Architecture  
---

**1. Graph Attentional Layer**

모델의 입력값(input)은 node feature의 집합인 $h = \lbrace {\vec h}_1, {\vec h}_2, ..., {\vec h}_N \rbrace, {\vec h}_i \in \mathbb{R}^F$ 로 정의된다. $N$ 은 노드 개수, $F$ 는 각 노드의 속성 개수를 뜻한다.  
이 레이어는 새로운 node feature ${h^\prime} = \lbrace {\vec {h^\prime}}_1, {\vec {h^\prime}}_2, ..., {\vec {h^\prime}}_N \rbrace, {\vec {h^\prime}}_i \in \mathbb{R}^{F^\prime}$ 를 생성한다.





