---
title : "[Algorithm] 점근 표기법 (asymptotic notation)"
excerpt : "접근 표기법에 대해 알아보자 !"
categories : 
    - Algorithm
tags :
    - [Algorithm, 알고리즘, asymptotic notation, 점근 표기법]
toc : true
toc_sticky : true
date : 2022-06-28
last_modified_at: 2022-07-03
published: true
---

# 점근 표기법
알고리즘을 공부하면 복잡도에 대해 <span class="h-text-y">점근 표기법</span>으로 표현한다는 것을 알 수 있다.  
그렇다면 점근 표기법이란 무엇일까?  
점근 표기법에 대해서 알고난 후 알고리즘 공부를 시작하자.  

---
## 컴퓨터 알고리즘의 수행 시간은 실행환경에 따라 다르게 측정된다
동일한 알고리즘을 슈퍼컴퓨터와 내 노트북에서 수행할 때는 분명 속도 면에서 차이가 날 것이다.  
수행환경을 통제하지 않고서는 공정한 비교라고 할 수 없기 때문에 수행환경을 통제해야 한다.  
  
하지만, 수행환경을 통제하는 것은 어렵기 때문에 컴퓨터의 성능과 관계없이  
데이터 개수(n)가 주어졌을 때 <span class="h-text-y">기본 연산(덧셈, 뺄셈, 곱셈 등)의 실행 횟수</span>로 성능을 분석한다.  
연산의 실행 횟수는 데이터 개수(n)에 대한 함수로 표현한다.  
ex) \\(f(n) = n^2+3n+1 \\)  

이렇게 따져본 내 알고리즘의 계산복잡성이 \\(20/πn^2 + n + 100\\) 이고,  
다른 개발자가 내놓은 알고리즘은 \\(100000n + 100000\\)이라고 가정해보자.  

두 개의 알고리즘 중 어떤게 더 효율적인지 단박에 알아내기가 어렵다.  
이럴 때 가늠하려는 알고리즘의 복잡성 증가 양상을 단순화 시켜 우리가 익히 알고 있는  
로그, 지수, 다항함수 등과의 비교로 표현하는 것이 ***점근 표기법***이다.  

점근 표기법에서는 원래의 함수를 단순화하여 최고차항을 제외한 모든 항과 최고차항의 계수는 무시하고 <span class="h-text-y">최고차항의 차수만을 고려</span>한다.
이 표기법을 따르면 \\(20/πn^2 + n + 100\\)의 복잡도는 \\(n^2\\)이고, \\(100000n + 100000\\)의 복잡도는 \\(n\\)이다.

<p align="center"><img style="width:400px; height:400px" src="/assets/images/algorithm/time-1.png"></p>  
<center>x : 데이터, y : 시간</center>  
    
<br>
그래프를 보면 다른 개발자의 알고리즘보다 내가 만든 알고리즘이 데이터가 많아질수록 시간이 더 오래걸리는 것을 알 수 있다.

## 점근 표기법의 종류
접근 표기법의 대표적인 종류는 다음 세가지가 있다.  
- <span class="h-text-p">빅오 표기법 (Big-O Noation)</span> 
- <span class="h-text-p">오메가 표기법 (Big-Ω Noation)</span>
- <span class="h-text-p">세타 표기법 (Big-θ Noation)</span>


### Big-O
빅오 표기법은 <span class="h-text-y">최악의 경우에서의 시간 복잡도</span>를 나타낸다.  
최악의 경우에서의 시간 복잡도라.. 무슨 말일까 ?  
  
빅오 표기법이 나타내는 의미를 수학적 정의를 통해 보자.  
  
모든 \\(n \ge n_0 \ge 0\\)에 대하여 \\(0 \le f(n) \le k*g(n)\\)인 양의 상수 \\(k\\)와 \\(n_0\\)가 존재하면 \\(f(n)=O(g(n))\\)이다.  
  
이래도 무슨 말인지 감이 잘 오지 않는다.  
  
예를 들어보자  **\\(f(n)\\)은 내가 만든 알고리즘의 효율성**을 나타내고, 충분히 큰 값인 n과 상수 k에 대해  
  
