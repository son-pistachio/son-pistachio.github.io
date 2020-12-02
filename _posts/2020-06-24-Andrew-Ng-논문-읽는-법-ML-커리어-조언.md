---
layout: post
title: "Andrew Ng - 논문 읽는 법 / ML 커리어 조언"
date: 2020-06-24
categories: Data_science
tags: [Data science, Paper, Andrew Ng, Career]
---

Andrew Ng의 딥러닝 강의인 Stanford CS230 Lecture 8([https://youtu.be/733m6qBH-jI](https://youtu.be/733m6qBH-jI))을 요약 정리해보았다. 

# 1. 논문 읽는 법

## 한 분야의 논문들을 읽는 법

예를 들면 Speech Recognition에 대해 새로 배우기 위해 논문을 읽는다고 하자.

- 읽어야 할 논문들을 모으기(+ Medium / Blog 포스트)
- 논문 리스트를 훑어보기

    ex) 5개의 논문을 읽는다 치면 5개 모두 10%만 간단하게 읽는다. 그럼 이상한 논문도 있고(읽기 중단) 중요한 논문도 있다(이걸 많은 시간을 들여서 읽고 이해한다). 그 후 중요한 논문에 나왔던 참고 논문(6번째 논문)을 읽어보고, 5개의 논문 중 다른 논문을 읽으며 이해를 더하고, 그 다음에 7번째 논문을 찾아 읽고 ... 한다.

    그렇게 한 분야에서 15-20개 정도의 논문을 읽으면 웬만큼 이해할 수 있고 작업을 할 수 있을 정도가 된다(알고리즘을 적용하기 등)

    그리고 한 분야에서 50-100개 정도의 논문을 읽을 정도가 되면 그 분야에 깊은 지식을 알고 있다고 해도 될 정도가 된다.

## 한 개의 최신 논문을 읽는 법

Andrew Ng의 경우에는, 항상 가방에 논문들을 들고 다니며 읽는다. 이게 내 진짜 인생임. 이라고 하심. Landing AI와 deeplearning.ai에서 한 주에 2개의 논문 스터디를 하는데, 2개를 고르기 위해 한 주에 5-6개를 읽는다.

그냥 첫 단어부터 마지막 단어까지 쭉 읽는 건 좋은 방법이 아니다.

핵심 내용에 집중해서 파악하고, 그 후에 수학 공식 같은 어려운 부분을 봐라.

1. Title / Abstract / Figure를 읽기

    특히 딥러닝에서는 Figure 한 두 개로 해당 논문의 모든 내용을 요약하는 리서치 논문들이 꽤 있다. 그래서 가끔은 Title과 Abstract, Figure(뉴럴넷 구조에 대한 그림이라던지)와 Experiments 1-2개 파트만 봐도 이 논문이 어떤 건지 웬만큼 파악할 수 있는 경우가 있음

2. Intro + Conclusion + Figure + Skim rest (Skim related work)

    논문의 publish 과정은 결국 reviewer들에게 내 논문이 accept될 만큼 가치가 있다는 것을 설득하는 과정. 그래서 reviewer들이 내용을 잘 파악할 수 있도록 저자들은 Intro와 Conclusion을 아주 신중하고 깔끔하게 요약한다.

    related work 부분은 관련 연구나 해당 분야의 중요한 논문을 알고 싶다면 유용하다. 그러나 솔직히 말하자면 처음 볼 때 related work 부분은 skim하거나 아니면 skip해도 된다. 만약 처음 접하는 분야라고 하면 대부분 내용을 이해하기가 거의 불가능하고, 가끔은 related work에서 저자들이 그냥 해당 분야에서 자기 논문의 reviewer가 될 것 같은 사람들의 연구를 모조리 인용해서(싸바싸바?) reviewer들을 기분 좋게 해서 논문을 통과할 확률을 높이고 싶은 의도도 보인다고..

3. Read but skip/skim math
4. 논문 전체를 읽지만 이해가 안 되는 부분은 넘긴다

    인용 수가 엄청나게 많은 좋은 논문들도 정말 중요한 부분이 있고, 이후에는 거의 안 쓰이고 묻히는 중요하지 않은 부분도 있다. 뭐 이건 나중에 시간이 지나봐야 알 수 있고, 당시의 저자들은 그걸 알 수가 없으니 일단 싣는다. 예를 들면 유명한 LeNet-5 논문에서 ConvNet이 처음 나왔고 정말 큰 파급력을 가져왔지만 그 논문의 절반은 Transducer 등 지금은 잘 쓰이지 않는 다른 개념에 대한 내용이다. 그러니까 이해가 안 된다/말이 안 된다고 생각하면(don't make sense) 그냥 skim하고 다른 거 봐도 된다.

    물론 내가 연구를 하는 데 필요하고 깊은 이해가 필요하다 하면 당연히 시간을 들여서 잘 이해해야 한다. 다만 많은 리서치 논문을 읽어야 한다면 중요한 것에 우선순위를 둬서 시간을 잘 써야 한다. ㅇㅋ?

그리고 논문을 읽을 때는 다음 질문들에 대해 답변할 수 있을 정도로 이해해야 한다. 논문을 읽고 나면 주변 사람들과 이 질문을 하며 의견을 나눠라.

- What did authors try to accomplish?
- What were the key elements of the approach?
- What can you use yourself?
- What other references do you want to follow?

그 후 7분 동안 Densely Connected Convolutional Network (Guo Hwang et al.) - DenseNet 논문 읽고, 4분 동안 주변 사람들과 의견을 나누는 실습을 진행.

## 기타 꿀팁

### 논문 정보를 얻는 방법

딥러닝 분야는 정말 빠르게 발전하고 있는데, 그럼 어디에서 정보를 얻어야 하나?

웹 검색을 해도 되고, 블로그 포스트를 봐도 되고.

Andrew Ng이 정보를 얻는 방법들은 다음과 같다.

- Twitter

    Andrew Ng이 Surprisingly important place for researchers to find out about new things라고 표현

- ML Subreddit
- NIPS / ICML / ICLR와 같은 탑 컨퍼런스
- ArXiv Sanity
- Friends (Slack, ...)

    
    (역자 첨부 내용) [Connected Paper](http://www.connectedpapers.com)라는 웹서비스를 활용하면 입력한 논문과 관련된 논문들을 시각화해준다. 너무 최신 논문이거나 citation 적으면 그래프 자체가 안 그려질 수도 있긴 한데 그래도 웬만한 건 나온다.
    
    또 한국의 경우 Facebook 커뮤니티(Tensorflow Korea, AI Korea, PyTorch Korea)나 카톡 오픈챗 등이 활성화되어 있다.

### 논문에 수식이 있는 경우 어떻게 이해할까

Batch Norm 같은 논문을 보면(진짜 어려운 논문들 중 하나인데) 수식이 많다. 이런 걸 잘 이해하려면 어떻게 할까.

- 처음부터 다시 유도해보기

    잘 이해하고 싶다면, theoretical science나 mathematics 박사과정 학생들이 많이 쓰는 방법인데 결과를 따로 적어놓고 빈 종이에다가 유도하는 과정을 다시 해본다. 그걸 하면 정말 잘 이해한 것. 그리고 이걸 할 수 있게 되면 새롭게 일반화를 할 수 있게 되고 새로운 아이디어를 얻는 데 도움이 된다. 시간은 많이 걸리지만 그만큼 도움이 될 것이다.

### 논문에 코드가 있는 경우 어떻게 이해할까

수식과 비슷하다.

- 오픈소스 코드를 받아 실행해보기
- 처음부터 코드를 재현해보기

**Steady reading, not shorts burst**

꾸준히 해라. 잠깐 불태우고 끝내지 말고.

# 2. ML 커리어 조언

목표 : 취업 or 박사

채용 담당자가 보는 것 :

- Skills(ML quiz, coding ability) - academic understanding?
- Meaningful work - can do work?

## T자형 인재가 되어라

Area가 많은데 -  ML, DL, PGM, NLP, CV ... broad area를 알면서도 한 분야의 deep understanding을 가진 T자형 인재가 되어야 한다.

depth는 project, open source contribution, research, internship 등으로 증명하기.

Stanford 학생들 중에서도 모든 분야를 하면서 비슷한 깊이를 파는 학생들이 있는데, 물론 취업하거나 박사과정에 들어갈 수는 있겠지만 그것이 best라고 할 수는 없다. 나쁜 건 아니지만 훌륭한 것도 아니다.

그리고 아직 얼마 배우지도 않은 신입생이 바로 연구 분야로 뛰어드는 경우도 있는데, 이것도 바람직하지는 않은 것 같음. 별로 효율적으로 할 수 있는 것도 아니고. 다양한 수업을 듣고 기초를 키워서 프로젝트를 나중에 좀 더 잘하는 것이 나음. ㅇㅋ? 

후자가 전자보다 안 좋다. 전자는 그래도 나쁜 건 아님.

Broad area는 Foundation, 강의를 듣고 논문을 읽으며 다지기.

Deep understanding은 관련 프로젝트를 진행.

## 토요일 아침의 문제

- 논문 읽기
- 연구하기
- 오픈소스 하기
- TV보기

매주 토요일, 혹은 일요일, 휴일에 뭘 할지는 너네 마음이지만, 1주에 2개 논문을 보면 1년이면 100개고 이렇게 하면 넌 딥러닝에 대해 좀 더 전문가가 되지 않겠니? 참고로 나는 집에 TV 없어 ^^ - Andrew Ng

아냐 그래도 너무 너 자신한테 매몰차게 굴지는 말고.. 건강하게 워라밸도 챙기렴. (어떻게 하라구요.. ㅂㄷㅂㄷ 찐교수님이네 이분)

## 직업을 선택하기

- 좋은 사람, 프로젝트와 함께 일하기
    - 아무리 회사가 커도 결국 closely interact with 하는 사람들은 10~30, 또는 ~50명 까지. 그 사람들에 집중해라. 그들이 얼마나 알고, 너한테 얼마나 알려줄 수 있고, 얼마나 열심히 일하는 지가 중요.
    - Manager
    - 회사명에 의존해서만 결정하지 마라. 같이 일하는 사람들이 너의 personal experience에 좀 더 관련이 있지 회사는 관련이 약함
    - 로테이션 잘못 됐다가 관심 없는 다른 팀 가서 커리어를 망치지 말고, 정말 원하는 팀에 가서 일할 수 있도록 오퍼를 제대로 받고 일해라.