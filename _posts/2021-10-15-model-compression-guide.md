---
layout: post
title: 딥러닝 경량화 튜토리얼 - Deep learning model compression guide
subtitle: Deep Learning model compression Tutorial - Deep learning model compression guide
category: [model_compression]
tags: [model_compression]
comments: true
---





> [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fysbsb.github.io%2Fmodel_compression%2F2021%2F10%2F15%2Fmodel-compression-guide.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
>
> 이번 포스팅에서는 딥러닝 경량화의 개념, 딥러닝 경량화의 종류를 이야기 하려고 합니다. 여러 딥러닝 경량화 방법들의 종류를 선정하였고 이 방법들에 대해서 간단히 설명을 듣고 활용하실 수 있도록 준비해 보았습니다.

<br>



# 딥러닝 경량화란?

딥뉴럴 네트워크에서 방대한 연산량을 줄이기 위한 연구입니다.

경량화를 하는 이유는 real-time에서 작동하지 않기 때문에 latency를 빠르게 하기 위해서, 메모리를 많이 차지 하기 때문에 memory를 줄여서 특정 환경에서 실행하기 위해, 하드웨어 디바이스 중에는 8비트만 지원하기 때문에 평소에 사용하던 32비트를 8비트로 변환하기 위해서 등이 있습니다.

GPU 뿐만 아니라 모바일, 엣지 디바이스, 클라우드 등에 딥러닝 모델이 쓰이기 위해서 개발되어 지고 있습니다.





<br>

ETRI의 경량 딥러닝 기술 동향을 참고하여 작성하였습니다.

https://ettrends.etri.re.kr/ettrends/176/0905176005/34-2_40-50.pdf





# 1. 경량 알고리즘 (기존 딥러닝 모델의 발전)

<h2>1) 모델 구조 변경 기술</h2>

ResNet

- 기존의 깊은 신경망은 단순히 깊게 쌓을 수록 성능이 낮아졌습니다. ResNet은 지름길 (short connection)을 만든 residual connection 구조를 새롭게 제안하여, 신경망을 깊게 쌓을 수도 있고 정확도 개선 효과를 보였습니다. 

DenseNet

- 기존 feature map을 더해주는 것이 아닌, concatation을 통한 쌓아주는 과정을 통해 모델의 성능을 높였습니다. 현재 층에서 이전의 모든 층의 정보들을 얻을 수 있는 방법입니다.

SqueezeNet

- 3x3 컨볼루션을 1x1 컨볼루션으로 대체하면서, 채널 수를 줄였다가 다시 늘리는 방법으로 파라미터 수를 줄이면서 성능을 내는 방법입니다.

<h2>2) 효울적인 합성곱 필터 기술</h2>

MobileNet

- 기존의 컨볼루션에서, 먼저 채널 단위로 합성하는 Depthwise convolution을 사용하고, 그 결과를 하나의 픽셀에 대하여 진행하는 Pointwise convolution으로 나누었습니다. 이 방법으로 성능을 유지하면서 효율적인 연산 방법을 제안하였습니다.

ShuffleNet

- Pointwise convolution을 수행할 때 특정 채널 연산에 대해서만 수행을 해서 더 효율적으로 진행하는 방식입니다. 예를 들어 64x64 채널이라면 이를 4 group으로 나누어서 계산할 수 있습니다.

AdderNet

- 화웨이에서 진행한 연구로 컨볼루션에서 곱셈연산을 덧셈연산으로 바꾼 새로운 구조를 제안하였습니다.

# 2. 알고리즘 경량화 (딥러닝 모델 압축 방법의 발전)



<h2>Quantization</h2>

Quantization은 가중치 양자화라고 불리고 일반적인 딥러닝 프레임워크는 모두 float 32bit 타입을 사용하는데, 이러한 변수를 int 8bit 타입으로 바꿔주는 방법입니다. Binarization 가중치 이진화는 weight 값을 -1과 +1 두 개의 값만 사용하도록 하는 방법입니다. 

- 양자화 역할
  
- 네트워크 가중치의 float 32 변수를 int 8 변수로 변환
  
