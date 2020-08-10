---
layout: post
title: 강화학습 DDPG
date:   2019-04-02 22:29:30
author: Subin Yang
categories: Reintorcement_learning
comments: true
use_math: true
---

<h1>DDPG: Deep Deterministic Policy Gradient</h1>

<h3>Paper Review</h3>

<br>

<h2>1. 서론</h2>

DQN은 discrete 하고 low-dimensional action spaces에 대해서 다루는 문제들만 해결할 수 있다. 흥미롭게도, 대부분의 주목 할 만한 control task 들은, continuous (real valued) 하고 high dimensional action spaces를 가지고 있. DQN은 continuous valued case는 매 스텝마다 iterative optimization process가 필요한데, action을 찾는 방법이 action-value function을 최대화 하는 방법에 의존하고 있어서 continuous domain에 바로 적용하기 어렵다. 

DQN 같은 deep RL을 continuous domain에 적용하는 방법은 간단하게 action space를 discretize 하는 것이다. 하지만, 이것은 많은 한계가 있는데, 가장 주목 할 만한 것은 차원의 저주이다: 자유도의 수에 따라서 action의 수가 exponentially(지수적으로) 증가한다. 각각의 관절에 대해서 a가  –k, 0, k로 coarsest discretization된 7 자유도 시스템은 action space의 차원이 3의 7제곱이 된다. Action의 섬세한 control이 필요한 task에서 더 섬세한 grained discretization이 필요하고, discrete action의 수만큼 explosion을 이끌 수 있다. Action space의 naïve한 discretization은 문제 해결에 중요할 지 모르는, action domain의 구조에 대한 정보를 손실 시킬 수도 있다. 

Model-free, high dimensional에서도 policies를 학습할 수 있는 deep function approximator를 사용하는 off-policy actor-critic 알고리즘을 선보인다. 이 작업은 deterministic policy gradient 알고리즘에 기초한다. 한편, 이러한 actor-critic 방법의 neural function approximator를 포함한 naïve한 application은 challenging한 문제들에서 unstable 하다. 

Actor-critic 접근법을 Deep Q Network의 insight와 결합했다. DQN는 function approximator로 value function을 학습시킬 수 있다. 

1. Network는 샘플들 사이의 상관관계를 최소화하는 replay buffer로부터 얻은 샘플들을 통해 off-policy로 학습한다.

2. Network는 temporal difference backup을 하는 동안 고정적인 target를 줄 수 있도록, target Q network를 통해 학습한다. 

제안된 method를 평가하기 위해, 다양한 도전적인 physical control 문제들을 구성하였다. (complex multi-joint movements, unstable and rich contact dynamics, and gait behavior) 로봇 control에서 오랜 문제는 video 처럼 raw sensory input을 갖고 직접 action policy를 학습하는 것이었다. 따라서, 시뮬레이터 상에서 고정된 viewpoint의 카메라를 설치하였고, low-dimensional observation과 pixel을 통해 직접 학습하는 tasks들을 모두 시도하였다. 

제안된 model-free 접근법은 Deep DPG(DDPG) 라고 불리는, 같은 hyperparameter들과 network 구조를 사용하는 low-dimensional observations를 사용하는 모든 task에 대한 competitive policies들을 학습할 수 있다. 많은 경우에 대해서, hyperparameter들과 network 구조를 일정하게 유지하며, pixel로부터 직접 좋은 policies 들을 학습시킬 수 있다.

접근법의 주요한 특징은 간단함이다: 이것은 straightforward actor-critic architecture이며 적은 “moving parts”를 가진 학습 알고리즘인데, 더 어려운 문제들이나 큰 network들에 대해서 더 쉽게 implement 하고 scale 하기 쉽다. 

 

 

<h2>2. 이론적 배경</h2>

RL의 많은 접근법들은 Bellman equation의 recursive relationship을 이용한다. 

Target policy는 deterministic 하고, inner expectation을 피하기 위해서 이것을 function mu로 표현한다. 

