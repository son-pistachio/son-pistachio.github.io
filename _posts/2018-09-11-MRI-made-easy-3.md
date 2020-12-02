---
layout: post
title: "MRI 쉽게 이해하기 (3) 신호 측정 및 이미지 재구성"
date: 2018-09-11
categories: Neuroscience
tags: [MRI, Neuroscience, Medical Imaging, Neuroimaging, MRI made easy, Introduction]
---



이번 글에서는 환자가 방출하는 신호 중 무슨 신호를 어떻게 찍어야 우리의 내부 조직을 볼 수 있는지, 또 어떻게 그 신호로 이미지를 만들 수 있는지에 대해 알아본다.



# MRI

MRI의 작동 순서를 정말 간단하게 정리하자면 다음과 같다.

> 1. 환자를 자석 속에 넣는다(The patient is placed in a magnet)
> 2. 전자기파를 쏜다(A radio wave is sent in)
> 3. 전자기파를 끈다(The radio wave is turned off)
> 4. 환자가 신호를 방출하고, 이것을 측정하여 쓴다(The patient emits a signal, which is received and used for)
> 5. 이미지를 재구성한다(Reconstruction of the picture)







# 무슨 신호를 어떻게 측정하는 것인가?

앞의 글에서는 전자기파를 쐈다가 꺼보면 어떤 현상이 나타나는지 확인할 수 있었다. 그러면 T1, T2 현상이 발생한다는 것은 알겠다. 그렇다면 무슨 신호를 어떻게 찍어야 우리의 내부 조직을 볼 수 있을까?



## 90° pulse와 FID signal

전자기파를 쏠 때, proton들이 에너지를 흡수하면 일부가 거꾸로 정렬된다고 했다. 그 결과로 longitudinal magnetization이 감소한다. 그렇다면 전자기파 중에 longitudinal magnetization을 0으로 만드는 세기의 파동도 있을 것이다. 그리고 이 때 같은 크기의 transversal magnetization이 생겨난다. 이런 현상을 만드는 세기의 전자기파를 90° pulse라 한다. 마찬가지로 180° pulse도 가능하다. 이것은 조금 뒤에서 다루기로 한다. 



![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_010.png?raw=true)



그럼 90° pulse를 쏘고 난 후의 magnetization 변화를 자세히 살펴보자. 90° pulse 직후에는 longitudinal magnetization은 0이 되고 transversal magnetization이 생겨난다. 그러나 그 후 longitudinal magnetization은 점차 다시 회복되고, transversal magnetization은 다시 감소한다. 한편 transversal magnetization은 회전한다. 만약 그 둘을 함께 벡터로 표현해보면, 나선형 모양을 관찰할 수 있다.



![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_011.png?raw=true)



다시 과학 지식을 복습해보자면, '움직이는 전자기력은 전류를 유도한다'. 즉 나선형으로 움직이는 전자기력이 전류를 유도하여 MRI 기기에서 그 신호를 측정할 수 있는 것이다. 이 신호는 FID(Free Induction Decay)라고 하며, 90° pulse 직후에 가장 세고 이후 점차 작아진다.이제는 'longitudinal/transversal magnetization'이 아닌 'T1, T2 축에서의 signal/signal intensity'라는 용어를 대신 쓸 수 있다는 것을 기억하자.



![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_012.png?raw=true)



## Pulse sequence

무슨 신호를 받는지까지는 알겠다. 그런데 MRI 이미지를 생각해보면, 조직이 다르면 다르게 찍힌다. 찍는 방법에 따라, 어떤 조직은 잘 나오고 어떤 조직은 잘 안 나온다고 한다. 그렇다면 어떻게 찍어야 하는 걸까?



### TR과 T1-weighted image

A와 B 조직이 있다고 하자. A가 B에 비해 짧은 T1, T2를 가지고 있다.(복습해보면 B가 더 수분을 많이 가지고 있다고 생각해 볼 수도 있다) 그럼 먼저 90° pulse를 쏴 보고, 일정 시간 뒤에 두번째 90° pulse를 쏴 본다. 무슨 일이 일어날까? 두 90° pulse 사이의 시간 간격이 충분히 길면, A B 조직 모두 longitudinal magnetization을 회복한다. 따라서 두번째 90° pulse 이후 두 조직의 transversal magnetization 크기는 모두 같을 것이다.



![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_013.png?raw=true)