\\(f(n) = 2n^2\\)  
\\(g(n) = n^3\\)  
  
이라고 가정하면, \\(f(n) \le k*g(n)\\) ===> (\\(2n^2 \le n^3\\))이 성립하는 k값(=1)이 존재하기 때문에  
내가 만든 알고리즘 \\(f(n)\\)은 빅오 표기법을 사용하여 \\(O(n^2)\\)로 나타낼 수 있다.
  
추가적인 이해를 돕기 위해 아래의 그림을 보자.  

<p align="center"><img style="width:400px; height:400px" src="/assets/images/algorithm/time-2.png"></p>  
    
내가 만든 알고리즘의 효율성이 \\(f(n)\\)이다.  
이 때 \\(n_0\\)보다 큰 입력값인 \\(n\\)을 넣었을 때 내가 만든 알고리즘의 효율성이 최악인 경우에도 점근 상한선인\\(k*g(n)\\)을 넘을수가 없다.  
이 때 빅오 표기법을 사용하여 \\(O(g(n))\\)이라고 나타내고,  
\\(f(n)\\)의 복잡도는 <span class="h-text-p">최악의 경우</span>라도 \\(g(n)\\)과 같거나 혹은 작다는 뜻이다.

### Big-Ω와 Big-θ는 대략적인 개념
최악의 경우에도 해당 알고리즘이 어떤 성능을 낼지 가늠해 볼 수 있기때문에  
알고리즘의 복잡도를 따질 때 Big-O 표기법이 가장 많이 쓰인다.  
그러므로 **Big-Ω, Big-θ에 대해서는 간략하게 알아보자.**

### Big-Ω
빅 세타 표기법에서는 아래 그림을 만족하는 \\(f(n)\\)을 \\(Ω(g(n))\\)이라고 표시한다.  
이 때 ***\\(g(n)\\)은 \\(f(n)\\)의 점근 하한***이라고 한다.  
이 말은 내가 만든 알고리즘 \\(f(n)\\)은 <span class="h-text-p">최선의 경우</span>를 생각하더라도 그 복잡도는 \\(g(n)\\)과 같거나 크다라는 뜻이다.
<p align="center"><img style="width:400px; height:400px" src="/assets/images/algorithm/time-3.png"></p>  

### Big-θ
빅 세타 표기법에서는 아래 그림을 만족하는 \\(f(n)\\)을 \\(θ(g(n))\\)이라고 표시한다.  
이 때 ***\\(g(n)\\)은 \\(f(n)\\)의 점근적 상한선과 하한의 교집합***이라고 한다.  
이 말은 내가 만든 알고리즘 \\(f(n)\\)은 <span class="h-text-p">아무리 나쁘거나 좋더라도</span> 그 복잡도는 \\(g(n)\\)의 범위 내에 있다라는 뜻이다.
<p align="center"><img style="width:400px; height:400px" src="/assets/images/algorithm/time-4.png"></p>  


## Big-O 표기법 표기 방법
---
Big-O 표기법에서 주로 사용되는 표기법을 아래 사진에서 보자.

<p align="center"><img style="width:400px; height:400px" src="/assets/images/algorithm/time-1.png"></p>  

사진에 나와있는 성능을 비교하면 다음과 같다.  
  
\\(O(1) < O(log n) < O(n) < O(n log n) < O(n^2) < O(2^n)\\)  
  
오른쪽으로 갈수록 알고리즘의 효율성이 떨어진다.

### Big-O 표기법 예제
--- 
   
\\(O(1)\\) : 스택에서 Push, Pop  
\\(O(log n)\\) : 이진트리  
\\(O(n)\\) : for 문  
\\(O(n log n)\\) : 퀵 정렬(quick sort), 병합정렬(merge sort), 힙 정렬(heap Sort)  
\\(O(n^2)\\): 이중 for 문, 삽입정렬(insertion sort), 거품정렬(bubble sort), 선택정렬(selection sort)  
\\(O(2^n)\\) : 피보나치 수열   
  
빅오 표기법의 예제에 어떤 알고리즘이 있는지 알아보고 알고리즘 공부를 시작하면 좋을 것 같다.
