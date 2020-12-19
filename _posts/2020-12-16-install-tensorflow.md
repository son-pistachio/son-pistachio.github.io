---
title: Tensorflow 2 설치 방법
date: 2020-12-16
categories: [Deep Learning]
tags: [Deep Learning, Tensorflow]
---

## 공식 홈페이지에 따른 설치 방법 ( for Windows )
필수적인 요소 중심으로 작성

### TensorFlow 2  패키지
- `tensorflow`  - CPU와  GPU 지원이 포함된 안정적인 최신 출시
- `tf-nightly`  - 미리보기 빌드

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

#### Windows 용

1. Python 인터프리터를 선택하고 저장할  `.\venv`  디렉터리를 만들어 새로운 가상 환경을 만든다.
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

#### Conda 사용
```
C:\) conda install -c anaconda tensorflow
```

### TensorFlow pip 패키지 설치
가상 환경 설치
```
(venv) $ pip install --upgrade tensorflow
```
설치확인
```
(venv) $ python -c "import tensorflow as tf;print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
```

*시스템 설치는 따로 확인!*

> https://www.tensorflow.org/install/pip?hl=ko#system-install

<br/>

## 다른 방법
- 위의 방법으로 설치가 이루어지지 않아 작성
- 프로젝트 별로 폴더 관리 가능
- 모든 라이브러리를 다시 설치해야 하는 단점

### 관리할 위치에 폴더 생성 ( 여기에서는 C: )  
C:\tf_env  
<img src = "https://github.com/alias-son/alias-son.github.io/blob/main/assets/images/posts/Install_tensorflow/1.png?raw=true"  width="200px" border="50" align="center"><br/>

### 찾기 ⇒ cmd (command, 명령 프롬프트) 실행  
<img src = "https://github.com/alias-son/alias-son.github.io/blob/main/assets/images/posts/Install_tensorflow/2.png?raw=true" width="300px" align="center"><br/>

### ipykernel (python 3.x)을 사용하여 새로운 환경 생성
```
C:\) conda create –n tf python=3.7 ipykernel
```
`tf`는 사용할 환경이름으로 설정

### tf 환경 활성화
```
C:\) activate tf
```

### Workspace 생성
- 1번에서 생성한 tf_env 폴더로 이동 후, tf 환경에 패키지 설치  
```
(tf) C:\) cd c:\tf_env
```

### tf 환경에 필요한 라이브러리 설치  
`conda install pandas`  
`conda install scipy`  
`conda install matplotlib`  
`conda install seaborn`  
`conda install scikit-learn`  
`conda install tensorflow`  
`conda install keras`  
`**conda install notebook ipykernel**`

### jupyter notebook 실행
```
(tf) C:\) jupyter notebook
```
