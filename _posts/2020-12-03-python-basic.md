---
title: Jupyter notebook 설정
date: 2020-12-03
categories: [Jupyter Notebook]
tags: [Python, Jupyter Notebook]
---

# 주피터노트북 셀 사이즈 조절하기
```python  
from IPython.core.display import display, HTML  
display(HTML("<style>.container { width:100% !important; }</style>"))  
pd.options.display.max_columns = 999  
pd.options.display.max_rows = 20000  
```  


## 주피터노트북 입력 셀 사이즈

{% highlight ruby %}
display(HTML("<style>.container { width:100% !important; }</style>"))
{% endhighlight %}

**width %**를 조절하면 브라우저 내 셀 너비를 조절할 수 있다.  


## 주피터노트북 데이터프레임 출력 수

{% highlight ruby %}
pd.options.display.max_columns = 999
pd.options.display.max_rows = 20000
{% endhighlight %}

데이터프레임 출력 row/column 수를 조절할 수 있다. 
