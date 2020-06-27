# Java

## Java?

> Java는 **객체 지향 프로그래밍 언어**로  1995년 Sun Microsystems의 James Gosling에 의해 처음 발표되었다. 초기 Java는 가전 제품에 탑재할 프로그래밍 언어로 개발되었지만, 현재 웹 어플리케이션 및 모바일 기기용 소프트웨어 개발에 널리 사용된다.

1. Java 연혁
   - 1990년대 초반: Sun의 그린 프로젝트로 'Oak' 언어로 개발
   - 1995년: Java 1.0 release
   - 1990년대 후반: Web Client의 Applet 기술
   - 1990년대 후반~현재: Web Server, Application Server 프로그램 등 구현
   - 2008년: Android Platform
2. Java Version
   - JDK 1.0
   - JDK 1.5 => Java 8.0 (JavaSE)
   - JDK 8 => java 8.0
   - JDK 11
3. Java Platform
   - Standard Edition (Java SE)
   - Enterprise Edition (Java EE)
   - Micro Edition (Java ME)



## Java의 특징

> Java는 <u>객체 지향</u>, <u>플랫폼 독립적</u>, <u>간결함</u>, <u>분산 프로그래밍 지원</u>, <u>멀티스레드</u>의 특징을 지니고 있어
> "Write Once, Run Everywhere" 라는 슬로건을 가지고 있다.



1. 객체 지향

   > **객체 지향 프로그래밍(Object-Oriented Programming, OOP)**이란, 데이터와 이를 처리하는 루틴들을 하나의 객체(Object)로 바라보는 컴퓨터 프로그래밍의 패러다임이다. 소프트웨어 시스템의 기본 구성 단위인 객체별로 코드를 작성하고 객체들을 조합하여 전체 프로그램을 완성한다.
   >
   > 코드의 재사용성, 유지보수성, 확장성을 증가시킨다.

   객체 지향의 특징

   * 추상화 (Abstraction)
     
   * 객체의 주요 특징을 추출하는 과정
     
   * 캡슐화 (Encapsulation)
     * 용어 그대로 관련된 데이터와 알고리즘(코드)가 하나의 묶음으로 정리하는 것을 의미한다. 객체 지향에서는 이 캡슐을 클래스라고 부른다.
     * 내부의 데이터를 캡슐로 보호하는 **정보 은닉(information hiding)** 역할을 한다. 이를 통해 다른 부분에 영향을 미치지 않고 쉽게 변경될 수 있음을 의미한다.

   * 다형성 (Polymorphism)
     * 객체가 취하는 동작이 상황에 따라서 달라지는 것을 의미한다. 서로 다른 타입에 속하는 객체들이 같은 이름의 멤버 함수에 응답해 서로 다른 동작을 보여줄 수 있다.
     * 부모 클래스의 인스턴스를 이용하여 자녀 타입의 클래스를 다룰 수 있다.
     * 메소드 오버 로딩을 통해 동일명의 메소드를 이용해 다양한 형태의 파라미터를 다룰 수 있다.

   * 상속 (Inheritance)

     * 기존의 코드를 **재사용**하기 위한 기법으로 부모 클래스에 정의된 메소드와 필드를 자녀가 물려받아 사용할 수 있다.
     * 상속을 받게 되면 부모 클래스의 메소드와 필드를 자식 클래스에서 따로 정의하지 않아도 사용 가능하다. 따라서, 객체간 계층적 분류 및 관리가 용이하며 소스가 간결해진다.

     > **TIP 1**
     >
     > 부모 클래스 (Super class) / 자식 클래스 (Sub class)
     > 자바는 다중 상속을 지원하지 않기 때문에 이를 해결하기 위해 interface를 이용
     >
     > **TIP 2**
     >
     > Overriding
     > : 부모 클래스로부터 상속받은 메소드를 <u>자식 클래스에서 재정의</u>해 사용하는 기술로 <u>매개변수, 리턴타입 변경이 불가</u>하다.
     >
     > Overloading
     > : 같은 이름의 함수를 여러 개 정의하고, <u>매개 변수의 유형과 개수를 다르게 하여</u> 다양한 호출에 응답할 수 있다.

   

2. 플랫폼 독립적

   > Java 컴파일러는 컴퓨터 구조에 중립적인 기계어로 번역되어 **JVM(Java Virtual Machine)**에 의해서 실행된다. 이러한 기계어는 특정 컴퓨터 구조에 독립적인 중간 코드이므로 하드웨어 구조, 운영체제, 윈도우 시스템에 독립적이고 이식성을 보장할 수 있다.



3. 간결함

   >Java는 C, C++의 복잡성과 메모리 관리의 어려움을 해결해 쉽게 접근할 수 있도록 설계됐다.
   >
   >예를 들어, 포인터 연산을 제거하였으며, 유지 보수를 힘들게 했던 연산자 중복 정의, 다중 상속 등의 복잡한 기능을 삭제했다. 그리고, C++에서 제공하지 않는 자동 메모리 관리 기능, 멀티 스레드, 객체 지향 방법으로 제작된 많은 양의 라이브러리를 제공한다.

   

4. 분산 프로그래밍 지원

   > Java는 네트워크상에서 동작되도록 설계된 언어로 TCP/IP, HTTP, FTP와 같은 프로토콜을 처리할 수 있는 라이브러리를 가지고 있어 네트워크 관련 프로그램을 개발할 수 있고, 원격 접속을 위한 다양한 기술을 가지고 있다.

   

5. 멀티 스레드

   > Java는 Thread API를 제공함으로써 JVM의 스케줄링을 통해 운영체제에 독립적으로 많은 작업을 동시에 할 수 있다.