기댓값은 오직 환경에 대해서만 의존한다. 이것은 stochastic behavior policy beta로부터 생성된 transitions를 이용하여, off-policy로 Qmu 를 학습시킬 수 있다는 것이다.

Q-Learning은 널리 이용되는 off-policy 알고리즘이며, greedy policy 를 이용한다. 우리는 loss를 최소화하여 optimize 하는 theta로 parameterized 된 function approximators를 고려한다. 

Use of replay buffer, and separate target network for calculating y

 

 

 

<h3>3. 알고리즘</h3>

Continuous spaces에서는 매 timestep 마다 a를 optimization을 요구하는 greedy policy를 찾아야하는데, Q-Learning에선 어려워서 적용하지 못 한다. 대신에, actor-critic 접근법에 기초한 DPG 알고리즘을 사용한다. 

DPG 알고리즘은 특정 action에 대해서 states를 deterministically 하게 mapping하는current policy를 명시하는, parameterized 된 action function mu를 유지한다. Critic Q는 Q-Learning을 안에 있는 Bellman Equation을 통해서 학습되었다. Actor는 actor parameter에 대한 start distribution J로부터 얻은 expected return에 chain rule을 적용하여 얻어진 것을 따르도록 업데이트 된다. 

 <br>

to be continued ...

<br>

<h1>DPG: Deterministic Policy Gradient</h1>

 

<h2>1. 서론</h2>

Policy gradient 알고리즘은 continuous actions spaces를 가진 RL 문제에서 널리 사용됩니다. 기본적인 아이디어는 다음과 같습니다.

Policy를 phi를 파라미터 벡터 theta를 통해서, stochastically하게(확률적으로) state s에서 action a를 선택하는 parametric probability distribution(파라미터를 포함한 확률 분포)를 가지고 표현하는 것이다.

(즉, policy phi를 확률 분포로 나타낼 수 있습니다. 이 확률 분포는 파라미터 벡터 theta에 대해서stochastic하게 action을 선택하는 분포입니다)

Poilcy gradient 알고리즘은 이러한 stochastic policy를 샘플링하고, reward를 더 좋게 하는 방향으로 policy parameter를 조절하는 것입니다.

 

이 논문에서는 stochastic polices 대신에, deterministic policies를 (a = function mu of s)고려하고 있습니다. Stochastic policies와 동일한 접근법을 가집니다. 

1. Policy gradient 하는 방향으로 policy parameter 조절

특징이 있다면, model-free 형식에서 action-value function의 gradient를 따라가는 모습을 보입니다. Deterministic policy gradient는 Stochastic policy gradient에서 policy variance가 zero가 되는 limit case라고 볼 수 있습니다.

 

Stochastic policy gradient의 단점이 있다면, action space가 many dimension을 가진 경우, SPG는 state와 action spaces를 모두 적분해야 하는데, DPG는 state space에 대해서만 적분해주면 됩니다.

Full state와 action space를 모두 explore하기 위해서, SPG는 굳이 사용할 필요가 없다. DPG 알고리즘의 explore satisfactorily 하는 것을 보장하기 위해서, off-policy learning 알고리즘을 도입합니다. 기본적인 아이디어는 다음과 같습니다. 

(적절한 exploration을 보장하기 위해서) Stochastic behavior policy에 따라서 action을 선택 하지만, Deterministic policy gradient에 대해서 학습을 진행합니다. (exploiting은 보다 효율적인 DPG 방법으로 한다.)

Differentiable function approximator를 사용하여 action-value function을 estimate하는 off-policy actor-critic 알고리즘을 derive하기 위해서 DPG를 사용하고, action-value gradient를 approximate하는 방향으로 policy parameters를 업데이트 하게 됩니다. 

\-      DPG를 사용하는 이유는 off-policy actor-critic 알고리즘을 유도하기 위함이다.

\-      Off-policy actor-critic 알고리즘은 action-value function을 estimate 한다. 

