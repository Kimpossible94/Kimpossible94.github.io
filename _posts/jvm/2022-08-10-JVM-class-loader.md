---
title : "[JVM] Class Loader"
excerpt : "JVM의 Class Loader에 대해 알아보자."
categories : 
    - JVM
tags :
    - [Java, JVM, Class Loader]
toc : true
toc_sticky : true
date : 2022-08-10
last_modified_at: 2022-09-18
published: true
---

# 1. Class Loader란 ?
---
Java는 Runtime동안 필요한 클래스 파일을 동적으로 읽어오는데 Class Loader는 여기서 Java 클래스를 JVM에 **동적으로 로드하는 역할을 한다.** JVM이 동작하다가 필요한 순간(클래스 파일을 참조하는 순간)에 클래스 파일을 읽어 메모리에 동적으로 로드한다.
  
Class Loader 덕분에 JVM은 Java 프로그램을 실행하기 위한 정보들을 모두 가지고 있을 필요가 없다.
  
<hr style="border-top: 2px solid lightgray;">
<div class='next-line'></div>  
  
# 2. 내장 Class Loader의 종류
## 2-1. Bootstrap Class Loader  
모든 Java 클래스는 [Java.lang.ClassLoader](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/ClassLoader.html)에 의해서 로드된다. **하지만 ClassLoader 자체도 클래스이다.**  
  
ClassLoader 자체도 클래스라면 **ClassLoader는 누가 로드할까 ?** 어딘가에서 로드해서 ClassLoader가 작동을 할테니 말이다.  
이러한 ClassLoader를 로드하는 것이 <span class="h-text-p">Bootstrap Class Loader</span>이다.  
  
- Bootstrap Class Loader는 \\($\\)Java_HOME/jre/lib 에 있는 <span class="h-text-y">**rt.jar 및 기타 핵심 라이브러리와 같은 JDK 내부의 클래스를 로드하는 역할**</span>을 한다.*(Java 8 기준)*  
- 또한 Bootstrap Class Loader는 <span class="h-text-y">**모든 ClassLoader 인스턴스의 부모 역할**</span>이다.  
- Bootstrap Class Loader는 Java가 아닌 <span class="h-text-y">**네이티브 언어(Native C)로 작성**</span>되어 있다.
- 우리가 흔히 사용하는 **java.lang** 패키지 안에 있는 클래스들을 로드한다고 생각하면 된다. (ex. java.lang.Object, java.lang.Classloader)

## 2-2. Extension Class Loader *(Java 8 기준이름)*
- Extension Class Loader는 **Bootstrap Class Loader의 자식**이다.
- 일반적으로 \\($\\)JAVA_HOME/jre/lib/ext 폴더나 java.ext.dirs 환경 변수로 지정된 폴더에 있는 클래스 파일을 로딩한다.*(Java 8 기준)*  
- URLClassLoader를 상속하고 있다.*(Java 8 기준)*
- Bootstrap Class Loader와 다르게 **Java로 구현**되어 있다.

## 2-3. Application Class Loader *(Java 8 기준이름)*
- **Extension Class Loader를 부모로 둔다.**
- 지정된 class path에 있는 클래스들을 로딩한다.
- sun.misc.Launcher 클래스 안에 static클래스로 구현되어 있으며, URLClassLoader를 상속하고 있다.*(Java 8 기준)*
  
<hr style="border-top: 2px solid lightgray;">
<div class='next-line'></div>  
  
# 3. Java 9 이상 버전에서의 내장 Class Loader
위의 내용에서 Java 8 기준이라고 적힌 내용을 볼 수 있다.  
<span class="h-text-p">Java 9 버전 이상부터는 모듈 시스템의 도입에 맞춰 이름과 범위 구현 내용 등이 바뀌었기 때문이다.</span>
  
