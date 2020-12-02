---
layout: post
title: "MRI 쉽게 이해하기 (2) 전자기파를 쏘고 끄기"
date: 2018-09-10
categories: Neuroscience
tags: [MRI, Neuroscience, Medical Imaging, Neuroimaging, MRI made easy, Introduction]
---



이번 글에서는 환자를 자석 속에 넣은 상태에서 **전자기파를 쏠 때와 끌 때** 발생하는 현상에 대해 알아본다.



# MRI

MRI의 작동 순서를 정말 간단하게 정리하자면 다음과 같다.

> 1. 환자를 자석 속에 넣는다(The patient is placed in a magnet)
> 2. 전자기파를 쏜다(A radio wave is sent in)
> 3. 전자기파를 끈다(The radio wave is turned off)
> 4. 환자가 신호를 방출하고, 이것을 측정하여 쓴다(The patient emits a signal, which is received and used for)
> 5. 이미지를 재구성한다(Reconstruction of the picture)





# 전자기파를 쏴 보자

MRI의 두번째 단계인 '전자기파를 쏜다'에 대해 한 번 얘기해보도록 하자. 

proton의 precession 주기를 알기 위해서는 Larmor equation을 쓴다고 했다. 이 precession 주기와 같은 파장으로 전자기파를 쏘게 되면, proton들은 에너지를 흡수하게 된다. 이를 공명(resonance) 현상이라 한다. 위에서 에너지가 높은 proton들은 어떻게 된다고 했더라.. 맞다. 물구나무를 선다. 에너지를 받은 proton들은 외부 자기장과 반대 방향으로 정렬되고, 이는 longitudinal magnetization와 상쇄되어 그 크기를 감소시킨다. 

![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_004.png?raw=true)



그런데, 전자기파를 받으면 하나 더 신기한 일이 발생한다. 

위의 사진을 보면, proton들의 precession은 한 시점에 다른 방향을 가리키는 식으로 있기 때문에 x/y축으로의 magnetic force는 서로 상쇄되어 잘 나타나지 않는다. 그런데 전자기파를 쏴주면, 놀랍게도 한 쪽으로 정렬된다!

![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_005.png?raw=true)

그러면 외부 자기장에 수직 방향으로 magetic force가 형성되는데, 이것을 **Transversal magnetization**이라고 한다. 



> 전자기파를 MRI 기기 내에 누워 있는 환자에게 쏘면, 
>
> - proton들이 에너지를 흡수해 더 많은 proton들이 외부 자기장과 반대 방향으로 정렬된다. 이것은 Longitudinal magnetization을 감소시킨다.
> - proton들이 다같이 모여서 돌게 되면서 외부 자기장의 방향과 수직으로 magnetic force를 형성한다. 이것을 Transversal magnetization이라고 한다.



# 전자기파를 꺼 보자

자, MRI에 사람을 집어넣고 전자기파도 쏴 봤으니, 이제 세 번째 차례인 '전자기파를 끈다'를 해 볼 시간이다. 

전자기파를 끄면, 전자기파로 인해 바뀌었던 상태가 다시 그 전 상태로 회복된다. proton들은 에너지를 잃어 다시 외부 자기장과 나란하게 정렬되고, Longitudinal magnetization은 다시 증가한다. 마찬가지로 proton들의 precession이 다시 제각기로 돌아가면서 Transversal magnetization은 감소한다. 



## T1

Longitudinal magnetization이 다시 증가하는 현상은 longitudinal relaxation 또는 spin-lattice-relaxation이라고 한다. 흡수했던 에너지를 열에너지의 형태로 lattice라 불리는, 주변의 환경에 넘겨주기 때문이다. 또 이 현상은 줄여서 T1이라고도 한다. 아래는 이 현상을 그래프로 나타낸 T1-curve이다.

![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_006.png?raw=true)



## T2

Transversal magnetization이 감소하는 현상은 transversal relaxation 또는 spin-spin-relaxation이라고 한다. 줄여서는 T2라고 한다. T2 현상을 그래프로 나타낸 T2-curve는 다음과 같다.

![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_007.png?raw=true)



## T1-T2 비교

T2 현상에 대해 조금만 더 자세히 알아보자. 사실 proton들의 precession 주기는 굉장히 빠르다. 예를 들어, p1이 10Mhz고 p2가 그보다 1% 빠른 10.1Mhz라고 해보자. p2는 5ms만에 중심축을 50.5번 돈다. p2는 50번 도는데 말이다. 이렇게 조금의 주기 차이가 순식간에 각 proton을 반대편으로 보내버린다. 따라서 proton들이 precession에 따라 다시 제각기로 돌아가는 시간은 짧은 편이고, transversal magnetization도 그에 따라 빠르게 감소한다. 

즉 T2는 T1에 비해 상당히 짧다. 생체 조직에서 T1은 300~2000ms인데 반해 T2는 30~150ms로 보통 2~10배까지 차이가 난다. 아래는 T1과 T2를 합친 것.

![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_008.png?raw=true)

사실 T1과 T2가 끝나는 시점을 정하기가 참으로 애매하다. 그래서 기준을 정했다. T1은 기존 longitudinal magnetization의 63%(1-1/e)까지 회복되는 때까지, T2는 기존 transversal magnetization의 37%(1/e)까지 감소되는 때까지이다.

그리고 물을 가지고 있는 조직(watery tissues)이 지방 조직(fatty tissues)보다 긴 T1, T2를 가진다. 그 이유는 바로 가지고 있는 에너지를 열에너지로 방출하게 되는데 watery tissue의 경우 그 전도가 느리기 때문이다. 감염된 조직의 경우 수분의 비중이 높아서 주변의 일반 조직보다 T1, T2가 길다.

![](https://github.com/karl6885/karl6885.github.io/blob/master/assets/images/posts/MRI_made_easy/MRI_made_easy_009.png?raw=true)

또 외부 자기장이 강할수록 precession이 빨라서 proton이 주변 lattice로 에너지를 내려놓기가 어렵다. 즉 외부 자기장이 강할수록 T1이 길다.

불순물이 많은 액체의 경우에는 proton들의 precession 주기들이 많이 달라 함께 돌던 상태에서 흩어지기가 쉽다고 한다. 즉 내부 조직이 불균일할수록 T2가 빨라진다.



지금까지 전자기파를 쐈다가 꺼봤다가 해 봤다. 그러면 T1, T2 현상이 발생한다는 것은 알겠다. 그렇다면 무슨 신호를 어떻게 찍어야 우리의 내부 조직을 볼 수 있을까? 어떻게 그 신호로 이미지를 만들 수 있을까? 다음 글에서는 이 내용에 대해 다뤄본다.