\-      Policy parameter들은 action-value gradient를 approximate하는 방향으로 업데이트 한다.

 

High dimensional task에서 DPG가 SPG보다 performance advantage가 있다는 것을 증명하기 위해서 벤치마크 문제들에 적용을 해보았다. (A high-dimensional bandit, a high dimensional task for controlling an octopus arm.)

업데이트를 하기 위한 computational cost가 선형이다. 

로보틱스 문제에서, (???)

 

 

<h2>2. 이론적 배경</h2>

 

<h3> 1. Model the problem as Markov Decision Process (MDP)</h3>

Write the performance objective as an expectation.

<h3>2. Stochastic Policy Gradient Theorem</h3>

<h3>3. Stochastic Actor-Critic Algorithms</h3>

Unknow true action-value function Qphi를 사용하는 대신에, parameter vector w가 포함된 action-value function Qw를 사용한다. 

Critic은 temporal-difference learning과 같은 policy evaluation 알고리즘을 사용하여, action-value function을 estimate 한다. 

일반적으로, true action-value function Qphi 대신에 function approximator Qw를 도입하는 것은 bias가 생기게 한다. 하지만 function approximator가 다음의 두 개의 조건을 만족한다면, bias는 없을 것이다. 

1)    Qw

(Compatible function approximator가 stochastic policy의 feature에 대해서 선형이다.)

2)    Parameters w가 mean-squared error를 최소화한다. 

(parameter는 이런 feature(특성)들에 대해서 Qphi를 estimate 하는 linear regression 문제에 대한 솔루션이다. Temporal-difference learning을 통해서 더 효율적으로 value function을 estimate 하는 Policy evaluation 알고리즘을 사용한다. )

<h4>4. Off-policy Actor-Critic</h4>

Distinct behavior policy로부터 샘플된 trajectories를 따라서 off-policy로 policy gradient를 estimate 합니다. Off-policy 셋팅에서, performance objective가 target policy에대한 value function으로 변형되고, behavior policy의 state distribution에 대해서 averaged 된다. 

이 방법은 gradient ascent가 수렴하는 local optima의 setㅇ르 preserve 할 수 있어서 좋다. 

Critic은 gradient TD Learning 방법을 가지고, trajectories에 대한 off-policy로 state-value function에 대해서 estimate 한다. Actor도 off-policy로, stochastic gradient를 ascent 하는 방향으로 policy parameter theta를 업데이트 한다 Actor와 Critic 둘 다 beta보단 phi에 대한 action을 고려하기 위해서 sampling ratio를 사용한다.  

 

<h2>3. Gradient of Deterministic Policies</h2>

이제 policy gradient framework를 deterministic polices에 대해서 확장해보자!

결과를 유도하고 이해하기 위한 몇 가지 방법들이 있다.

처음으로, DPG 형식 뒤에 informal intuition을 제공한다. 마지막으로, DPG가 SPG의 limiting case라는 것을 보여준다. 

 

<h3>1. Action-Value Gradients</h3>

Model-free RL 알고리즘들의 다수는 generalized policy iteration에 기초한다. (policy evaluation + policy improvement)

Policy evaluation 방법은 MC evaluation 혹은 TD Learning 방법으로 action-value function을 estimate 한다. 

Policy improvement 방법은 action-value function에 대해서 policy를 업데이트 한다. 일반적인 방법은, action-value function을 최대화 하는 greedy 방법이다. 

Continuous action spaces에서, greedy policy improvement 방법은, 모든 step에 대해서 global maximisation을 필요로 해서 문제가 있다. 대신에, 심플하고 계산적으로도 대체 가능한 것은, Q의 gradient 방향으로 policy를 움직이는 것이다. (Q를 globally maximizing하는 대신에)

매번 방문한 state s에 대해서, policy parameter theta는 gradient에 비례하여 업데이트 된다. 각각의 state는 policy improvement에 대한 다른 direction을 제안할 것이다. 이것들이 state distribution에 대한 기댓값을 취함을 통해서 평균이 내어진다.

 

