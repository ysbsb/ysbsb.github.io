---
layout: post
title: Generative Adversarial Nets, GAN 논문 리뷰
subtitle: Generative Adversarial Nets, GAN paper review
date:   2020-03-19 15:49:49
author: Subin Yang
categories: GAN
comments: true
---

<strong><em>Generative Adversarial Networks, Ian J. Goodfellow et al</em></strong>

[Paper](https://arxiv.org/abs/1406.2661)

<br>

<h2>Generative Adversarial Nets</h2>

Generator

데이터 x에 대한 generator이 분포 $p_g 를 학습한다.  입력 노이즈 변수 $p_{z}(z) 에 대해 정의한다. 그 다음 data space $G(z;\theta__{g}) 로 맵핑을 표현한다.

G는 파라미터 $\theta_{g} 에 대해 미분가능한 MLP (multilayer perceptron) 층이다. 

Discriminator

Single scalar 값을 출력하는 두 번째 MLP 층을 정의한다. $D(x) 는 x가 $p_g 가 아닌 data distibution으로부터 왔을 확률을 나타낸다.

training examples와 G로 부터 생성된 sample 둘 다 정확하게 분류하기 위한 확률을 최대화하도록 D를 학습한다.

동시에 $log(1-D(G(x))) 를 최소화화도록 G를 학습한다.



결론적으로, Adversarial Net은 두 명의 플레이어가 Value function $V(D, G) 을 가지고 minmax game을 하는 구조로 설명이 된다.
$$
\min _{G} \max _{D} V(D, G)=\mathbb{E}_{\boldsymbol{x} \sim p_{\text {data }}(\boldsymbol{x})}[\log D(\boldsymbol{x})]+\mathbb{E}_{\boldsymbol{z} \sim p_{\boldsymbol{z}}(\boldsymbol{z})}[\log (1-D(G(\boldsymbol{z})))]
$$


<br>



![gan1](https://user-images.githubusercontent.com/37301677/77046216-e0602700-6a05-11ea-82da-77673dc28825.png)



discriminiator는 $p_x 와 p_g 사이에서 구분한다.

$p_x 는 data가 생성하는 분포에서 샘플된 것이고, $p_g는 generative 분포에서 샘플된 것이다.

z가 uniformly하게 샘플되고, $x=G(z)로 

그림 예시 및 업데이트 하는 과정

- $p_g 는 $p_data 랑 비슷하고 D는 정확한 분류기이다.

- D는 데이터로부터 샘플을 구별하기 위해서 학습된다. 

$$
D^{*}(\boldsymbol{x})=\frac{p_{\text {data }}(\boldsymbol{x})}{p_{\text {data }}(\boldsymbol{x})+\boldsymbol{p}_{g}(\boldsymbol{x})}
$$

- G를 업데이트 하고 나면, D의 gradient는 G(z)에 의해 데이터로 분류될 수 있는 가능성이 높은 지역으로 흐르도록 가이드된다.
- 몇 번의 학습 후에, G와 D의 용량이 충분하다면, 이들은 더 이상 성능이 향상되지 않는 부분이 있을 것이다. 왜냐하면 $p_g=p_data 가 되기 때문이다. 그러면 Discriminator는 두 개의 분포 사이에서 구분을 할 수 없을 것이다. $D(\boldsymbol{x})=\frac{1}{2}$







<br>

<h2>이론적 결과</h2>

1. minmax problem이 p_g=p_data 에서 global optimum을 갖는다.
2. 논문에서 제시하는 알고리즘이 global optimum을 찾는다.







어느 generator G가 주어졌을 때, 최적의 discriminator D를 고려한다.

가정 1. G가 고정되기 위해, 최적의 discriminator는 다음과 같이 된다.
$$
D_{G}^{*}(\boldsymbol{x})=\frac{p_{\text {data}}(\boldsymbol{x})}{p_{\text {data}}(\boldsymbol{x})+p_{g}(\boldsymbol{x})}
$$
ㅎㅎ

어느 generator G가 주어졌을 때 discriminator D를 위한 학습 기준은 quantity $V(G, D)$ 를 최대화 시키는 것이다.
$$
\begin{aligned}
V(G, D) &=\int_{\boldsymbol{x}} p_{\text {data }}(\boldsymbol{x}) \log (D(\boldsymbol{x})) d x+\int_{\boldsymbol{z}} p_{\boldsymbol{z}}(\boldsymbol{z}) \log (1-D(g(\boldsymbol{z}))) d z \\
&=\int_{\boldsymbol{x}} p_{\text {data }}(\boldsymbol{x}) \log (D(\boldsymbol{x}))+p_{g}(\boldsymbol{x}) \log (1-D(\boldsymbol{x})) d x
\end{aligned}
$$


ㅎㅎ





GAN이 생성한 이미지는 noise가 껴있고 incomprehensible하다.

black-box methods

뉴럴넷은 사람이 소비할 수 있는 알고리즘의 수준에서 네트워크가 무엇을 생성하는지 이해하기 어려운 black-box methods 라는 비판이 있습니다.

low resolution upsampling 힘듦




