---
title: Tensorflow 2 설치 방법
date: 2020-12-16
categories: [Deep Learning]
tags: [Deep Learning, Tensorflow]
---

Building......  
수정중...

**TensorFlow 설치**  
공식 홈페이지에 따른 안정적인 Tensorflow 설치 ( Window 용 )  
필수적인 요소 중심으로 작성

### TensorFlow 2  패키지
-   `tensorflow`  - CPU와  GPU 지원이 포함된 안정적인 최신 출시
-   `tf-nightly`  - 미리보기 빌드

### 요구사항
- Python 3.5 ~ 3.8
	- Python 3.8 지원은 Tensorflow 2.2 이상
- pip 19.0이상
- windows 7 이상 (64비트)
```
python --version 혹은 python3 --version
pip --version 혹은 pip3 --version
```

### 가상 환경 만들기(권장)
#### Windows용
1. Python 인터프리터를 선택하고 저장할  `.\venv`  디렉터리를 만들어 새로운 가상 환경을 만든다.  
**중간과 맨 뒤의 venv 원하는 이름으로 작성**  
```
C:\) python -m venv --system-site-packages .\venv
```

2. 가상 환경을 활성화
```
(venv) C:\) .\venv\Scripts\activate
```

3. 가상 환경 내에 패키지를 설치 및 `pip` 업그레이드
```
(venv) C:\) pip install --upgrade pip
(venv) C:\) pip list # 가상환경 내 설치된 패키지 확인
```

4. 가상 환경 종료
```
(venv) C:\) deactivate
```

#### Conda용
```
C:\)  conda install -c anaconda tensorflow
```

### TensorFlow pip 패키지 설치
- 가상 환경 설치
```
(venv) $ pip install --upgrade tensorflow
```
설치확인
```
(venv) $ python -c "import tensorflow as tf;print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
```

시스템 설치는 따로 확인!

> https://www.tensorflow.org/install/pip?hl=ko#system-install