## Java 설치

1. JavaSE SDK 설치
   - JRE는 Java의 실행환경만 가지고 있으므로 컴파일러가 포함된 JDK를 다운받아야 한다.
2. C:\Program Files\Java\jdk_1.8.xxx
3. 시스템 환경 변수 설정
   - JAVA_HOME
     : C:\Program Files\Java\jdk_8.xxxx\bin;
4. IDE(Integrated Development Environment) 설치
   - Eclipse
   - NetBeans



## Java 실행



<img src='https://t1.daumcdn.net/cfile/tistory/2540294C5654207F26' width=500>

[출처: https://hoonmaro.tistory.com/19]

<img src='https://t1.daumcdn.net/cfile/tistory/991D064B5AE999D512' width=600>

[출처:  https://aljjabaegi.tistory.com/387]



> 1. Java Source Code (**.java**)  파일 생성한다.
> 2. **Build** 작업 실행 시 Java 컴파일러의 **javac**라는 명렁어를 사용한다.
> 3. 컴퓨터가 읽을 수 없는 반기계어인 Byte Code (**.class**) 파일 생성한다.
> 4. Byte Code는 **클래스 로더 (Class Loader)**에 의해서 JVM 내로 로드한 후, 
>    실행 엔진에 의해 기계어로 해석돼 **Runtime Data Area(JVM 메모리)**에 배치한다.
>
> > **TIP** 
> >
> > 실행엔진 (Execution Engine)
> > : 인터프리터 방식으로 실행하다가 JIT Compiler 방식으로 실행한다.
> >
> > <img src='https://t1.daumcdn.net/cfile/tistory/99EE83415AE977BE34' width=350>
> >
> > [출처:  https://aljjabaegi.tistory.com/387]
> >
> > * Interpreter: 바이트 코드를 한줄씩 읽어 실행한다.
> > * JIT(Just-In-Time) Compiler: 바이트 코드 전체를 컴파일 후 해석된 코드(NativeCode)를 캐시에 보관해 해당 코드를 직접 실행한다.



## JVM

> JVM이랑 자바 가상 머신으로써 자바와 운영체제 사이에서 중계자 역할을 하며 운영체제에 독립적으로 실행될 수 있도록 한다. 또한, Garbage Collector가 있어 메모리 관리를 자동으로 해준다.



**Runtime Data Areas**

> 1. **Method(Class, Static) Area**
>
>    - 프로그램 종료 전까지 런타임 시 생성된 모든 스레드가 공유하는 영역으로 class, method 정보, static 변수 등의 Byte Code 등을 보관한다.
>    - ~~프로그램 실행 시 Method area보다 메모리가 큰 경우 OutOfMemory가 발생한다.~~ (?)
>
> 2. **Stack Area**
>
>    - Method area가 모든 스레드가 공유하는 영역이라면 Stack area는 프로세스 내부에 생성된 스레드마다 생성된다. 
>    - 클래스 내 메소드에서 사용되는 매개변수, 지역변수, 리턴값 등의 정보들이 저장된다.
>    - Exception 발생 시 printStackTrace() 메소드를 호출해 해당 내용을 확인할 수 있다.
>    - LIFO 방식으로 메소드 실행 시 저장 후 실행이 완료되면 제거된다.
>
> 3. **Heap Area**
>
>    >* New 명령어를 통해 생성된 **객체가 저장**되는 공간으로, Heap area가 가득 차게 되면 **Garbage Collection**이 수행된다.
>    >
>    >1. ~~Permanent Generation~~
>    >   ~~: 클래스 로더에 의해 로드되는 클래스, 메소드 등에 대한 정보가 저장되는 영역으로 JVM에 의해 사용된다. 리플렉션을 사용하여 동적으로 클래스가 로딩되는 경우 사용된다. 내부적으로 리플렉션 기능을 자주 사용하는 Spring Framework를 이용할 경우 이 영역에 대한 고려가 필요하다.~~
>    >
>    >   > **TIP**
>    >   >
>    >   > Java 8 이전에는 Permanent Generation을 사용했지만, Java 8 이후에는 Metasapce 사용한다. 이 때, Metaspace는 none heap이다.
>    >
>    >2. Young Generation
>    >
>    >   - Eden
>    >     - 객체가 최초로 heap에 할당되는 장소이다. 
>    >     - 영역이 가득 찼다면 object의 
>    >   - From
>    >   - To
>    >
>    >3. Old Generation
>
> 4. **PC Register**
>
>    - 각 스레드 별로 하나 씩 존재해 현재 수행 중인 JVM instruction의 주소를 저장한다.
>    - Native method를 수행하는 경우 PC Register는 undefined 상태가 되며, JVM을 거치지 않고 API를 통해 바로 수행하게 된다.
>
> 5. **Native Method Stack**
>
>    - Java 외의 언어로 작성된 Native Code를 위한 스택으로, JNI(Java Native Interface)를 통해 호출되는 C/C++ 등의 코드를 수행하기 위한 스택이다.
>    - Native Method를 호출하기 되면 새로운 Stack Frame을 생성해 푸쉬해 JNI를 이용해 JVM 내부에 영향을 주지 않는다.



---

참고

* https://aljjabaegi.tistory.com/387
* https://limkydev.tistory.com/51
* https://hoonmaro.tistory.com/19
* 김홍래, 'BACK TO THE BASIC, JAVA 핵심 요약 노트', 한빛미디어(2013)
* https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-2.html
* [https://medium.com/@lazysoul/jvm-%EC%9D%B4%EB%9E%80-c142b01571f2](https://medium.com/@lazysoul/jvm-이란-c142b01571f2)
* https://swiftymind.tistory.com/112

