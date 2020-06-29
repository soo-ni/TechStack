# JVM Structure

## JVM

> JVM이랑 자바 가상 머신으로써 자바와 운영체제 사이에서 중계자 역할을 하며 운영체제에 독립적으로 실행될 수 있도록 한다. 또한, Garbage Collector가 있어 메모리 관리를 자동으로 해준다.



### **# Runtime Data Areas**

#### 1. Method(Class, Static) Area

- 프로그램 종료 전까지 런타임 시 생성된 모든 스레드가 공유하는 영역으로 class, method 정보, static 변수 등의 Byte Code 등을 보관한다.

- ~~프로그램 실행 시 Method area보다 메모리가 큰 경우 OutOfMemory가 발생한다.~~ (?)

  

#### 2. **Stack Area**

- Method area가 모든 스레드가 공유하는 영역이라면 Stack area는 프로세스 내부에 생성된 스레드마다 생성된다. 

- 클래스 내 메소드에서 사용되는 매개변수, 지역변수, 리턴값 등의 정보들이 저장된다.

- Exception 발생 시 printStackTrace() 메소드를 호출해 해당 내용을 확인할 수 있다.

- LIFO 방식으로 메소드 실행 시 저장 후 실행이 완료되면 제거된다.

  

#### 3. **Heap Area**

<img src='https://t1.daumcdn.net/cfile/tistory/995811375C76841D02' width=450 align='center'>

<p align='center'>[출처:  https://swiftymind.tistory.com/112]</p>

- New 명령어를 통해 생성된 **객체가 저장**되는 공간으로, Heap area가 가득 차게 되면 **Garbage Collection**이 수행된다.
- Young Generation

  - Eden
    - 객체가 최초로 heap에 할당되는 장소이다. 
    - 영역이 가득 찼다면 Object의 참조 여부를 파악 후 Live Object는 Survivor 영역으로 넘어가고, 참조가 사라진 Garbage Object이면 남겨 놓는다. 모든 Live Object가 Survivor 영역으로 넘어간다면 Eden 영역을 모두 청소한다.
  - Survivor (Survivor0, Suvivor1)
    - Eden 영역에 살아 남은 Object들이 잠시 머무르는 곳으로, Live Object들은 하나의 Survivor 영역만 사용하기 되며 이러한 과정을 Minor GC라 한다.
- Old Generation
  - Survivor 영역에서 살아남아 오랫동안 참조 되었고 앞으로 사용 확률이 높은 Object들을 저장하는 영역이다. 이러한 Promotion 과정 중, Old Generation 메모리가 충분하지 않으면 GC가 발생하는데 이를 **Major GC**라고 한다.
- ~~Permanent Generation~~
  - ~~클래스 로더에 의해 로드되는 클래스, 메소드 등에 대한 정보가 저장되는 영역으로 JVM에 의해 사용된다. 리플렉션을 사용하여 동적으로 클래스가 로딩되는 경우 사용된다. 내부적으로 리플렉션 기능을 자주 사용하는 Spring Framework를 이용할 경우 이 영역에 대한 고려가 필요하다.~~

> **TIP**
>
> Java 8 이전에는 Permanent Generation을 사용했지만, Java 8 이후에는 Metasapce 사용한다. 이 때, Metaspace는 heap영역이 아닌 Native Memory 영역으로 포함된다.
>
> ```
> Java 7 Hotspot JVM 구조
> <---- Java Heap ------> < Permanent Heap > < Native Memory  >
> +------+---+---+-------+------------------+--------+--------+
> | Eden | S | S |  Old  |     Permanent    | C Heap | Thread |
> |      | 0 | 1 |       |                  |        | Stack  |
> +------+---+---+-------+------------------+--------+--------+
> ```
>
> ```
> Java 8 Hotspot JVM 구조
> <---- Java Heap ------> <--------- Native Memory ---------->
> +------+---+---+-------+------------------+--------+--------+
> | Eden | S | S |  Old  |     Metaspace    | C Heap | Thread |
> |      | 0 | 1 |       |                  |        | Stack  |
> +------+---+---+-------+------------------+--------+--------+
> 
> * Java Heap: JVM이 관리하는 영역
> 
> * Native Memory: OS에서 관리하는 영역
> ```
>
> <p align='center'>[출처: https://johngrib.github.io/wiki/jvm-memory/]</p>



#### 4. **PC Register**

- 각 스레드 별로 하나 씩 존재해 현재 수행 중인 JVM instruction의 주소를 저장한다.

- Native method를 수행하는 경우 PC Register는 undefined 상태가 되며, JVM을 거치지 않고 API를 통해 바로 수행하게 된다.

  

#### 5. **Native Method Stack**

- Java 외의 언어로 작성된 Native Code를 위한 스택으로, JNI(Java Native Interface)를 통해 호출되는 C/C++ 등의 코드를 수행하기 위한 스택이다.
- Native Method를 호출하기 되면 새로운 Stack Frame을 생성해 푸쉬해 JNI를 이용해 JVM 내부에 영향을 주지 않는다.





참고

* https://aljjabaegi.tistory.com/387
* https://limkydev.tistory.com/51
* https://hoonmaro.tistory.com/19
* 김홍래, 'BACK TO THE BASIC, JAVA 핵심 요약 노트', 한빛미디어(2013)
* https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-2.html
* [https://medium.com/@lazysoul/jvm-%EC%9D%B4%EB%9E%80-c142b01571f2](https://medium.com/@lazysoul/jvm-이란-c142b01571f2)
* https://swiftymind.tistory.com/112
* https://d2.naver.com/helloworld/2675
* https://johngrib.github.io/wiki/jvm-memory/

