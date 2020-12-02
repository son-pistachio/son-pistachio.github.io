---
layout: post
title: "MRI 쉽게 이해하기 (1) 개요"
date: 2018-09-09
categories: Neuroscience
tags: [MRI, Neuroscience, Medical Imaging, Neuroimaging, MRI made easy, Introduction]
---



의료영상 중 가장 대표적인 MRI에 대해 쉽게 이해해보는 글. 

고등학교 과정에서 물리를 배운 적이 없던 문과도 이해할 수 있도록 쉽게 풀어 써 보았다.

MRI 영상들 중에서도 뇌를 촬영한 Neuroimaging에 초점이 맞춰져 있다. 

MRI made easy라는 원서를 정리하였으며, 필요에 따라 내용을 가감하였다.

이 내용은 고려대학교 뇌인지융합전공 `뇌및의공학입문` 수업에서 다룬 내용이기도 하다.



# MRI

MRI의 작동 순서를 정말 간단하게 정리하자면 다음과 같다.

> 1. 환자를 자석 속에 넣는다(The patient is placed in a magnet)
> 2. 전자기파를 쏜다(A radio wave is sent in)
> 3. 전자기파를 끈다(The radio wave is turned off)
> 4. 환자가 신호를 방출하고, 이것을 측정하여 쓴다(The patient emits a signal, which is received and used for)
> 5. 이미지를 재구성한다(Reconstruction of the picture)



이번 글에서는 MRI의 기초 개념과 함께 **1. 환자를 자석 속에 넣는다** 부분에 대해 알아보겠다.



## MRI 기초 - Magnetic field & Protons

MRI는 무슨 약자일까? 무슨 뜻일까?

**MRI**는 (Nuclear) **M**agnetic **R**esonance **I**maging의 약자다. 번역해보자면 핵자기공명영상..쯤 되시겠다. 사실 NMR(Nuclear Magnetic Resonance, 핵자기공명)이라고 해서 **'자기장 속에 놓인 원자핵이 특정 주파수의 전자기파와 공명하는 현상'**이라는 게 존재한다. 이것이 MRI의 기초 원리다. 근데 뭔가 Nuclear 하면 핵폭탄 같고(...) 부정적인 느낌이 들어서 여기서 Nuclear를 빼고 만든 용어가 Magnetic Resonance Imaging이다. 자기공명영상이라고 해서 그걸로만 기억하시지 마시고, 자기장으로 '무엇을' 공명하는 건데? 라는 질문도 좀 하고 그랬으면 한다.

그래서, 결국 MRI의 원리를 아는 것은 **자기장** 속에서 **원자핵**이 **특정 주파수의 전자기파**에 의해 공명하는 원리를 알고, 이것을 어떻게 다시 **이미지화**할 수 있는지를 아는 것이다. 중요하니까 다시 한 번 강조해보자.



> MRI의 원리를 아는 것은 **자기장** 속에서 **원자핵**이 **특정 주파수의 전자기파**에 의해 공명하는 원리를 알고, 이것을 어떻게 다시 **이미지화**할 수 있는지를 아는 것이다.



자, 다시 MRI를 찍는 순서로 돌아가 생각해보면, 첫번째 단계가 '**환자를 자석 속에 넣는다**'였다. 환자를 자석 속에 넣으면 무슨 일이 일어날까? 이걸 이해하기 전에, 좀 지루하겠지만 물리법칙 몇 개를 알아야 한다. 

원자가 있다. 원자는 **원자핵**과 **전자**들로 구성되어 있다. 이 원자핵 안에는 양의 전하를 가진 proton들이 있다. 이 proton들은 지구와 같은 방향으로 나란히 있고 또 같은 방향으로 돈다. 그리고 고등학교 물리 시간에 배웠겠지만, 움직이는 전하는 곧 전류이고, 전류는 전자기력, 전자기장(Magnetic field)을 유도한다.(물론 문과는 고등학교 때 배운 기억이 없겠지만ㅠ 이해가 안 되어도 그냥 끄덕끄덕하고 넘어가자. 그냥 이런게 있다-정도로만 알아도 무관하다. 매번 강조하는 부분만 제대로 알고 가면 된다. 여기서 중요한 건 proton이 magenetic field를 가진다는 것이다.)



> 각 proton들은 고유의 magenetic field를 가진다.



proton들이 각자 magnetic field를 가지고 있기 때문에, 어찌 보면 하나의 작은 자석이라고 봐도 무방하다. 그런데 사실 우리는 모두 엄청나게 큰 자기장 속에 있다. 무슨 말이냐면, 우리는 모두 '지구'라는 큰 자기장 속에 있다. 다들 지구가 하나의 거대한 자석이고 자기장을 형성하고 있다는 얘기를 한번쯤 들어봤을 것이다. 