그렇다면 두 90° pulse 사이의 시간 간격을 짧게 하면 어떤 일이 일어날까? A는 B보다 더 빨리 longitudinal magnetization을 회복한다. 그래서 둘 다 미처 회복하지 못한 시점에 두번째 90° pulse를 쏘면 A와 B의 transversial magnetization 크기가 달라진다. B가 더 작겠지.



![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_014.png?raw=true)



이런 식으로 두 90° pulse 사이의 시간을 다르게 하면 다른 조직 간에 차이를 주는 이미지를 찍을 수 있다. 이 때 두 90° pulse 사이의 시간을 **TR(time to repeat)**이라고 한다. 그리고 TR을 짧게 했을 때 찍히는 이미지는 T1에 의한 결과이므로 **T1-weighted image**라고 한다. 

보통 TR이 500ms보다 짧으면 짧은 편이고, 1500ms보다 길면 긴 편으로 여겨진다.



### Proton density weighted image

TR을 짧게 하면 두 조직 간의 차이를 더 잘 볼 수 있는, 그 중에서도 T1이 짧은 조직을 더 잘 볼 수 있는 사진을 찍을 수 있다고 했다. 그런데 TR을 매우 길게 해서 T1이 더 이상 조직에 영향을 주지 않는 상황이어도 두 조직의 signal intensity에는 차이가 있다. 바로 proton density 때문이다. 그래서 TR을 길게 해서 찍는 사진은 **proton density weighted image**라고 한다.



![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_015.png?raw=true)



### T2-weighted image

T1을 활용해서 T1이 짧은 조직을 더 잘 볼 수 있게 하는 이미지를 찍을 수 있다면, T2를 활용해서 T2가 긴 조직을 볼 수 있지 않을까?(왜냐하면 T1은 시간이 갈수록 신호가 증가하는 반면 T2는 시간이 갈수록 신호가 감소하기 때문에, T2가 길수록 측정되는 신호는 크다)

T2 현상은 proton들이 함께 돌다가 그 일관성을 잃게 되면서 transversal magnetization이 감소하는 현상이다. 그러면 어떻게 이 proton들이 다시 함께 모이도록 할 수 있을까? 180° pulse를 활용하여 그렇게 할 수 있다. 

180° pulse는 제각기 돌아가는 proton들을 '반대방향으로 돌려' 다시 모이게 한다. 그리고 이것은 다시 transversal magnetization을 회복시켜 신호를 증가시킨다.
![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_017.png?raw=true)
![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_016.png?raw=true)

이렇게 proton들이 180° pulse에 의해 다시 모이는 것을 **spin echo**라 하고, 첫 90° pulse에 의해 transversal magnetization이 생긴 시점부터 180° pulse에 의해 spin echo가 생기는 시점까지의 시간을 **time to echo**, 줄여서 **TE**라 한다. 그러니까 180° pulse를 주는 시점은 TE/2라고 보면 된다.

그리고 spin echo가 생기더라도 곧 다시 proton들은 흩어진다. 그래서 다시 180° pulse를 주고, 또 180° pulse를 주고.. 그렇게 spin echo의 신호를 이은 것은 다음과 같다.

![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_018.png?raw=true)

한편 180° pulse가 상쇄시켜 주는 것은 외부 자기장의 일반적인 효과이고, 내부 proton들이 각각 가지고 있는 자기장의 효과는 상쇄시켜 줄 수가 없다. 따라서 일부 proton들은 spin echo 때 함께 도는 것이 아니라 그 근처에서 줄을 서서 돌게 되는데, 이것이 신호를 계속 감소시킨다. 이 효과를 **T2-effect**라 한다. 그리고 만약 180° pulse를 안 쓰면 상쇄시켜주는 효과가 없어서 매우 빠르게 신호가 감소하는데, 이것을 **T\*2-effect**라 한다. 이것은 이후 설명할 fast imagning sequences에 쓰인다.

T2-weighted image의 특성을 보자면, 짧은 TE를 쓸 경우 조직들 간의 차이를 보기 힘들다는 것이다. 긴 TE를 쓸 수록 그 차이를 보기 쉽다. 그렇지만 그렇다고 너무 긴 TE를 쓰면 애초에 신호 자체가 너무 작아져서 노이즈와 신호를 분간하기 힘들어지고, 좋은 이미지를 생성하기 어렵다.

![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_019.png?raw=true)



