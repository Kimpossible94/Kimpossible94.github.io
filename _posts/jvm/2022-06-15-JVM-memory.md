---
title : "[JVM] Runtime Data Area"
excerpt : "JVM의 Runtime Data Area"
categories : 
    - JVM
tags :
    - [Java, JVM, Runtime Data Area]
toc : true
toc_sticky : true
date : 2022-06-14
last_modified_at: 2022-06-28
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
사진에서 보이듯이 <u>Method Area</u>, <u>Stack Area</u> 영역은 **모든 Thread가 공유**하고  
<u>Stack Area</u>, <u>PC register</u>, <u>Native Method Stack</u> 영역은 **Thread 별로 생성**된다.  
<span style="font-size:11px">[쓰레드(Thread)란?](#)</span>  