- 양자화 장점

  - 예를 들어 32bit가 8비트로 뀌면 20MB인 모델을 1/4배인 5MB로 줄일 수 있다. 리소스가 제한된 디바이스에 넣을 수 있다.
  - 연산이 32비트에서 8비트로 바뀌었기 때문에 연산 자체의 속도가 줄어들어서 latency가 줄어든다.
  - 8비트만 지원하는 하드웨어에 넣을 수 있다.

- 분류

  - Quantization Aware Training (QAT)

    - 학습 시에 양자화를 하는 방법. 학습 시에 실제로 int 8이 되는 것이 아니라, Quantization 수식을 사용해서 float 32 변수를 임의로 int 8 변수의 값이 되도록 맵핑해서 학습함. 학습 할 때 일부러 int 8과 같은 효과를 적용하면 이 환경에 맞추어서 학습이 되기 때문에 성능 방지 효과를 더 누릴 수 있음

    ![quantization](https://user-images.githubusercontent.com/37301677/135977443-06872500-2fad-41c7-917a-d95ca3307a7d.png)

  - Post Training Quantization (PTQ)

    - Pretrained 모델을 바로 양자화 하는 방법. QAT 보다 성능이 떨어지만 PTQ를 하면서도 성능 방지를 위한 방법이 연구되고 있음



<h2>Pruning</h2>

Network Pruning은 가중치를 가지치기 하는 방법으로, 딥러닝 네트워크에서 연산에서 불필요한 부분을 가치기하는 방법입니다. 딥러닝에서 dropout의 효과와도 비슷합니다. 사람의 뇌와 비유를 하면, 사람의 뇌도 일부만 사용하고 나머지는 사용하지 않는 부분이 많다고 하는데 딥러닝 모델에서는 실제 사용시 나머지 부분이 불필요하고 다 없애도 되기 때문에 최대한 딥러닝 역할을 잘 하는 weight, filter, layer 등만 남기고 없애는 방법입니다. 가장 간단한 방법으로는 weight에서 threshold를 만들어서 이보다 작은 것은 0으로 남기고 나머지는 남기는 방법이 있습니다.

![pruning](https://user-images.githubusercontent.com/37301677/135974881-1d5ce9bc-d027-4ad8-8f62-d8344a7c7c38.png)

- Pruning 역할
  - 네트워크 가지치기를 통한 파라미터 줄이기
- Pruning 장점
  - 파라미터를 줄이면 메모리가 줄어든다. 메모리를 줄이면 리소스가 제한된 디바이스에 넣을 수 있다.
  - 네트워크 자체가 줄어들었기 때문에 latency가 줄어든다.
- 분류
  - Structured Pruning
    - 프루닝 시 개별적인 weight들이 0으로 되어 남아 있어 네트워크 구조가 바뀌지 않는다.
  - Unstructed Pruning
    - 프루닝 시 channel이나 filter가 제거되어 네트워크 구조가 바뀐다.





<h2>Distillation</h2>

성능이 더 좋은 Teacher 모델로 부터 학습의 기준이 되는 Student 모델에게 지식을 증류 (knowledge distillation) 하는 방법입니다.

![distillation](https://user-images.githubusercontent.com/37301677/135977566-f6fa1171-bca3-4f4c-a5ba-f2fcbab73b18.jpeg)

- Distillation 역할
  - 성능 향상
    - Baseline에서 더 좋은 성능을 가진 모델로부터 지식을 증류하는 방법입니다. 이렇게 하면 baseline 보다 성능 향상을 이루를 효과를 가져올 수 있습니다.
  - 모델 압축
    - 작은 toy network를 만들어서 원래 모델의 성능을 증류시킬 수도 있습니다. 네트워크는 더 작지만 원래 모델의 성능이 나오는 효과가 있습니다.
- 예시
  - KD: 두 개의 네트워크 간의 softmax로 나오는 부분을 비슷하게 하기
  - AT: 두 개의 네트워크 간의 feature의 attention map을 비슷하게 하기
  - SP: 두 개의 네트워크 간의 similarity matrix를 비슷하게 하기

<h2>Neural Architecture Search</h2>

Neural Architecutre Search (NAS)는 강화학습 또는 optimization 기반으로 네트워크를 자동으로 찾는 방법입니다.

<br>



여기까지 딥러닝 경량화의 개념과 종류들에 대한 설명을 해보았습니다. 질문이나 지적, 요청해주실 부분이 있다면  댓글이나 메일 부탁드립니다.

읽어주셔서 감사합니다. 😃