## 정리: Spin echo pulse sequence

지금까지 말한 내용을 정리해서 MRI에서 전자기파를 쏘고 그 신호를 받는 과정을 요약하자면 다음과 같다.

> 90° pulse -> wait TE/2 -> 180° pulse -> wait TE/2 -> record signal

![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_020.png?raw=true)



## TR과 TE의 길이에 따라 달라지는 이미지

![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_021.png?raw=true)



위 그림처럼, TR을 길게 하고 TE를 짧게 하면 무슨 이미지를 얻을 수 있을까?

TR을 길게 했기 때문에 각 조직들은 longitudinal magnetization을 모두 회복했을 것이다. 한편 TE는 짧기 때문에 아직 T2로 인한 조직 간의 신호 차이는 너무 작다. 따라서 이 이미지는 T1-/T2-weighted가 모두 아닌 오직 proton density에 의한 신호 차이만이 존재하는 이미지다. 즉 proton density-weighted image다.



![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_022.png?raw=true)



위 사진처럼 TR과 TE 모두 길게 했을 때는 어떤 사진이 나올까?

T1로 인한 신호 차이는 거의 없는 반면 T2로 인한 신호 차이는 크기 떄문에, T2-weighted image이다.

![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_023.png?raw=true)

위 사진처럼 TR과 TE 모두 짧게 하면 어떤 사진이 나올까?

T1으로 인한 신호 차이가 크고 T2로 인한 신호 차이는 크지 않아서, T1-weighted image라고 보면 된다.

마지막으로 TR은 매우 짧게, TE는 매우 길게 하면 어떨까? 이 경우 측정되는 신호 자체가 너무 작아서 의미 있는 이미지를 만들 수 없다.

이렇게 proton density/T1/T2 weighted image를 실제로 찍으면 다음처럼 나타난다.



![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_024.png?raw=true)

![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_025.png?raw=true)



T1-weighted image에서는 CSF(물이 많아서 T1, T2가 길다)가 검은색, 그리고 gray matter가 white matter보다 어둡게 나타난다.

Proton density-weigthed image에서는 CSF가 T1보다는 밝은 검은색이고, white matter가 gray matter보다 어둡게 나타난다.

T2-weighted image에서는 CSF가 가장 밝은색이다.

다음 그림을 보면 생각하기 편하다.

![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_026.png?raw=true)



# Fast imaging sequences(Short TR)

일반적인 MRI imaging sequences는 시간이 좀 걸린다. 뇌의 경우 20~45분, 흉부의 경우 25~45분까지. 따라서 제한된 수의 환자만 촬영이 가능하고, 환자들도 그 긴 시간 동안 조금씩 움직이기 때문에 문제가 발생한다. 따라서 이러한 문제를 해결하기 위해 fast imaging sequences가 필요하다. 

Fast imaging sequences는 FLASH(Fast Low Angle Shot) 혹은 GRASS(Gradient Recalled Acquisition at Steady State)를 통해서 이루어진다.

MRI imaging sequences에서 TR은 가장 시간을 많이 잡아먹는 존재다. 그래서 이미징을 빨리 하려면 이것을 줄여야 한다. 하지만 이것을 줄일 땐 그만큼 문제점이 생긴다.

- spin echo seqence에서 proton을 반대 방향으로 돌리기 위해 원래는 180° pulse를 썼는데, 매우 짧은 TR 을 쓰려면 180° pulse를 못 쓴다. 180° pulse를 쓰려면 좀 시간이 필요한데 매우 짧은 TR을 쓸 때는 90° pulse 사이 시간 간격이 그만큼 충분하지 않기 때문이다.
- TR이 감소하면 그만큼 longitudinal magnetization도 회복을 많이 하지 못하는데, 따라서 신호가 작아진다.

이 문제들은 다음과 같이 해결될 수 있다:

- 180° pulse 대신 magnetic field gradient를 활용해 proton들을 반대로 돌린다. 이 말은 기존 자기장 위에 불균형한(예를 들면 경사가 있는) 자기장을 얹는다는 것이다.

  경사 자기장을 짧은 시간 동안 켜준다. 그러면 이것이 기존의 뷸균형한 자기장(외부 자기장의 불균형과 내부 proton들의 자기장으로 인해 발생)에 더해져 자기장의 불균형 정도가 커진다.

  Transversal magnetization이 사라지는 큰 이유는 바로 자기장의 불균형 때문인데, 더 불균형하게 만들었으니 더 빠르게 사라진다. 이 때 다시 경사 자기장을 꺼주었다가, 잠시 뒤에 다른 방향으로 다시 켜 주면, 빠른 proton들은 느려지고 느린 proton들은 빠르게 움직인다. 그러면 spin echo와 비슷한 echo가 생기는데, 이것은 **gradient echo**라 부른다.

