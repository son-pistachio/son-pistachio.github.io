---
title: Brute Force, Divide and Conquer
date: 2020-11-07
categories: [Algorithm]
tags: [Algorithm]
---

# Brute Force
순차적으로 모두 확인하는 방식  
예로, [1, 3], [6, 7] 카드에서 가장 큰 곱의 카드를 구한다면,  
1 x 6 = 6  
1 x 7 = 7  
3 x 6 = 18  
3 x 7 = 21  
가장 큰 수는 21이다.

```python
left_cards = [1, 3]
right_cards = [6, 7]

for left in left_cards:
        for right in right_cards:
            max_product = max(max_product, left * right)
print(max_product)
```

Input이 많아지면 경우의 수가 많아지므로, 일반적으로 Brute Force 알고리즘은 비효율적이다.

<!-- more -->

`Brute Force 장점`  
직관적이고 명확하다  
답을 확실하게 찾을 수 있다  


효율적인 알고리즘의 시작은 Brute Force로 생각해보고 발전시킨다.<br/><br/>


#  Divide and Conquer

Divide and Conquer (분할 정복)

큰 문제를 해결하기 위해, 부분 문제로 나누어서 답을 찾아 문제를 해결하는 방식

1. `Divide` - 문제를 부분 문제로 나눈다.  
2. `Conquer` - 각 부분문제를 풀어 답을 도출한다.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(부분문제가 어려우면, 부분문제를 쪼개서 Divide & Conquer로 풀어야 한다)  
3. `Combine` - 부분문제의 솔루션을 합쳐서, 기존문제를 해결한다.<br/>


## 합병정렬(Merge Sort)

정렬 알고리즘  
선택정렬(Selection Sort), 삽입정렬(Insertion Sort), 합병정렬(Merge Sort)

**Merge Sort Divide and Conquer**  
`Divide` - 리스트를 반으로 나눈다.  
`Conquer` - 왼쪽 리스트와 오른쪽 리스트를 각각 정렬한다.  
`Combine` - 정렬된 두 리스트를 하나의 정렬된 리스트로 합병한다.  

Combine 부분에서 좌우의 리스트를 합병할 때,  
각 리스트의 가장 왼쪽이 작은 수이므로 대소 비교하면서 하나의 리스트로 합병한다.<br/>


### 합병정렬 예시  
다음과 같이 숫자 8개의 리스트가 존재.  
[16, 11, 6, 13, 1, 7, 10, 4]<br/>

- **Divide 단계**  
  [16, 11, 6, 13] &nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp; [1, 7, 10, 4]  
  리스트를 반으로 나눈다.  
  `Conquer단계`에서는 나누어진 리스트를 각각 정리해주어야 하지만,  
  리스트 길이가 길어, 재귀적으로 다시 `Divide and conquer` 실행<br/>


  왼쪽 리스트부터 다시 `Divide and conquer`

  - **Divide 단계**  
    [16, 11] &nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp; [6, 13]  
    리스트를 반으로 나누어 준다.  
    이번에도 리스트가 충분히 작지 않아서 다시 `Divide and conquer` 실행<br/>

    왼쪽 리스트 [16, 11] `Divide and conquer`  
    - **Divide 단계**  
      [16] &nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp; [11]  
      리스트를 반으로 나누어 준다.<br/> 

    - **Conquer 단계**  
      리스트를 각각 정렬해주어야 하는데,  
      이번에는 요소가 하나이므로 정렬되었다고 할 수 있다.`정복`<br/> 

    - **Combine 단계**  
      정복한 문제를 기반으로 `combine단계`에서 합쳐준다.  
      16보다 11이 작으므로, 11 ⇨ 16으로 순서대로 넣어준다.  
      [11, 16]<br/>

    오른쪽 리스트 [6, 13] `Divide and conquer`  
    - **Divide 단계**  
      [6] &nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp; [13]  
      리스트를 반으로 나누어 준다.<br/> 

    - **Conquer 단계**  
      리스트를 각각 정렬해주어야 하는데,  
      요소가 하나이므로 정렬되었다고 할 수 있다.`정복`<br/>

    - **Combine 단계**  
      정복한 문제를 기반으로 `combine단계`에서 합쳐준다.  
      6 ⇨ 13으로 순서대로 넣어준다.  
      [6, 13]<br/>

  - **Conquer 단계**  
    [11, 16], [6,13] 문제 `정복`<br/>

  - **Combine 단계**  
    두 정렬된 리스트를 합쳐준다.  
    우선, 가장 왼쪽 값들만 확인.  
    11보다 6이 작으므로, [6]  
    11이 13보다 작으므로, [6 ⇨ 11]  
    16보다 13이 작으니까, [6 ⇨ 11 ⇨ 13]  
    마지막 남은 16, [6 ⇨ 11 ⇨ 13 ⇨ 16]  

    왼쪽리스트 완벽하게 정렬 [6, 11, 13, 16]<br/>

  [1, 7, 10, 4] 오른쪽도 같은 방법으로 `Divide and conquer` 진행  
- **Conquer 단계**  
  왼쪽 리스트: [6, 11, 13, 16]  
  오른쪽 리스트: [1, 4, 7, 10]  
  `정복`<br/>