## 3-1. rt.jar, tools.jar가 제거됨
<p style="border: solid black 1px" align="center"><img src="/assets/images/JVM/class-loader/remove-re.jar-and-tools.jar.png"></p>  
rt.jar, tools.jar와 기타 다양한 내부 JAR파일에 저장된 클래스 및 리소스 파일은 보다 효율적인 형식으로 lib폴더 안에 저장된다.  
이에 따라 **rt.jar내의 클래스를 로딩하던 Bootstrap Class Loader가 로딩할 수 있는 범위가 줄어들었다.**  

## 3-2. jre/lib/ext, java.ext.dirs, lib/endorsed, java.endorsed.dirs가 제거됨
<p style="border: solid black 1px" align="center"><img src="/assets/images/JVM/class-loader/remove-extension-mechanism.png"></p>  
위에 언급한 부분이 제거됨에 따라 jre/lib/ext, lib/endorsed가 파일 시스템에 존재하거나 java.ext.dirs, java.endorsed.dirs가 환경변수로 설정되어 있다면 javac나 java는 종료된다.

## 3-3. 변경내용 정리
  

| Java 8 | Java 9 | 변경 내용 |
| :-----------: | :---------: | :--------- |
| Bootstrap<br>ClassLoader | <span style="font-weight:bold">변경 되지 않음</span> | <span style="font-size:14px; font-weight:bold">rt.jar 등이 없어짐에 따라 로딩할 수 있는 클래스의 범위가 전반적으로 축소</span> |
| Extension<br>ClassLoader | <span style="font-weight:bold">Platform<br>ClassLoader</span> | <span style="font-size:14px; font-weight:bold">1. jre/lib/ext, java.ext.dirs를 지원하지 않음 Java SE의 모든 클래스와 Java SE에는 없지만 JCP에 의해 표준화 된 모듈 내의 클래스를 볼 수 있으며, Java 8에 비해 볼 수 있는 범위가 확장됨</span><br><span style="font-size:14px; font-weight:bold">2. URLClassLoader가 아닌 BuiltinClassLoader를 상속받아 ClassLoders 클래스의 내부 static 클래스로 구현됨</span> |
| Application<br>ClassLoader |  <span style="font-weight:bold">System<br>ClassLoader</span> | <span style="font-size:14px; font-weight:bold">URLClassLoader가 아닌 BuiltinClassLoader를 상속받아 ClassLoders 클래스의 내부 static 클래스로 구현됨</span> |
  

<div class='next-line'></div>  
  
# 4. Class Loader의 원칙
---
클래스 로더의 원칙은 크게 4가지로 나눌 수 있다.
1. <span class="h-text-p">가시성 원칙</span>
2. <span class="h-text-p">유일성 원칙</span>
3. <span class="h-text-p">위임 계층</span>
4. <span class="h-text-p">언로드(Unload) 불가</span>
  
## 4-1. 가시성 원칙
3번 항목에서 설명했듯이 각 클래스 로더들은 <span class="h-text-y">계층구조</span>를 가지고 하위 클래스는 상위 클래스를 상속한다.  
  
이러한 구조에서 **클래스 로더를 요청받아 클래스 로더 캐시를 확인**할 때, 
**자식 클래스 로더는 부모 클래스 로더가 로드한 것을 볼 수 있지만 부모 클래스 로더는 자식 클래스 로더가 로드한 클래스를 볼 수 없다는 원칙이다.**  
 
## 4-2. 유일성 원칙
부모가 로드한 클래스를 **자식 클래스 로더가 다시 로드하지 않아야**하며 이미 **로드한 클래스를 다시 로드해서는 안된다**는 원칙이다.  
이 원칙을 통해 클래스가 정확히 한 번만 로드할 수 있다.

## 4-3. 위임 계층
<p style="border: solid black 1px" align="center"><img src="/assets/images/JVM/class-loader/delegation-principle.png"></p>  

위에서 설명한 가시성 **원칙과 유일성 원칙을 충족하기 위해 JVM은 클래스 로딩 요청을 아래와 같은 순서로 처리**한다.  
1. <span class="h-text-p">클래스 로더 캐시</span>
2. <span class="h-text-p">상위 클래스 로더</span>
3. <span class="h-text-p">자기 자신</span>  

