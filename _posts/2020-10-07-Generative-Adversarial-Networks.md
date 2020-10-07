---
layout: post
title: GAN 논문 리뷰 - Generative Adversarial Networks (NIPS2014)
subtitle: GAN paper review - Generative Adversarial Networks (NIPS2014)
category: [GAN]
tags: [GAN]
comments: true
use_math: true

---







> 안녕하세요. 모카의 머신러닝 입니다. 이번 포스팅은 GAN의 시초가 되는 Ian Goodfellow의 Generative Adversarial Networks 논문에 대해 리뷰합니다. 영어로 된 논문을 한글로 같이 해석하며 논문에서 의미하는 것이 무엇인지 같이 공부하며 이해하고 활용하실 수 있도록 준비해보았습니다. 

<br>



<em><strong>Generative Adversarial Networks, Ian Goodfello et al, NIPS 2014</strong></em>  [Paper link](https://arxiv.org/pdf/1406.2661.pdf)

논문에 대한 해석은 간결한 문장으로 표현하려고 합니다. 그림과 표는 모두 해당 논문을 참조하였습니다.

<br>





# Abstract

적대적인 과정을 통해 생성 모델을 추정하기 위한 새로운 프레임워크를 제안한다. 우리는 동시에 두 가지 모델을 학습한다: 데이터 분포를 캡쳐하는 generative model (생성자) G와, 샘플이 G보다 학습 데이터로부터 온 것인지 확률을 추정하는 discriminator model (판별자) D가 있다. 이 프레임워크는 두 명의 플레이어가 minmax game (최대최소 게임) 을 하는 것과도 같다.  임의의 함수 G와 D의 공간에서, G가 학습 데이터 분포를 포함하고 D가 모든 곳에서 1/2과 같을 때, 독특한 해가 존재한다. G와 D가 다층레이어 퍼셉트론으로 정의된 경우, 전체 시스템은 역전파를 통해서 학습될 수 있다. 학습 과정이나 샘플을 생성하는 과정에서 Markov chain이나 unrolled approximate inference network는 필요없다. 실험은 생성된 샘플의 질적, 양적 평가를 통해 프레임워크의 가능성을 증명한다. 

# 1. Introduction

딥러닝의 가능성은 자연적인 이미지들, 스피치가 포함된 오디오 파형, 그리고 자연언어 말뭉치 등 인공지능 응용분야에서 만나게 되는 다양한 종류의 데이터의 확률 분포를 표현하는 rich, hierarchical model을 발견하는 것이다. 더 나아가 딥러닝에서 대부분의 눈에 띄는 성공은 고차원의 풍부한 sensory input을 클래스 라벨에 맵핑하는 discriminative model과 연관되어 있다.  눈에 띄는 성공은 주로 특히 잘 행동하는 gradient를 가지는 piecewise linear unit을 사용하는 역전파와 dropout 알고리즘에 기반되어 있다. Deep generative model은 최대 가능도 추정이나 관련된 부분에서 많은 다루기 힘든 확률적 계산의 추정의 어려움 때문에, 그리고 생성 컨텍스트에서 piecewise linear unit의 이점을 활용하기가 어렵기 때문에 더 적은 임팩트를 가지고 있었다. 우리는 이러한 어려움들을 회피하는 새로운 생성 모델 추정 과정을 제안한다.

제안된 adversarial nets 프레임워크에서 생성자가 적대적으로 맞서 싸운다. 판별자는 샘플이 모델의 분포에서 온 것인지, 데이터의 분포에서 온 것인지 결정하는 것을 학습한다. 생성자는 위조 지폐를 만들고 이것을 발견되지 않고 사용하려고하는 위조 지폐범 팀과 유사하다고 생각할 수 있고, 판별자는 위조 지폐를 찾으려고 하는 경찰과 유사하다고 생각할 수 있다. 이 게임에서 경쟁은 위조 지폐가 진짜 지폐로부터 구분이 안 될 때 까지 두 개의 팀 둘 다 그들의 방법을 증진시키는 것이다.

이 프레임워크는 많은 종류의 모델과 최적화 알고리즘에 대한 특별한 학습 알고리즘을 생성할 수 있다. 본 논문에서, 우리는 생성 모델이 다층레이어 퍼셉트론을 통해 랜덤 노이즈를 통과하는 것에 의해서 샘플을 생성하는 특별한 경우를 탐험한다. 우리는 이러한 특별한 케이스를 adversarial nets라고 부른다. 이 경우에, 우리는 오직 매우 성공적인 역전파와 드롭아웃 알고리즘을 통해서 두 개의 모델을 학습시킬 수 있고, 오직 순전파를 통해서 생성모델로 부터 샘플할 수 있다. 대략적인 추정이라 마코브 체인은 불필요하다.

 

# 2. Related work

directed 그래피컬 모델들을 latent 변수들로 대체한 것은  restricted Boltzmann machines (RBMs)이나, deep Boltzmann machines (DBMs)이나 그들의 다양한 변화된 것들과 같은 latent 변수들이 있는 undirected 그래피컬 모델들과 같다. 이러한 모델들과의 상호작용은 랜덤 변수의 모든 상태에 대한 전역 합계/통합에 의해 정규화된 비정규화 잠재적인 함수들의 곱과 같다고 표현된다. 이런 quantity (분할 함수)와 이것의 그래디언트는 Markov chain Monte Carlo (MCMC) 방법을 통해서 측정될 수 있음에도 불구하고 사소한 경우를 제외하고는 모두 다루기 어렵다. Mixing은 MCMC에 의존하는 학습 알고리즘에 대해 중요한 문제를 제기한다.

Deep belief networks (DBNs)는 단일이 직접적이지 않은 레이어와 몇개의 직접적인 레이어들이 포함된 하이브리드 모델이다. 빠른 근사의 layer-wise 학습 기준이 존재하지만, DBN은 undirected와 directed 모델 모두와 관련된 계산상의 어려움을 겪는다.

Log-likelihood를 근사하거나 제한하지 않은 score matching과 noise-contrastive estimation (NCE)과 같은 대체 기준도 제안되었다. 둘 다 분석적으로 정규화 상수를 지정하기 위해서 학습된 확률 밀도가 필요하다. latent 변수들이 있는 몇개의 레이어로 된 많은 흥미로운 생성 모델들은, 다룰수 있는 비정규화된 확률 밀도를 유도하는 것이 불가능하다. denoising auto-enconders나 contractive autoenconder들과 같은 몇몇의 모델들은 RBMs에 적용된 score matching과 매우 비슷하게 룰을 학습한다. 하지만, 개별적인 discriminiative 모델을 맞추는 대신에, 생성 모델 스스로는 고정된 노이즈 분포를 샘플하는 것으로 부터 생성된 데이터를 판별하는데 사용된다. NCE가 고정된 노이즈 분포를 사용하기 때문에 학습은 모델이 심지어 관찰된 변수들의 작은 집합에 대한 대략적인 맞는 분포를 학습한 후에 급격하게 느려진다.

마지막으로, 몇몇의 기술들은 확률 분포를 명확하게 정의하는것에는 관여하지 않지만, 생성 머신이 바라는 분포로부터 샘플을 얻도록 학습하는 것에 관여한다. 이 접근법은 이러한 머신들이 역전파에 의해 학습되도록 설계될 수 있기 때문에 이점이 있다. 이 분야에서 유명한 최근에 연구들은 일반화된 denoising auto-encoder까지 넓히는 generative stochastic network (GSN) framework를 포함하고 하나는 generative Markov chain의 한 스텝을 수행하는 머신의 파라미터를 학습하는 파라미터화된 Markov chain을 정의하는 것처럼 보일 수 있다. adversarial nets는 생성하는 동안 피드백 루프가 필요하지 않기 때문에, 그들은 역전파의 성능을 증가시키지만 내부 피드백 루프에서 사용될 때 경계되지 않은 activation에서 문제를 가지는 piecewise linear 유닛들을 경감하는데 더 잘 가능하다. 역전파에 의해 생성 머신이 학습되는 더 최근의 예시들은 최근의 auto-encoding variational Bayes이나 stochastic backpropagation을 포함한다.

# 3. Adversarial nets

adversarial 모델링 프레임워크는 모델이 다층레이어 퍼셉트론일 때 가장 간단하게 적용할 수 있다. 데이터 x에 대한 생성자의 분포 Pg를 학습하기 위해서, 우린느 입력 노이즈 변수 Pz에 prior를 정의하고, 데이터 공간을 맵핑하는 것을 G(z, theta g)라고 표현하고, G는 파라미터 theta g를 가진 다층레이어 퍼셉트론에 의해 표현되는 미분가능한 함수이다. 우리는 또한 두 번째 단일 스칼라를 출력하는 다층레이어 퍼셉트론 D(x, theta d)를 정의한다. D(x)는 데어터가 Pg 보다는 x로부터 왔다는 확률을 의미하낟. 우리가 학습 예시들과 생성자로부터의 샘플 둘 다에 맞는 라벨를 할당하는 확률 분포를 최대화하기 위해서 D를 학습한다. 우리는 동시에 $\log (1-D(G(\boldsymbol{z})))$를 최소화하기 위해서 G를 학습한다.

다른 말로, D와 G는 value function V(G, D)라는 두 명의 플레이어가 있는 minmax game을 플레이한다.

![Screenshot from 2020-10-05 17-48-54](https://user-images.githubusercontent.com/37301677/95292108-cf94d700-08ab-11eb-9f8e-1220e018b4ca.png)

다음 섹션에서, 우리는 적대적 네트워크의 이론적인 분석을 제시하고, 본질적으로 예를 들어 non-parametric limit과 같이 하나가 G와 D가 충분한 가능성이 주어졌을 때 데이터 생성 분포를 회복하도록 하는 학습 기준을 보여준다. 덜 공식적이지만 접근법에 대한 더 교육적인 설명이 있는 Figure 1을 보자. 실제로, 우리는 iterative 하고 수치적인 접근법으로 game을 구현해야 한다. 학습의 내부 루프에서 D를 최적화하는 것을 끝내는 것은 계산적으로 안 좋고, 그리고 제한된 데이터셋에 대해 오버피팅의 결과가 나올 수 있다. 대신에, 우리는 k 스텝 동안 D를 최적화하고 한번의 step 동안 G를 최적화 하는 것을 번갈아했다. 이 결과로 D는 최적의 솔루션에 가깝게 유지가 되었고, 따라서 G도 충분히 천천히 변화했다. 이 전략은 SML/PCD training이 학습의 내우 루프의 일부에서 Markov chain이 burning 하는 것을 피하기 위해서 한번의 학습 단계에서 다음 학습 단게까지 Markov chain의 샘플을 유지하는 것과 유사하다. 과정은 공식적으로 algorithm 1에 제시되어 있다.  

실제로, 식 1은 G가 학습을 잘 하도록 충분한 그래디언트를 제공하지 못할 수 있다. 학습의 초기애서, G가 안 좋을 때는, D는 샘플들이 학습 데이터와 명확하게 다르기 때문에 높은 자신감을 가지고 샘플들을 거절할 수 있다.  이러한 경우에, $log(1-D(G(z))$ 는 포화된다. G가 $log(1-D(G(z))$ 를 최소화하도록 학습시키는 대신에, 우리는 G가 $logD(G(z))$ 를 최대화하도록 학습시킬 수 있다. 이러한 목적함수는 G와 D의 다이나믹스가 같은 고정점을 만들지만 학습의 초기에서 더 강력한 그래디언트를 제공할 수 있다.

![Screenshot from 2020-10-05 16-48-09](https://user-images.githubusercontent.com/37301677/95292242-0ff45500-08ac-11eb-80c6-06959ba07e23.png)

 

# 4. Theoretical Results

생성자 G는 암묵적으로 샘플들의 분포 $G(z)$ 가 $z$~$p_{z}$ 를 다를 때 확률 분포 $p_{g}$ 를 정의한다. 그러므로, 우리는 Algorithm 1이 $p_{data}$ 에 대한 좋은 측정기가 되도록 수렴하도록 한다. 이 섹션의 결과는 non-parametric 셋팅을 통해 수행되었다. (예를 들어, 우리는 확률 밀도 함수의 공간에서 수렴을 연구하여 무한한 능력을 가진 모델을 표현한다.) 

![Screenshot from 2020-10-05 18-25-13](https://user-images.githubusercontent.com/37301677/95292281-239fbb80-08ac-11eb-9e29-fd3e0dfe00ae.png)

## 4.1 Global Optimality of $p_{g}=p_{\text {data }}$

우리는 먼저 어떠한 주어진 생성자 G를 위한 최적의 판별자 D를 고려한다.

### Proposition 1.

G가 고정되었을 때, 최적의 판별자 D는 다음과 같다.

![Screenshot from 2020-10-07 14-49-27](https://user-images.githubusercontent.com/37301677/95292407-5c3f9500-08ac-11eb-97d5-334c653c8c6b.png)



증명. 어떠안 생성자 G가 주어졌을 때, 판별자 D를 위한 학습 기준은 quantity $V(G,D)$ 를 최대화하는 것이다.

![Screenshot from 2020-10-07 14-49-37](https://user-images.githubusercontent.com/37301677/95292408-5d70c200-08ac-11eb-9287-3a297391e06f.png)

실수 집합 안의 0이 아닌 어떠한 a,b에 대해서, 함수 $y \rightarrow a \log (y)+b \log (1-y)$ 는 0과 1사이에서 $\frac{a}{a+b}$ 일 때 최대값을 갖는다. 판별자는 $\operatorname{Supp}\left(p_{\text {data }}\right) \cup \operatorname{Supp}\left(p_{g}\right)$ 외부에서 정의할 필요가 없다. 

D를 위한 training 목적함수는 Y가 x가 $p_{data}$ (y=1 일 때) 또는 $p_{g}$ (y=0 일 때) 으로부터 오는 것을 암시할 때, 조건부확률 $P(Y=y \mid \boldsymbol{x})$ 를 추정하는 log-lilkelihood를 최대화하는 것으로 해석될 수 있음에 주목하자. 식 1에서 minmax game은 다음과 같이 재구성될 수 있다.

![Screenshot from 2020-10-07 14-49-46](https://user-images.githubusercontent.com/37301677/95292410-5d70c200-08ac-11eb-80de-9ca5f7b52ff6.png)

### Theorem 1.

가상의 학습 기준 $C(G)$ 의 글로벌 최저점에 도달하는 것은 $p_{g}=p_{\text {data}}$ 인 것과 필요충분조건이다. 해당하는 점에서, $C(G)$ 는 값 $-\log 4$ 에 도달한다.

증명. $p_{g}=p_{\text {data }}$이기 위해서, $D_{G}^{*}(\boldsymbol{x})=\frac{1}{2}$가 된다. 그러므로 $D_{G}^{*}(\boldsymbol{x})=\frac{1}{2}$일 때 식4를 조사하면, 우리는 $C(G)=\log \frac{1}{2}+\log \frac{1}{2}=-\log 4$ 임을 발견할 수 있다. 이것이 $C(G)$ 에 대한 가장 가능한 값을 보기 위해서, $p_{g}=p_{\text {data }}$ 일 때만 살펴본다.

![Screenshot from 2020-10-07 14-51-19](https://user-images.githubusercontent.com/37301677/95292545-9e68d680-08ac-11eb-9a67-b9898c943e4b.png)

임을 관찰하고, 이 식을 $C(G)=V\left(D_{G}^{*}, G\right)$ 으로부터 뺄 때, 우리는 다음과 같은 식을 얻을 수 있다.

![Screenshot from 2020-10-07 14-51-26](https://user-images.githubusercontent.com/37301677/95292547-9f016d00-08ac-11eb-8b1c-7fdca18b3a7b.png)

KL은 Kullback-Leibler divergence 이다. 우리는 이전의 표현에서 Jensen-Shannon divergence를 발견했다.

![Screenshot from 2020-10-07 14-51-34](https://user-images.githubusercontent.com/37301677/95292549-9f9a0380-08ac-11eb-8f2e-9fcb0f587a22.png)

두 분포 사이의 Jensen-Shannon divergence는 항상 음수가 아니고 분포가 같을 때만 0이기 때문에, 우리는 오직 해가 $p_{g}=p_{\text {data }}$일 때 $C^{*}=-\log (4)$ 는 $C(G)$ 의 글로벌 최저점이라는 것을 보일 수 있다. 즉, 생성 모델은 데이터 생성 프로세스를 완벽하게 복제할 수 있다.

## 4.2 Convergence of Algorithm 1

### Proposition 2.

만약 G와 D가 충분한 용량을 가지고 있다면, 그리고 Algorithm 1의 각각의 step에서, 판별자는 주어진 G에 대해서 최적점을 도달 할 수 있고, $p_{g}$ 도 향상된 기준에 대해서 업데이트 될 수 있다.

![Screenshot from 2020-10-07 14-52-35](https://user-images.githubusercontent.com/37301677/95292605-be989580-08ac-11eb-8a7b-00d9023ffc9c.png)

그리고나서 $p_{g}$ 는 $p_{data}$ 로 수렴된다.

증명. 위의 기준에서 수행된 $p_{g}$ 에 대한 함수 $V(G, D)=U\left(p_{g}, D\right)$  를 고려하자. $U\left(p_{g}, D\right)$ 가 $p_{g}$ 에서 convex 함에 주목하자. 최고 convex 함수의 부도함수(subderiatives)는 최대값이 달성되는 점에서의 함수의 도함수를 포함한다. 다른 말로, 모든 $\alpha$에 대한 $x$에서 $\begin{equation}
\sup _{\alpha \in \mathcal{A}} f_{\alpha}(x)
\end{equation}$ 이고 $f_{\alpha}(x)$ 가 convex 하다면, $\beta=\arg \sup _{\alpha \in \mathcal{A}} f_{\alpha}(x)$ 를 만족할 때 $\partial f_{\beta}(x)$ 가 될 것이다. 이것은 최적의 D와 이에 상응하는 G가 있을 때 $p_{g}$에 대한 gradient descent update를 계산하는 것과 같다. 이론1에서 증명된 유니크한 글로벌 최적점의 $p_{g}$에서 $\sup _{D} U\left(p_{g}, D\right)$ 는 convex이고, 그러므로 $p_{g}$에 대해 충분히 작은 업데이트를 하면, $p_{g}$는 $p_{x}$에 수렴한다.

실제로, adversarial nets는 함수 $G\left(\boldsymbol{z} ; \theta_{g}\right)$를 통해서 제한된 $p_{g}$ 분포 계열을 내타해고, 그리고 우리는 $p_{g}$ 대신에 $\theta_{g}$를 최적화한다. G를 정의하기 위해 다층레이어 퍼셉트론을 사용하여 파라미터 공간에 다층의 critical point 들을 도입할 수 있다. 하지만, 다층레이어 퍼셉트론의 우수한 성능은 실제로 그들이 이론적인 개런티가 부족함에도 불구하고 그것들은 사용하기에 합리적인 모델이라는 것을 제안한다.

# 5. Experiments

우리는 adversarial nets를 MNIST, Toronto Face Database(TFD), CIFAR-10을 포함하는 범위에서 학습했다. 생성자 네트워크는 ReLU와 sigmoid activation을 혼합하여 사용했고, 판별자 네트워크는 maxout activation을 사용했다. 판별자 네트워크를 학습 할 때 dropout이 적용되었다. 우리의 이론적 프레임워크에서 생성자의 중간 레이어에 dropout 과 다른 노이즈를 사용할 수 있다. 우리는 생성자 네트워크의 가장 마지막 레이어에만 노이즈를 사용했다.

우리는 G로 생성된 샘플에 Gaussian Parzen window를 핏팅하고 이 분포 하에서 log-likelihood를 reporting 함으로써 $p_{g}$ 하에서의 테스트셋 데이터의 확률을 예측할 수 있다. Gaussian의 $\sigma$ 파라미터는 validation set에서 교차 검증을 통해 얻어질 수 있다. 이 과정은 Breuleux et al에 의해 소개되었고, 정확한 likelihood가 다루기 힘든 다양한 생성 모델에 사용된다. 결과들은 Table 1에 제시되었다. Likelihood를 추정하는 이 방법은 약간 높은 분산을 가지고 높은 차원의 공간에서는 잘 작동하지 않지만 우리가 아는 선에서 가장 가능한 최고의 방법이다. 샘플링 할 수 있지만 직접적으로 likelihood를 추정할 수 없는 생성 모델의 발전은 이러한 모델을 어떻게 평가하는지에 추가 연구에 대한 동기를 준다.

Figure 2와 3에서 우리는 학습 후에 생성자로부터 샘플이 된 것들을 보여준다. 우리는 이러한 샘플들이 기존의 방법들에 의해 생성된 샘플들보다 좋다고 주장하지는 않지만, 우리는 이러한 샘플들이 적어도 문헌 상에서의 생성 모델과 비교할만 하고 adversarial 프레임워크의 잠재력을 강조한다고 믿는다.

# 6. Advantages and disadvantages

이 새로운 프레임워크는 이전의 모델링 프레임워크와 비교하여 장단점이 있다. 단점은 주로  $p_{g}(\boldsymbol{x})$ 명시적 표현이 없다는 것이고, 그리고 Boltzmann machine의 네거티브 체인이 학습 단계에서 최신 상태로 유지되어야 하는 만큼 D는 학습 동안 G와 동기화되야만 한다. 장점은 Markov chain은 절대 필요하지 않고, 오직 그래디언트를 얻기 위해 역전파가 사용되고, 학습 동안 추론이 필요 없고, 그리고 모델에 다양한 함수들이 통합될 수 있다. Table 2는 generative adversarial nets가 다른 generative modeling applications와 비교하여 요약한 것이다.

앞서 말한 장점은 주로 계산적이다. Adversarial model들은 생성자 네트워크가 데이터 예시들에 대해 직접적으로 업데이트 되지 않고, 판별자를 통해 흐르는 그래디언트로만 업데이트 된다는 점으로부터 몇몇 통계적 이점을 얻을 수 있다.  이것은 입력의 구성요소가 생성자의 파라미터에 직접적으로 복사되지 않는다는 것을 의미한다. Adversarial network의 또 다른 이점은 Markov chain 기반 방법은 모드간에 혼합도리 수 있도록 하기 위해서 다소 흐린 분포들을 필요로 하는 반면, adversarial network는 날카로운 분포들을 표현할 수 있다.

# 7. Conclusions and future work

이 프레임워크는 많은 간단한 확장을 허용한다.

1. Conditional generative model $p(\boldsymbol{x} \mid \boldsymbol{c})$ 는 G와 D 둘 다에 c를 더함으로써 얻어질 수 있다.
2. 대략적인 추론 학습은 x가 주어졌을 때 z를 예측하기 위해서 auxiliary network를 학습함에 의해서 수행될 수 있다. 이것은 wake-sleep 알고리즘에 의해 학습된 추론 네트웤크와 비슷하지만 추론 네트워크가 생성자 네트워크긔 학습이 끝난 후에 고정된 생성자를 위해 학습된다는 장점이 있다.
3. 모든 조건부 확률 $p\left(\boldsymbol{x}_{S} \mid \boldsymbol{x}_{\not \not}\right)$ 를 대략적으로 모델링 할 수 있다. S는 파라미터를 공유하는 조건부 모델의 집합을 학습에 의한 x 인덱스의 하위 집합이다. 본질적으로, adversarial nets를 사용하여 결정론적 MP-DBM의 확률적 확장을 구현할 수 있다.
4. Semi-supervised learning: 판별자 혹은 추론 네트워크로부터 나온 feature는 제한된 라벨이 데이터가 있을 때 분류기의 성능을 향상시킬 수 있다.
5. Efficiency improvements: 학습은 G와 D를 조정을 위한 방법을 나누거나 학습 동안 z를 샘플하기 위한 더 좋은 분포를 결정하는 것에 의해 가속화될 수 있다.

본 논문은 적대적 모델링 프레임워크의 실행 가능성을 입증하여 이러한 연구 방향이 유용할 수 있음을 시사한다.

