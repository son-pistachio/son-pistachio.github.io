---
title: DBSCAN
date: 2020-10-07
categories: [Machine Learning]
tags: [Machine Learning, Clustering, Unsupervised Learning]
---

# DBSCAN
`Density-based spatial clustering of applications with noise`  
- Data Point(D.P)가 얼마나 짧은지로 군집  
- a density-based clustering non-parametric algorithm

![](https://github.com/alias-son/alias-son.github.io/blob/main/assets/images/posts/DBSCAN/dbscan_img_1.png?raw=true)<br/>

<img src = "https://github.com/alias-son/alias-son.github.io/blob/main/assets/images/posts/DBSCAN/dbscan_img_1.png?raw=true" width="800px" align="center"><br/>

### **거리측정**  
- Euclidean(유클리디안)  
- Cosine distance = 1 - 코사인유사도<br/>

코사인유사도를 사용하려면, 방향만 보기 때문에 거리를 모두 1로 만들어 같게 해준다.  
유닛벡터를 만든다. $ \frac{\|v\|}{v} $ 이용 => 표준화가 중요 Z~(0,1)<br/>

### **Main concepts**  
사용자 parameter  
- `ε(epsilon)` : 이웃하는 점을 지정하는 반지름  
- `minPts` : 최소 점의 개수<br/>

Data Point는 3가지로 분류  
- `core points`  
  ε을 반지름으로 갖는 점 p(center point)가 minPts보다 많거나 같아야 한다. (including p)  
- `reachable points`  
  ε을 반지름으로 갖는 다른 점 q가 core point의 minPts를 포함한다.  
  reachable points는 core point가 될 수도 안될 수도 있다.  
  core point에 대해서만 reachable 한지 판가름한다.  
- `outliers`(=noise points)  
  reachable points가 아닌 모든 points는 outliers이다.

### **Forming a cluster**

- p(core point)가 자신에게 속할 수 있는 모든 포인트와 군집을 형성한다.
- 각 군집에는 하나 이상의 core point가 존재한다.
- non core points는 군집 일부가 될 수 있지만, 더 많은 point를 군집하는 데 사용할 수 없으므로 `edge`를 형성한다.  
  (edge : 군집의 외각을 표현)
<br/><br/>

> 이상엽교수님  강의 중 @ Yonsei Univ.
