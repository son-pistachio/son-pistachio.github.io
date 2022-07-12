---
title: Folder Split
date: 2022-07-12
categories: [Machine Learning, Deep Learning]
tags: [Machine Learning, Deep Learning, Tensorflow, Pytorch]
---

**폴더 나누기(Folder Split)**  
전체 데이터셋 폴더 -> Train, Validation, Test 폴더로 나누기

---  

# 폴더 예시  

변경 전
```
input/
    class1/
        img1.jpg
        img2.jpg
        ...
    class2/
        imgWhatever.jpg
        ...
    ...
```


```
output/
    train/
        class1/
            img1.jpg
            ...
        class2/
            imga.jpg
            ...
    val/
        class1/
            img2.jpg
            ...
        class2/
            imgb.jpg
            ...
    test/
        class1/
            img3.jpg
            ...
        class2/
            imgc.jpg
            ...
```


# split-folders 설치
```python
pip install split-folders
```

# 사용법

## 비율로 나누기  
- train/val/test 나누기  
ratio 파라미터에 원하는 (train, validation, test) 비율만 입력 ex) (0.6, 0.2, 0.2)  

```python
import splitfolders

splitfolders.ratio("input", output="output", seed=12, ratio=(.6, .2, .2))
```  
  
- train/val 나누기  
train, validation만 나누고 싶으면 두 개의 비율만 입력 ex) (0.8, 0.2)  

```python  
import splitfolders

splitfolders.ratio("input", output="output", seed=12, ratio=(.8, .2))
```  

## 고정 값으로 나누기  
```python
splitfolders.fixed("input", output="output", seed=12, fixed=(100, 100)) 
```

> https://pypi.org/project/split-folders/