그렇다면 지구라는 거대한 자기장 속에 있는 proton이라는 작은 자기장들은 어떻게 될까? 바로 그 지구의 자기장에 나란히 정렬된다. 또는 그것에 거꾸로***(!)*** 정렬된다. 나란히 정렬되는 proton들과 거꾸로 정렬되는 proton들은 에너지 정도에 차이가 있다. 거꾸로 정렬되는 proton들이 더 에너지가 큰데, 사실 그도 그럴 것이 자연스럽게 자기장을 따라 있는 것보다 그에 역행해서 거꾸로 있는 게 더 힘들고 에너지가 많이 필요하다. 그냥 땅 위에 서 있는 것과 물구나무를 하고 있는 것 중에 물구나무가 더 힘이 든다. 이렇게 생각해보면 좀 이해가 빠를지도.(실제로 책에서 이런 비유를 든다. 이 책이 이런 비유들을 좋아한다. 과학적으로 정확한 비유는 아니겠지만 이해하기엔 정말 직관적이고 좋은 비유들이라고 생각한다)

이것은 지구뿐만 아니라 지구보다 더 센 외부 자기장에도 해당되는 얘기다. 예를 들면 MRI 기기의 자기장에 넣으면 그 자기장에  따라 나란히 또는 그 반대 방향으로 정렬된다.

나란히 정렬되거나 반대 방향으로 정렬되는 proton 수의 차이는 작은 편이고, 적용되는 magnetic field에 따라 달라진다. 



> proton들은 외부 자기장의 방향에 나란히 또는 그 반대 방향으로 정렬된다. 그리고 그 반대 방향으로 정렬되는 proton들의 에너지가 더 높다.



### Precession

Proton들이 외부 자기장의 방향에 따라 나란히 있게 된다고 서술하긴 했지만, 사실 그냥 가만히 있는 것은 아니다. Proton들은 그 방향으로 정렬이 되어 있는 한편 마치 팽이처럼 빙빙 도는 움직임을 보인다. 이 움직임을 **Precession**이라 한다. 

![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_001.png?raw=true)

이 precession의 회전 주기는 magnetic field의 세기에 비례한다. 이것을 측정하는 **Larmor equation**이 존재한다.


$$
W = r * B
$$


W는 precession의 주기(단위는 Hz 또는 MHz), B는 외부 자기장의 세기(단위는 Tesla), r은 gyro-magentic ratio라고 해서 물질마다 다르다. proton의 경우 42.5MHz/T라고 한다. 환율과 비슷하다고 생각하면 된다.



> Proton들은 그 방향으로 정렬이 되어 있는 한편 마치 팽이처럼 빙빙 도는 움직임을 보인다. 이 움직임을 **Precession**이라 한다. 
>
> 그리고 precession의 회전 주기는 magnetic field의 세기에 비례한다. 이것은 Larmor equation로 측정할 수 있다.



## Longitudinal magnetization

앞으로의 설명을 좀 더 편하게 하기 위해, 좌표계를 쓰기로 한다.(매번 proton과 자석 하나하나를 다 그릴 수 없으니..)사실 proton은 direction과 magnitude를 모두 가지고 있으니 vector로 표현하기 더 편하다. *이제부터 proton들은 화살표로 표현한다.*

![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_002.png?raw=true)

그럼 이렇게 좌표계로 표현하게 될 때, 실제 사람의 몸에서는 어떤 식으로 각 자기력의 힘이 분포하고 있을까? 그건 다음 사진과 같다.

![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_003.png?raw=true)

왼쪽 사진을 보면 위에서 얘기했듯 proton들이 외부 자기장에 나란히 혹은 반대로 정렬된다. z축이 외부 자기장의 방향이라고 생각하면 된다. 그러면 조금 더 많은  magnetic force가 외부 자기장의 방향에 있다는 것을 알 수 있다. 그도 그럴것이 자연스럽게 외부 자기장에 편승하는 것보다 그 반대쪽 방향을 향해 있는 것이 더 에너지가 많이 필요하기 때문이다.(기억 안 나면 위에 가서 다시 보고 오자) 

그렇게 반대 방향에 있는 magnetic force를 정방향에 있는 magnetic force와 빼서 그 차이만 표현해보면, 오른쪽 그림이 나온다. 일부 magnetic force가 외부 자기장에 나란히 형성되는 것이다. 이것을 **Longitudinal magnetization**이라고 한다.



> 외부 자기장에 나란히 형성되는 magnetic force를 Longitudinal magnetization이라고 한다.



지금까지는 MRI의 첫 단계인 '환자를 자석 속에 넣는다'에서의 상황에 대해서만 얘기했다. 다음 글에서는 두번째 단계인 '전자기파를 쏜다'와 세번째 단계인 '전자기파를 끈다'에 대해 알아본다.