이 방법은 distribution의 변화에 대한 account를 하지 않는데, 다음의 policy gradient theorem을 보면 state distribution을 계산할 필요가 없다. 위의 직관적인 업데이트 방법은, performance objective의 gradient를 따른다. 

(직관적으로는 paremeter theta를 업데이트, 정확한 유도는 performance objective 유도를 통한 policy gradient theorem으로 한다. )

 

<h3>2. Deterministic Policy Gradient Theorem</h3>

별첨의 증명을 통해 확인할 수 있다.

 

<h3>3. Limit of Stochastic Policy Gradient</h3>

Stochastic policy phi를 deterministic policy mu로 표현할 수 있다.

 

 

<h2>4. Deterministic Actor-Critic Algorithms</h2>

DPG theorem을 on-policy와 off-policy 둘 다로 유도해보자. On-policy 업데이트는 Sarsa critic을 사용하고, off-policy 업데이트는 Q-Learning critic을 사용한다. 

 

<h3>1. On-policy Deterministic Actor-Critic</h3>

일반적으로, deterministic policy는 적절한 exploration을 보장하지 못 하고, sub-optimal solution으로 이끌 수 있다. 그럼에도 불구하고, 이 첫 번째 알고리즘은 deterministic policy를 따르는 on-policy actor-critic 알고리즘이며, 얻어 갈 것이 있다. 하지만, deterministic policy 일지라도, 환경에 충분한 noise가 있어서 적절한 exploration을 보장한다면 더 유용할 것이다.

Stochastic actor-critic 처럼, deterministic actor-critic도 두 개의 구성요소가 있다. 

<Stochastic 이야기>

Actor가 action-value function의 gradient를 ascend 하는 동안, Critic은 action-value function을 estimate 한다. 세부적으로, actor는 Equation에서 stochastic gradient를 ascent 하면서, deterministic policy mu의 parameter theta를 조절한다.  Actor는 true action-value function Qw에 대신에 Differentiable action-value function Qmu로 대체한다. Critic은 적절한 policy evaluation 방법을 사용하여 action-value function을 estimate 한다. 

예를 들어, deterministic actor-critic 알고리즘에서, Critic은 action-value function을 estimate 하기 위해서 Sarsa 업데이트를 사용한다. 

 

<h3>2. Off-Policy Deterministic Actor-Critic</h3>

이제 stochastic behavior policy phi에 의해 생성된 deterministic target policy mu를 학습하기 위해서, off-policy 방법을 고려해보자.

이전에, behavior policy의 state distribution에 대한 평균으로, target policy의 value function에 대한 performance objective를 변형해보았다.

[Equation 15]

이 방정식은 off-policy deterministic policy gradient 방법을 의미한다.

이제, off-policy deterministic policy gradient의 방향으로 policy를 업데이트 하는 actor-critic 알고리즘을 발전시켜보자.

우리는 다시, true action-value function Qmu 대신에, differentiable action-value function Qw로 대체한다. Critic은 적절한 policy evaluation 방법을 이용하여, sampled policy beta에 의해 생성된 trajectories를 따르는 off-policy로써 action-value function Qw를 estimate 한다. 다음의 off-policy deterministic actor-critic(OPDAC) 알고리즘에서, Criticㅇ느 action-value function을 estimate 하기 위해서 Q-Learning 업데이트 방법을 사용한다.

Stochastic off-policy actor-critic 알고리즘이 actor와 critic 둘 다에 대한 샘플링을 사용한다. 한편, deterministic policy gradient는 action에 대한 integral을 제거하고, actor를 샘플링 하는 것을 피할 수 있다. 그리고 Q-Learning을 사용하면서, Critic에서 importance sampling을 피할 수 있다. 

 

<h3>3. Compatible Function Approximation</h3>

[설명 추가하기]

 

<h2>5. Experiments</h2>

[설명 추가하기]

 