- 이전에 로드된 클래스인지 <span class="h-text-y">클래스 로더 캐시를 확인</span>하고 없다면 <span class="h-text-y">상위 클래스 로더에게 요청을 위임</span>한다.  
Application Loader는 Extension Loader에게 요청을 위임하고 Extension은 Bootstrap Loader에게 요청을 위임한다.
- 요청을 위임받은 **Bootstrap Loader는 
<span class="tooltip h-text-y">Java 버전에 맞게<sup>❓</sup>
  <span class="tooltip-text">
  Java 8버전 : rt.jar에 담긴 jdk 클래스 파일을 로딩  
  Java 9버전 이후 : ClassLoader 내 최상위 클래스들만 로딩
  </span>
</span> 요청한 클래스를 찾는다.**  
만약 클래스가 있다면 클래스를 반환하고 아니라면 **다시 Extension Loader에게 요청을 위임**한다. 
- Extension Loader도 마찬가지로
<span class="tooltip h-text-y">Java 버전에 맞게<sup>❓</sup>
  <span class="tooltip-text">
  Java 8버전 : jre/lib/ext, java.ext.dirs  
  Java 9버전 이후 : Java SE 모든 클래스, JCP에 의해 표준화된 모듈 내의 클래스
  </span>
</span> 클래스를 찾고 있다면 클래스를 반환 아니라면 요청을 Application Loader에게 위임한다.
- Application Loader도 동작은 마찬가지로 
<span class="tooltip h-text-y">Classpath<sup>💬</sup>
  <span class="tooltip-text">
  클래스 파일을 찾는 데 기준이 되는 파일 경로를 말한다.<br>시스템의 모든 폴더를 JVM이 검사하도록 하는 것은 비현실적이므로 JVM에 찾아볼 파일 경로를 제공해야 한다.
  </span>
</span>
에서 요청한 클래스를 찾은 뒤 있다면 반환
없다면 **ClassNotFoundException이** 발생한다.  
  
## 4-4. 언로드 불가
언로드 불가 원칙은 말그대로 **이미 로드한 클래스를 언로드(Unload)할 수 없다는 원칙**이다.


<div class='next-line'></div>  
<div class='next-line'></div>  




# 5. Class Loader의 동작 순서
---
클래스 로더 시스템의 크게 3가지의 순서로 실행된다.
1. <span class="h-text-p">로딩</span>
2. <span class="h-text-p">링크</span>
3. <span class="h-text-p">초기화</span>  

<p style="border: solid black 1px" align="center"><img src="/assets/images/JVM/class-loader/class-loader-system.png"></p>  

