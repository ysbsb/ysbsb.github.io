---
layout: post
title: 강화학습 DPG
date:   2019-04-02 21:30:30
author: Subin Yang
categories: Reintorcement_learning
comments: true
use_math: true
---

<h1>DPG: Deterministic Policy Gradient</h1>

 <h3>Paper Review</h3>

<br>

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

 
