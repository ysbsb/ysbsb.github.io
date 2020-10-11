---
layout: post
title: The Lottery Ticket Hypothesis Finding Sparse, Trainable Neural Networks 논문 리뷰
subtitle: Deep Learning Neural Network Pruning paper review
date:   2020-04-21 10:00:00
author: Subin Yang
categories: pruning
comments: true
---





> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Fpruning%2F2020%2F04%2F21%2FLottery-ticket-hypothesis.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> <strong><em>The Lottery Ticket Hypothesis: Finding Sparse, Trainable Neural Networks, Jonathan Frankle et al, ICLR 2019</em></strong>
>
> [Paper](https://arxiv.org/pdf/1803.03635.pdf)



<br>

<h2>The Lottery Ticket Hypothesis 방법 요약</h2>
<h4>1. 이론적 배경: Pruning</h4>
뉴럴넷 프루닝은 학습된 네트워크의 90퍼센트 이상의 파라미터 수를 줄이고, 필요한 저장 용량을 줄이고, 정확도 감소 없이 inference의 계산 성능을 증가시킬 수 있습니다.

<h4>2. 일반적인 pruning의 문제</h4>
프루닝에 의해 제안된 새로운 sparse 구조는 학습 성능을 높이도록 처음부터 학습시키기는 어렵습니다. 일반적인 pruning 기술은 학습을 효과적으로 할 수 있도록 잘 초기화(initialization)된 subnetwork를 포함하지 않기 때문입니다.

<h4>3. 방법 제시: The Lottery Ticket Hypothesis</h4>
위에 언급된 결과를 바탕으로, 저자들은 새로운 방법을 제안합니다. <em>The Lottery Ticket Hypothesis</em>는 dense, randomly-initialized, feed-forward 네트워크는 비슷한 iteration 수 일 때 테스트 성능이 원래의 네트워크와 비슷한 성능의 subnetwork (winning ticket)을 가지고 있다라는 것입니다. 우리가 찾은 winning ticket은 initialization lottery에 당첨된 것과 같다고 표현합니다. 이러한 네트워크는 학습을 효과적으로 할 수 있도록 하는 초기화된  weight을 가지고 있을 것 입니다.(어떠한 랜덤한 subnetwork 중에는 원래의 네트워크와 테스트 성능이 비슷한 네트워크가 적어도 하나 있을 것이다. 이런 좋은 subnetwork가 뽑히는 경우가 복권의 방법처럼 생각한 것 입니다.)  

<h4>4. 결과</h4>
Lottery ticket hypothesis를 따르는 알고리즘을 제안합니다. 결과적으로 우리가 찾은 winning ticket은 원래의 네트워크보다 빠르게 학습하고 더 높은 테스트 성능을 보입니다.



<br>



<h2>Methods</h2>

MNIST에서 fully-connected 네트워크와 CIFAR10에서 covolutional 네트워크로부터 랜덤하게 샘플링하여 얻은 subnetwork를 학습합니다. 네트워크가 더 sparse 해질수록(듬성해질수록) 학습이 느려지고 테스트 정확도가 낮아질 수 있습니다. 본 논문에서는 처음부터 학습할 때 적어도 더 빠르고 비슷한 테스트 정확도에 도달하는 subnetwork가 존재한다 라는 것을 보여줍니다.



<h4>1. The Lottery Ticket Hypothesis</h4>
<em>The Lottery Ticket Hypothesis</em>는 dense, randomly-initialized, feed-forward 네트워크는 비슷한 iteration 수 일 때 테스트 성능이 원래의 네트워크와 비슷한 성능의 subnetwork (winning ticket)을 가지고 있다라는 것입니다. 



일반적인 pruning은 fully-connected & convolutional feed-forward network로 부터 학습가능한 subnetwork를 포함하지 않습니다. 저자는 학습가능한 subnetwork를 학습이 가능한 weight와 connection의 조합의 initialization 복권을 당첨된 것을 찾았다고 해서, winning ticket이라 말합니다.



<h4>2. Identifying winning tickets</h4>
1. Winning ticket을 찾는 과정입니다. 뉴럴 네트워크를 랜덤하게 초기화 합니다.

2. j번째 iteration까지 학습하게 되면, weight와 같은 파라미터도 특정한 값에 도착할 것 입니다.

3. 얻은 파라미터에 대해서 p% pruning 합니다. 마스크 m도 생성합니다.

4. pruning을 하고 남은 파라미터에 대해서, 학습하기 전에 처음 랜덤하게 초기화 했던 것과 동일한 파라미터로 다시 리셋합니다. 생성한 마스크 m과 리셋하여 얻은 파라미터로 winning ticket을 생성합니다. (파라미터를 리셋하는 이유는 다시 랜덤하게 subnetwork를 초기화하게 되면 더 이상 원래의 네트워크와 비슷한 성능을 내도록 할 수 없기 때문입니다. 이론은 랜덤하게 초기화된 네트워크의 winning ticket에 해당하는 subnetwork가 적어도 하나 존재한다는 것 입니다. 다시 초기화하면 이 가정을 쓸 수가 없습니다.)



![1](https://user-images.githubusercontent.com/37301677/79829353-cbe4c500-83dd-11ea-8219-46b858841ecd.png)

또한 본 논문에서는 한 번에 pruning을 진행하는 one-shot pruning 방법을 사용하는 것이 아니라, n번의 라운드마다 train, prune, reset을 반복하는 iterative pruning 방법을 사용합니다. 위의 알고리즘은 one-shot pruning 방법처럼 보여지는데, 위의 방법을 그대로 사용하고 이것을 n 단계로 나누어 pruning을 진행하면 iterative pruning이 됩니다.





<h4>3. Results</h4>


Winning ticket은 원래의 네트워크와 거의 비슷한 시간 동안(commensurate training time), 원래의 네트워크보다 10~20% 작은 사이즈를 갖고(smaller size), 원래 네트워크의 테스트 정확도와 비슷하거나 이에 넘는 성능을 보입니다.(commensurate accuracy)





<h4>4. The Lottery ticket conjecture</h4>


위의 실험 결과를 바탕으로 새로운 가정을 생각해볼 수 있습니다. Dense, randomly-initialized 네트워크가 pruning 결과로 나온 sparse한 네트워크보다 학습하기 쉬운 이유는 학습할 때 winning ticket을 포함하는 subnetwork들이 있을 가능성이 더 많기 때문이라고 얘기할 수 있습니다.



<h4>5. Contributions</h4>
1. pruning이 원래의 네트워크와 비슷한 수의 iteration을 가질 때 테스트 정확도가 비슷하게 도달하는 학습가능한 trainable subnetwork를 포함하지 않는다는 것을 증명합니다.
2. Winning ticket을 찾는 pruning이 원래의 네트워크보다 빠르고 더 높은 정확도에 도달하고 generalizing도 더 잘 한다고 합니다.
3. 위의 발견들에 대해서 뉴럴 네트워크에 대한 새로운 관점의 구성에 대한 이론인 <em>The Lottery Ticket Hypothesis</em>를 제안합니다.



<br>