## 5-1. 로딩
로딩 단계는 **.class 파일을 읽어서 바이너리 코드로 만들고** 이를 메모리의 **메서드 영역(Method Area)에 저장**하는 과정을 말한다.  
<span style="font-size:12px">[JVM의 메모리 구조](../JVM-memory)</span>
- Class, Enum, Inteface를 구분해서 저장한다.
- 로딩이 끝나면 해당 클래스 타입의 객체를 생성해 **메모리의 Heap영역에 저장**한다.
- <span class="h-text-p">동적 로딩</span> : 본문의 처음에 언급했듯이 런타임 중 필요할 때마다 동적으로 메모리를 할당해서 효율적으로 메모리를 관리한다.
- 동작 순서는 이 글의 [위임 계층](#4-3-위임-계층)에서 설명했으므로 다시 확인하자.
  
## 5-2. 링크
링크 단계는 로드된 Class, Interface 등을 **검증, 준비, 해석**하는 과정을 말한다.
- Verify(검증) - Prepare(준비) - Resolve(해석) 의 과정을 거친다.
- <span class="h-text-p">Verify</span>
  - .class 파일이 자바 언어 명세서에 따라 코드를 제대로 잘 작성했는지, JVM 규격에 따라 검증된 컴파일러에서 파일이 생성되는지 등을 확인해 **파일의 정확성을 보장**한다.
  - 만약 검증이 실패한다면 ***java.lang.VerifyError***을 발생시켜 유효하지 않은 클래스 파일의 변경을 미연에 방지할 수 있다.
- <span class="h-text-p">Prepare</span>
  - 매모리를 준비하는 단계
  - Class 또는 Interface에 필요한 정적 필드를 만들고 해당 필드를 기본값으로 초기화 하는 작업을 수행한다.
  ```java
  class ExampleClass {
      private static int a = 10;
  }
  ```  
  위와 같은 코드가 있다면 int형 정적 변수인 a에 4byte의 메모리 공간을 확보하고 기본값인 0으로 초기화한다.
  - 위의 작업 수행중 메모리가 부족하다면 **java.lang.OutOfMemoryError**이 발생한다.
- <span class="h-text-p">Resolve</span>
  - 해석단계는 런타임 상수 풀(run-time constant pool)에 있는 **심볼림 참조를 직접 참조로 대체하는 과정**을 말한다.
  - **JVM의 명령어**(anewarray, checkcast, getfield, getstatic, instanceof, invokedynamic, invokeinterface
  , invokespecial, invokestatic, invokevirtual, ldc, ldc_w, multianewarray, new, putfield 및 putstatic)는 심볼릭 참조를 사용하는데
  이러한 명령어를 해석하려면 심볼릭 참조를 해석해야한다.  
  ```java
  class ConstantPoolExample{
      public void example(){
         System.out.println("constant pool");
      }
  }
  ```  
  이 코드를 디어셈블러 명령어인 javap -v 이름.class 를 통해 바이트코드로 분석하면 아래와 같다.  
  **#n은 상수풀의 n번 인덱스로 접근하는 바이트코드이다.**  
  ```java
  #1 = Methodref          #6.#14         // java/lang/Object."<init>":()V
  #2 = Fieldref           #15.#16        // java/lang/System.out:Ljava/io/PrintStream;
  #3 = String             #17            // constant pool
  #4 = Methodref          #18.#19        // java/io/PrintStream.println:(Ljava/lang/String;)V
  #5 = Class              #20            // ConstantPoolExample
  #6 = Class              #21            // java/lang/Object
  #7 = Utf8               <init>
  #8 = Utf8               ()V
  #9 = Utf8               Code
  #10 = Utf8               LineNumberTable
  #11 = Utf8               example
  #12 = Utf8               SourceFile
  #13 = Utf8               constant.java
  #14 = NameAndType        #7:#8          // "<init>":()V
  #15 = Class              #22            // java/lang/System
  #16 = NameAndType        #23:#24        // out:Ljava/io/PrintStream;
  #17 = Utf8               constant pool
  #18 = Class              #25            // java/io/PrintStream
  #19 = NameAndType        #26:#27        // println:(Ljava/lang/String;)V
  #20 = Utf8               ConstantPoolExample
  #21 = Utf8               java/lang/Object
  #22 = Utf8               java/lang/System
  #23 = Utf8               out
  #24 = Utf8               Ljava/io/PrintStream;
  #25 = Utf8               java/io/PrintStream
  #26 = Utf8               println
  #27 = Utf8               (Ljava/lang/String;)V
  ```  
  위의 상수풀의 심볼릭 참조를 직접참조로 대체하는 과정을 거친다.
  
## 5-3. 초기화
초기화 단계는 로드된 각 **Class나 Interface의 초기화 로직이 실행되는 과정**을 말한다.
- 정적 변수는 코드에 명시된 원래 값이 할당된다.
- 초기화 블록 (static { })이 실행된다.
- 클래스 계층구조에서 부모에서 자식까지 한 줄씩 실행된다.
<div class='next-line'></div>  
<details>
<summary style="font-weight: bold">참고한 내용의 출처</summary>
<div markdown="1">       
[IBM 클래스 로딩](https://www.ibm.com/docs/ko/sdk-java-technology/7?topic=uc-class-loading-1)
[블로그](https://velog.io/@leede418/Java-ClassLoader-Class-Loading-Process)
[Oracle 로딩, 링킹, 초기화](https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-5.html)
[Constant Pool](https://jiwondev.tistory.com/114)
[블로그](https://blog.hexabrain.net/397)
</div>
</details>  