- 신호가 작아지는 문제는 작은 flip angle(10°~35°)을 통해 해결할 수 있다.

  90° pulse는 longitudinal magnetization을 완전히 없애기 때문에 회복되는 데 시간이 많이 걸린다. 하지만 90° pulse가 아니라 10°~35° 정도의 작은 flip angle을 쓰면 longitudinal magnetization이 남아 있어 짧은 시간 뒤에 다음 pulse를 쓸 수 있다. 그리고 이것은 다음에 매우 짧은 TR로 pulse가 와도 꽤 쓸만한 신호를 준다.

  ![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_027.png?raw=true)

이것이 바로 fast imaging sequences이다. 다른 말로 gradient echo sequences라고 부르기도 한다. 이 gradient echo imaging의 특징을 더 얘기하자면 다음과 같다.

- 180° pulse를 쓰지 않기 때문에 위에서 얘기했던, Transversal magnetization이 급속하게 감소하는 T2*-effect가 나타난다.
- flip angle이 클수록 T1-weighting 된다.
- TE가 길수록 T2*-weighting 된다.
- 빠른 이미징에서는 대개 혈관에서 오는 신호가 강하게 관찰된다.

우리가 gradient echo imaging에서 시간을 줄일 수 있는 이유는 다음과 같다. 이러한 이유로 1초, 혹은 그 미만으로도 이미징을 할 수 있게 된다.

- 작은 flip angle을 활용해 전자기파를 짧은 시간 간격으로 쏠 수 있다.
- 작은 flip angle을 쓰기 때문에 언제나 충분한 longitudinal magnetization이 존재한다. 따라서 이것의 회복을 위한 긴 TR을 쓰지 않아도 된다.
- 쏘는 데 시간이 좀 걸리는 180° pulse를 쓰지 않는다.

## Fast imaging time

Fast imaging sequences보다 짧게 이미징 시간을 줄일 수 있는 방법은 없을까? 대체 무엇이 이미징 시간을 결정하는 걸까? 일반적인 이미징 시간은 다음과 같이 구할 수 있다.
$$
acquisition\ time = TR * N * N_{ex}
$$
N은 찍는 사진의 수이고, N_ex는 하나의 사진을 완성시킬 때까지 필요한 여러 번의 촬영 횟수이다. 사실 fast imaging에서 나오는 신호는 다소 작다. 따라서 좋은 퀄리티의 사진을 위해 여러 번 사진을 찍고 그것의 평균을 구하는 것이 좋다. 결국, 우리가 원하는 것은 신호 대비 노이즈가 적은 사진이다.

## Multi-slice imaging(Long TR)

만약 추가적인 신호 측정을 위해 긴 TR을 사용한다면 그만큼 이미징 시간도 증가한다. 그러나, 긴 TR을 사용하면서도 이미징 시간을 줄일 수 있는 방법이 존재한다.

첫 번째 방법은 조영제(contrast medium)을 사용하는 것이다. Gandolinium을 인체 내부에 주입하면 T1이 감소하고, TR을 줄일 수 있어 신호의 감소 없이 빠른 시간 내에 이미징이 가능하다.

두 번째 방법은 multi-slice imaging이다. 한 slice에서 imaging sequence를 반복할 때까지 기다리는 것이 아닌, 몸을 여러 개의 slice로 나눠서 TR이 끝날 때까지 다른 slice에서 이미징을 하는 것이다.

신체를 어떻게 자르지 않고(...) slice할 수 있을까? 여기서 말하는 slice는 가상의 구역을 나눈 것으로 이해하자. <del>우리는 이미징을 하고 싶은거지 부검을 하고 싶은게 아니다</del> 우리가 환자를 MRI 기기 내에 넣으면 비교적 균일한 전자기장 속에 있게 된다. 그러면 Larmor equation에 따라 모든 proton들이 같은 frequency를 가지게 된다. 그리고 같은 파장의 전자기파에 반응하게 된다. 