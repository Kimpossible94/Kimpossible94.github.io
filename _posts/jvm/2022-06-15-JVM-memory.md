---
title : "[JVM] Runtime Data Area - 작성중"
excerpt : "JVM의 Runtime Data Area"
categories : 
    - JVM
tags :
    - [Java, JVM, Runtime Data Area]
toc : true
toc_sticky : true
date : 2022-06-14
last_modified_at: 2022-07-26
published: true
---

# 1. Runtime Data Area란 
---
자바 애플리케이션을 실행하면 JVM이 OS로부터 메모리 공간을 할당받고  
JVM은 할당받은 메모리를 용도에 따라 여러 영역으로 나누어 관리하게 되는데  
관리하는 전체 영역을 <span class="h-text-p">Runtime Data Area</span>라고 부른다.  
<p align="center"><img src="/assets/images/JVM/memory/MEMORY-1.png"></p>
메모리 영역은 <span class="h-text-p">Method Area</span>,
<span class="h-text-p">Heap Area</span>,
<span class="h-text-p">Stack Area</span>, 
<span class="h-text-p">PC register</span>,
<span class="h-text-p">Native Method Stack</span>으로 나뉜다.   
사진에서 보이듯이 <u>Method Area</u>, <u>Heap Area</u> 영역은 **모든 Thread가 공유**하고  
<u>Stack Area</u>, <u>PC register</u>, <u>Native Method Stack</u> 영역은 **Thread 별로 생성**된다.  
<span style="font-size:11px">[쓰레드(Thread)란?](#)</span>  
모든 Thread가 공유하는 **Method Area**, **Heap Area**는 Java Vertual Machine 시작 시 생성되고, JVM이 소멸될 떄 소멸한다.  
그리고 나머지 영역은 쓰레드가 생성될 때 생성되고, 쓰레드 종료될 때 소멸한다.  
  

<div class='next-line'></div>  
  
# 2. Thread별로 생성되는 영역  
## Native Method Stack
Java로 작성된 프로그램을 실행하면서, 순수하게 Java로 구성된 코드만을 사용할 수 없는 시스템의 자원이나 API가 존재한다.  
이렇게 **다른 프로그래밍 언어로 작성된 메서드들을 Native Method**라고 한다.  
Native Method Stacks는 이런 Native Method를 다루는 영역을 말한다. **'C Stacks'**라고도 불린다.  
  
## PC(Program Counter) Register  
JVM은 한 번에 많은 Thread를 실행할 수 있고, 각 Thread는 각자의 메서드를 실행한다.  
이 때 Thread별로 동시에 실행하는 환경이 보장되어야 하므로 <span class="h-text-y">PC Register는 Thread에서 실행할 다음 명령어의 주소를 저장한다.</span>  
  
**다음 명령어의 주소를 저장하는 이유** CPU는 쓰레드 사이를 전환하며 명령을 수행하기 때문에 **Context Switching**이 일어나는 순간에 실행할 위치를 알아야 하기 때문이다. 이것을 이용해 다수의 쓰레드들이 명령의 흐름을 잃지 않고 실행된다.  

## Stack Area  
JVM Stack은 Thread에서 
<span class="h-text-p">로컬 변수</span>, 
<span class="h-text-p">부분 결과</span>, 
<span class="h-text-p">메서드 호출 및 반환</span>에 대한 데이터를 저장하는데 사용된다.  
<p style="border: solid black 1px" align="center"><img src="/assets/images/JVM/runtime-data/JVM-STACK.jpg"></p>  
각 Thread는 메서드를 호출할 때마다 <span class="h-text-y">Frame</span>이라는 단위로 Stack Area에 추가한다. 메서드가 실행되어 결과를 반환하면 해당 Frame은 Stack Area에서 
제거가 된다.  
  
<details>
<summary style="font-weight: bold">Frame에 대한 설명</summary>
<div markdown="1">       

### Frame
프레임은 데이터 및 부분 결과를 저장, 동적 연결, 메서드 결과값 반환, 예외 전달을 수행한다.  
프레임은 메서드 호출이 완료되면 결과가 정상이든 갑자스러운 종료(예외 발생)이든 상관없이 없어진다.  
  
<p style="border: solid black 1px" align="center"><img src="/assets/images/JVM/runtime-data/frame.png"></p>  
  
Frame은 <span class="h-text-p">Local vaiable array</span>, <span class="h-text-p">Operand Stack</span>, <span class="h-text-p">Constant Pool Reference</span> 이렇게 세부분으로 나뉜다.  
Local Variable : 메서드 안의 지역 변수들이 있는 공간    
Operand Stack : 메서드 내 연산을 위해서, 바이트 코드 명령문들이 들어있는 공간  
Constant Pool Reference : Constant Pool 참조를 위한 공간  

</div>
</details>  

  
<div class='next-line'></div>  
<div class='next-line'></div>  
  

# 3. 모든 Thread가 공유하는 영역
## Method Area  
**Class/Interface**에 대한 
<span class="tooltip h-text-y">
    **Metadata<sup>💬</sup>**
    <span class="tooltip-text">
        데이터에 관한 구조화된 데이터로, 다른 데이터를 설명해 주는 데이터
    </span>
</span>를 저장한다.<br><br>
클래스 로더에 의해서 로드된 **Class의 정보(Class의 Method, Field이름, Field타입, Class 이름 등)**가 여기에 저장된다.  
또한, Runtime Constant Pool 과 static 변수와 같은 것들도 저장이 된다.  
Method Area는 Heap Area의 일부일 수도 있고 아닐 수도 있으며, 이를 저장할 위치를 결정하는 것은 전적으로 JVM 구현에 달려 있다.  
  
<details>
<summary style="font-weight: bold">PermGen, MetaSpace에 대한 설명</summary>
<div markdown="1">       

### PermGen, MetaSpace
<p style="border: solid black 1px" align="center"><img src="/assets/images/JVM/runtime-data/permgen_java7.png"></p>  
<p style="border: solid black 1px" align="center"><img src="/assets/images/JVM/runtime-data/permgen_java8.png"></p>  
Java8 버전 이전에서는 Method Area는 <span class="h-text-p">PermGen(Permanent Generation Space)</span>에 할당되었다.  
Java8 버전 이후에는 PermGen이 완전히 제거되고 Method Area는 <span class="h-text-p">MetaSpace</span>에 할당된다.  
  
PermGen은 OS, JVM 버전마다 각기 다른 default 값을 가지고 있었고, 대부분 작게 할당되어 있었다.  
그래서 클래스 로딩을 많이 하게 되면 PermGen이 부족해 에러가 발생했다.  
MetaSpace는 Class의 Metadata를 Native 메모리에 저장하고 부족할 경우 자동으로 늘려준다.  

</div>
</details>  
  
## Heap Area
Heap Area은 메서드 안에서 사용되는 객체들을 위한 영역으로 **new를 통해 생성된 객체, 배열, immutal 객체 등의 값**이 저장된다.  
해당 영역에서 생성된 객체들은 JVM Stack Area의 변수나 다른 객체의 필드에서 참조가 가능하다. 
따라서, Heap 에서는 참조되지 않는 인스턴스와 배열에 대한 정보 또한 있을 수 있기 때문에 GC 의 주 대상이 된다.  
  
<details>
<summary style="font-weight: bold">Hotspot JVM 대한 설명</summary>
<div markdown="1">       

### Hotspot JVM
Hotspot JVM은 미국의 Longview Technologies LLC라는 회사에서 1999년에 처음 발표된 JVM이다.  
이후 이 회사는 같은 해 SUN에 의해 인수되어 1.3버전부터 SUN의 기본적인 JVM이 되었다.  
Hotspot JVM은 현재 가장 일반적인 JVM 중의 하나로 Windows, Linux, Solaris는 물론 Mac OS와 기타 UJnix OS에도 탑재가 가능하다.  
HP Unix에서도 Hotspot JVM을 제공하고 있고, 심지어 Oracle을 설치할 때 기본적으로 설치되어 구동되는 JVM 조차 Hotspot JVM이다.  
따라서 Sun, Oracle, HP, Windows, Linux, MacOS에서 제공하는 JVM은 Hotspot JVM으로 명명하고 IBM에서 제공하는 JVM은 IBM JVM이라 부르기도 한다.  

### Hotspot JVM의 Heap 구조
Hotspot JVM의 Heap은 크게 Young Generation과 Old Generation으로 나누어져 있다.  
  
#### Young Generation
Young Generation은 다시 Survivor 영역과 Eden 영역으로 나뉜다.  
Eden영역은 Object가 Heap에 최초로 할당되는 장소이며, Eden영역이 꽉 차게 되면 Object의 참조 여부를 따져 참조가 되어있는 Object라면 Survivor 영역으로 넘기고  
참조가 되어있지 않은 Garbage Object라면 그냥 남겨 놓는다.  
참조되어있는 모든 Object가 Survivor영역으로 넘어가면 Eden영역을 모두 청소한다.  
  

<p style="border: solid black 1px" align="center"><img src="/assets/images/JVM/runtime-data/permgen_java7.png"></p>  
<p style="border: solid black 1px" align="center"><img src="/assets/images/JVM/runtime-data/permgen_java8.png"></p>  
Java8 버전 이전에서는 Method Area는 <span class="h-text-p">PermGen(Permanent Generation Space)</span>에 할당되었다.  
Java8 버전 이후에는 PermGen이 완전히 제거되고 Method Area는 <span class="h-text-p">MetaSpace</span>에 할당된다.  
  
PermGen은 OS, JVM 버전마다 각기 다른 default 값을 가지고 있었고, 대부분 작게 할당되어 있었다.  
그래서 클래스 로딩을 많이 하게 되면 PermGen이 부족해 에러가 발생했다.  
MetaSpace는 Class의 Metadata를 Native 메모리에 저장하고 부족할 경우 자동으로 늘려준다.  

</div>
</details>  



