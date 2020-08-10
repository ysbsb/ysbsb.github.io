---
layout: post
title: 강화학습 Policy Gradient, REINFORCE, Actor-Critic, A2C, A3C
date:   2019-07-16 01:04:31
author: Subin Yang
categories: Reintorcement_learning
comments: true
use_math : true

---

>  Policy gradient 계열의 강화학습인 actor-critic 에 대해서 알아보고자 합니다. Actor는 policy를 학습하여 action을 알아내는 네트워크이며, Critic은 state의 value를 알아내는 네트워크 입니다. Monter-Carlo 기반의 REINFORCE 부터 시작하여, 이에 actor-critic을 기반으로 advantage function을 도입한 A2C, 비동기적인 asynchronous한 방법으로 A3C 방법을 살펴보도록 하겠습니다.

REINFORCE

참고논문: [https://papers.nips.cc/paper/1713-policy-gradient-methods-for-reinforcement-learning-with-function-approximation.pdf](https://papers.nips.cc/paper/1713-policy-gradient-methods-for-reinforcement-learning-with-function-approximation.pdf)

Actor-Critic

참고자료: [https://dnddnjs.gitbooks.io/rl/content/actor-critic_policy_gradient.html](https://dnddnjs.gitbooks.io/rl/content/actor-critic_policy_gradient.html)

A3C

참고논문

[Asynchronous Methods for Deep Reinforcement Learning](https://arxiv.org/abs/1602.01783)

----------------------------------

# Policy Gradient

Policy gradient는 Object function을 정의하고 이것의 gradient를 구하는 것이 핵심입니다. Object function을 value function에 대해서 policy의 parameter로 표현할 수 있는데, 이때 object function J를 최대화 시키는 parameter vector theta를 찾는 것이 목표입니다.

![PG11-b9bd71ca-f44f-431b-81dd-385639b70d22](https://user-images.githubusercontent.com/37301677/61230795-ba430280-a765-11e9-9adb-1403fbc7d157.png)

Object function의 gradient를 구하는 방법이 여러개가 있는데 그 중에서 REINFORCE라 불리는 Monte-Carlo Policy Gradient 방법과, Actor-Critic이라 불리는 Actor-Critic Policy Gradient 방법을 살펴보겠습니다.

----------------------------------

# REINFORCE

REINFORCE 알고리즘은 Monte-Carlo Policy Gradient 방법으로도 불립니다.

### **Objective function**

$$J(\theta)=E_{\pi_\theta}[r]  =\sum_{s\in S}d(s)\sum_{a\in A}\pi_\theta(s,a)R_{s,a}$$

$${\triangledown} _\theta J(\theta)=\sum_{s\in S}d(s)\sum_{a\in A}\pi_\theta(s,a)\triangledown_\theta log\pi_\theta (s,a)R_{s,a}=E_{\pi_\theta}[\triangledown_\theta log\pi_\theta(s,a)r]$$

### **Likelihood ratio**

$$\triangledown_\theta \pi_\theta(s,a) = \pi_\theta(s,a){\triangledown_\theta \pi_\theta(s,a) \over \pi_\theta(s,a)} = \pi_\theta(s,a) \triangledown_\theta log\pi_\theta(s,a)$$

### **Score function**

$$\triangledown_\theta log\pi_\theta(s,a)$$

앞선 식에서 instance value인 reward r을 long-term value 

$$Q^\pi(s,a)$$

로 바꿉니다.

### **Gradient of objective function**

$${\triangledown} _\theta J(\theta)=E_{\pi_\theta}[\triangledown_\theta log\pi_\theta(s,a)Q^{\pi_\theta}(s,a)]$$

Monte-Carlo 방법으로 episode를 가보고 모든 step에 대해나 reward를 기억해놓고 각 state에 대한 return을 계산합니다.

![Sutton_REINFORCE-d656e16b-07f3-4f88-ade8-948bd2a38a38](https://user-images.githubusercontent.com/37301677/61230829-d3e44a00-a765-11e9-8a00-50e4b2bba705.png)

-------------------------------------------

# Actor Critic

### REINFORCE 단점

기존 Monte-Carlo 방법에서는 Q function을 return을 통해서 가져올 수 있었습니다. 이러한 Monte-Carlo 방법은 high variance 문제가 발생하며, return을 구할 때 episode가 너무 길거나 복잡한 문제에서 오래 걸리거나 수렴하지 않을 수 있습니다.

### Actor-Critic으로 보완

이렇게 Q function을 구할 때 발생하는 high variance 문제를 보완하기 위하여 action-value function을 estimate하는 방법으로 critic을 도입합니다. Critic은 TD(0)의 방법으로 매 step마다 parameter를 업데이트 하여 function을 estimate 합니다.

$$Q_w(s,a) \approx Q^{\pi_\theta}(s,a)$$

### Actor-Critic 구조

Actor-Critic 구조는 크게 두개의 parameter를 update 합니다.

- Critic: action-value function (Q function)의 parameter w를 업데이트 합니다.
- Actor: critic에서 estimate한 Q function을 참고하여 policy parameter theta를 업데이트 합니다.

$${\triangledown} _\theta J(\theta)=E_{\pi_\theta}[\triangledown_\theta log\pi_\theta(s,a)Q_w(s,a)]$$

$$\triangle\theta=\alpha\triangledown_\theta log\pi_\theta(s,a)Q_w(s,a)$$

<img width="722" alt="rl-one-step-actor-critic-algorithm-285c3e1b-dc99-4f0a-b1ea-ea3ff368b22b" src="https://user-images.githubusercontent.com/37301677/61230900-ef4f5500-a765-11e9-851c-52d5cc970fe4.png">

---------------------------

# A2C - Advantage Actor Critic

## Baseline

Baseline을 사용하여 variance를 줄일 수 있습니다. Baseline은 state value function V를 사용하고, Advantage function A를 도입합니다.

$$B(s)=V^{\pi_\theta}(s)$$

$$E_{\pi_\theta}[\triangledown_\theta log\pi_\theta(s,a)B(s)]=\sum_{s\in S}d^{\pi_\theta}(s)\sum_a\triangledown_\theta\pi_\theta (s,a)B(s)=\sum_{s\in S}d^{\pi_\theta}(s)B(s)\triangledown_\theta\sum_{a\in A}\pi_\theta (s,a)=0$$

### Advantage function

Policy gradient를 advantage function A의 term으로 다시 표현할 수 있습니다. State value function을 일종의 평균으로 사용해서 현재의 행동이 평균적으로 얻을 수 있는 value보다 얼마나 더 좋은지 계산에서 variance를 줄일 수 있습니다.

$$A^{\pi_\theta}(s,a) = Q^{\pi_\theta}(s,a)-V^{\pi_\theta}(s)$$

$${\triangledown} _\theta J(\theta)=E_{\pi_\theta}[\triangledown_\theta log\pi_\theta(s,a)A^{\pi_\theta}(s,a)]$$

---

# A3C - Asynchronous Advantage Actor Critic

### Experience memory의 한계

이전에 나왔던 RL 같은 알고리즘 들은 공통점이 있습니다. Online RL agent가 보는 observed data의 sequence가 nonstationary하고 online하게 update 하는 경우 data가 상당히 correlated 되어있다는 것입니다.

 DQN에서는 agent의 data를 replay memory에 저장하고, 이러한 data를 batch와 random sample을 하여 보완하였습니다. 

Memory에 데이터를 넣어 사용하는 방법으로 nonstationarity를 줄이며 update를 decorrelate 할 수 있지만, 이 방법은 off-policy RL 알고리즘에만 적용할 수 있도록 한계를 둡니다. 

또한 experience memory는 메모리와 계산량을 더 필요로 하기도 하고, 실시간으로 update하는 off-policy 알고리즘에 대해 이전 policy로 부터 생성된 data를 사용하게 합니다.

### Asynchronous 방법의 제안 및 experience memory 보완

여러개의 agent를 병렬적으로 asynchronous(비동기적)으로 실행하도록 합니다. 

### 장점:

- 이러한 병렬처리는 주어진 time step 마다 병렬적인 agent가 다른 state에서 다양한 경험을 하기 때문에, agent의 data를 decorrelate 할 수 있습니다.
- On-policy 알고리즘 (such as Sarsa, n-step methods, actor-critic methods)과 Off-policy 알고리즘 (such as Q-Learning) 모두에 사용될 수 있어 하나의 아이디어로 더 많은 알고리즘에 사용될 수 있습니다.
- 하나의 standard multi-core CPU에서 실험할 수 있습니다. 이전 deep RL 연구들은 GPU와 같이 전문화된 하드웨어에 크게 의존합니다.
- Discrete, continuous, 2D game, 3D game 등에 모두 사용될 수 있습니다. 예를 들어, Continuous motor control task, visul input이 들어가는 3D maze 탐험 등을 학습하는 데에도 사용될 수 있습니다.

### Asynchronous RL Framework

Multi thread asynchronous 방법의 RL framework를 제시 합니다. 알고리즘의 목표는 policy에 reliable하게 deep neural network를 학습시키는 것이며, 학습 시 많은 자원을 요구하지 않도록 하는 것입니다.

알고리즘을 구성하는데에 두가지 main idea를 사용햐였습니다. 

- Asynchronous actor-learner를 사용합니다. 하나의 machine 에서 multiple CPU thread를 사용합니다.
- Observation을 얻을 때 multiple actors-learner가 병렬적으로 실행되어 환경의 다른 part를 exploring하도록 하였습니다. 다른 thread에서 각각 다르게 policy를 exploration 합니다. Multiple actor-learner에 의한 parameter로 부터 만들어진 변화들이 병렬적으로 online update에 적용되어 , single agent를 online update할 때 보다 덜 correlated 하게 만들 수 있습니다.

학습을 안정화할 수 있기 때문에, multiple parallel actor-learner를 사용하는 것이 다양한 이점이 있습니다.

- Parallel actor-learner의 수 만큼 linear하게 학습 시간을 줄일 수 있습니다.
- On-policy methods (such as Sarsa, actor-critic)에도 안정적인 방법으로 neural networks를 학습할 수 있도록 합니다. 학습을 안정화시키기 위한 방법으로 experience memory에 의존하지 않기 때문입니다.

### Asynchronous advantage actor-critic

크게 policy를 maintain하는 부분과 value function을 estimate하는 부분으로 이루어 집니다. 

Policy와 value function은 매 t action 또는 terminal state에 도달할 때 까지 update 합니다.

$$\triangledown_{\theta'}log\pi(a_t\mid s_t;{\theta'})A(s_t,a_t;\theta,\theta_v)$$

$$\sum_{i=0}^{k-t}\gamma^ir_{t+1}+\gamma^kV(s_{t+k};\theta_v)-V(s_t;\theta_v)$$

Update는 다음과 같은 advantage function으로 수행됩니다.

Policy에 대한 parameter는 다음과 같고

$$\theta$$

Value function에 대한 parameter는 다음과 같습니다.

$$\theta_v$$

Convolutional  neural network는 policy에 대해서 one softmax output이 다음과 같이 있고

$$\pi(a_t \mid s_t;\theta)$$

one linear output이 다음과 같이 있습니다.

$$V(s_t;\theta_v)$$

non-output layer는 share 됩니다.

Exploration을 improve하기 위해서 object function에 대한 policy pi에 대해서 entropy를 추가합니다. 이는 suboptimal deterministic policies에 수렴하는 것을 방지하기 위함입니다.

Entropy regularization term을 포함한 object function에 표현식은 다음과 같습니다. Hyperparameter beta는 entropy regularization term에 대한 strength control를 합니다. H is entropy.

$$\triangledown_{\theta'}\pi(a_t \mid s_t; \theta')(R_t-V(s_t;\theta_v))+\beta\triangledown_{\theta'}H(\pi(s_t;\theta'))$$

![a3c-alg-3-ac9a0268-b855-436a-9218-db8deb5e41d6](https://user-images.githubusercontent.com/37301677/61230954-04c47f00-a766-11e9-82a8-2b7cafeb07c6.png)