- **Combine 단계**  
   [6, 11, 13, 16] &nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp; [1, 4, 7, 10]  
  마지막으로, 두 리스트를 작은 순서대로 합쳐 주면 된다.  
  [1, 4, 6, 7, 10, 11, 13 ,16]<br/>


---
## 퀵정렬(Quick Sort)  
<u>퀵 정렬은 Divide 과정을 생각해야한다.</u><br/>

`Divide` : pivot값보다 작으면 왼쪽으로, 크면 오른쪽으로 위치시킨다.  
`Conquer` : pivot 값의 양쪽을 각각 정렬시킨다.  
Divide와 Conquer 단계만 거치면 자동으로 정렬되므로 Combine은 할 일이 없다.<br/> 

### 퀵정렬 예시  
Partition - 퀵 정렬에서 리스트를 나누는 과정 (Divide 단계)  
pivot ℗ - 정렬을 위한 기준 점<br/>

[16, 11, 6, 13, 1, 4, 10, 7]<br/>

- **Divide 단계**

  |  16  |  11  |  6   |  13  |  1   |  4   |  10  | <span style="color:red">7</span> |
  | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :------------------------------: |
  |      |      |      |      |      |      |      |                ℗                 |
  
  Partition을 위하여, 리스트 맨 끝 값인 7를 pivot으로 지정,
  
  |  6   |  1   |  4   | <span style="color:red">7</span> |  11  |  16  |  10  |  13  |
  | :--: | :--: | :--: | :------------------------------: | :--: | :--: | :--: | :--: |
  |      |      |      |                ℗                 |      |      |      |      |
  
  ℗ 7보다 작은 값은 왼쪽으로, 큰 값은 오른쪽으로 위치시킨다.  
  숫자 7의 Conquer 단계에서, 
  왼쪽리스트 정렬을 위해 다시 `Divide and conquer`<br/>

  - **Divide 단계**
  
    |  6   |  1   | <span style="color:red">4</span> |  7   |  11  |  16  |  10  |  13  |
    | :--: | :--: | :------------------------------: | :--: | :--: | :--: | :--: | :--: |
    |      |      |                ℗                 |      |      |      |      |      |
    
    이전 pivot인 7 왼쪽 숫자 중 끝의 4를 pivot으로 지정.<br/>

    |  1   | <span style="color:red">4</span> |  6   |  7   |  11  |  16  |  10  |  13  |
    | :--: | :------------------------------: | :--: | :--: | :--: | :--: | :--: | :--: |
    |      |                ℗                 |      |      |      |      |      |      |
    
    ℗ 4보다 작은 값은 왼쪽으로, 큰 값은 오른쪽으로 위치시킨다.<br/>

  - **Conquer 단계**  
    ℗ 4의 양 값이 하나밖에 없어서 이미 정렬된 것이다.  
    **값이 하나 밖에 없는 이 경우가 base case이다.**  
    숫자 7의 왼쪽 부분은 완벽하게 정리되었다.<br/>

    숫자 7의 오른쪽을 정렬을 위해 `Divide and Conquer`를 진행  
  - **Divide 단계**
  
    |  6   |  1   |  4   |  7   |  11  |  16  |  10  | <span style="color:red">13</span> |
    | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :-------------------------------: |
    |      |      |      |      |      |      |      |                 ℗                 |
    
    숫자 7 오른쪽 끝의 숫자 13을 pivot으로 지정<br/>

    |  6   |  1   |  4   |  7   |  11  |  10  | <span style="color:red">13</span> |  16  |
    | :--: | :--: | :--: | :--: | :--: | :--: | :-------------------------------: | :--: |
    |      |      |      |      |      |      |                 ℗                 |      |
    
    ℗ 13보다 작은 값은 왼쪽으로, 큰 값은 오른쪽으로 위치시킨다.<br/>

    ℗ 13의 Conquer 단계에서, 
    왼쪽리스트 정렬을 위해 다시 `Divide and conquer`  
    - **Divide 단계**
    
      |  6   |  1   |  4   |  7   |  11  | <span style="color:red">10</span> |  13  |  16  |
      | :--: | :--: | :--: | :--: | :--: | :-------------------------------: | :--: | :--: |
      |      |      |      |      |      |                 ℗                 |      |      |
      
      이전 pivot 13의 왼쪽의 가장 마지막 값인 10을 pivot으로 지정.<br/>

      |  6   |  1   |  4   |  7   | <span style="color:red">10</span> |  11  |  13  |  16  |
      | :--: | :--: | :--: | :--: | :-------------------------------: | :--: | :--: | :--: |
      |      |      |      |      |                 ℗                 |      |      |      |
      
      ℗ 10보다 작은 값은 왼쪽으로, 큰 값은 오른쪽으로 위치시킨다.<br/>

    - **Conquer 단계**  
      ℗ 10의 왼쪽 값이 없고, 오른쪽 값이 하나이므로 base case<br/>

  - **Conquer 단계**  
    숫자 13의 오른쪽은 값은 base case로 정복.<br/>

- **Conquer 단계**  
  가장 처음인 ℗ 7의 양쪽 모두 완벽하게 정렬되었으므로, 리스트 전체가 정렬되었고 퀵 정렬 종료.<br/><br/>
  
  



> https://www.codeit.kr/courses/algorithms &nbsp;&nbsp;&nbsp;&nbsp; <코드잇>
