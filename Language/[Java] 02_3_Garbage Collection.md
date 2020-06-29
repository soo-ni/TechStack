# Garbage Collection

Java에서는 메모리를 명시적으로 해제하지 않기 때문에 Garbage Collector가 더 이상 필요없는 객체를 찾아 메모리를 해제해주는 역할을 한다.

'Most allocated objects die young'이라는  'Weak generational hypothesis (약한 세대 가설)'에 따라 JVM의 Heap은 **Young Generation**과 **Old Generation**로 구조를 나누었다.

* Young Generation
  * 새로 생성된 객체가 할당되는 영역으로, 가득 차게되면 **Minor GC**가 발생하게 된다.
  * 더 오래 참조될 것 같은 객체는 Old Generation 영역으로 이동하게 된다.
* Old Generation
  * Young Generation에서 살아남은 객체가 복사되는 영역으로, 가득 차게 되면 **Major GC** (Full GC)가 발생하게 된다.

#### Stop-the-World

> Garbage Collection을 실행하기 위해 JVM이 애플리케이션 실행을 멈추는 것으로 GC를 실행하는 스레드를 제외한 모든 스레드는 작업을 멈춘다.



## 동작 과정

1. Young Generation 영역의 Eden에 새로운 객체가 생성된다.
2. Eden이 가득 차는 경우 살아남은 객체는 Survivor0으로 이동하게 된다. (Survivor0 혹은 Suvivor1 중 비어있는 영역으로)
3. 다시 Eden의 GC가 발생하는 경우 Survivor0의 객체는 Survivor1로 이동하게 된다. (Survivor0 혹은 Suvivor1 중 비어있는 영역으로)
4. 마지막으로 이 과정이 반복되는 경우에도 객체가 살아있는 상태라면 Survivor에서 Old Generation 영역으로 이동한다.



## Minor GC vs. Major GC

Minor GC는 Eden의 메모리 공간이 가득 차게 되면 발생한다. Minor GC는 빠르고 효율적으로 Young Generation 크기에 다르지만 보통 1초 미만이다. 

Major GC는 모든 스레드의 실행을 멈추고 Heap영역의 정리를 위해 살아있는 객체를 표시하고 사용하지 않는 객체는 제거한다. 이 때, Minor GC에 비해 10배 이상의 시간을 사용하게 된다.



## Garbage Collection의 종류

#### Serail GC (-XX:+UseSerialGC)

#### Parallel GC (-XX:+UseParallelGC)

#### Parallel Old GC(-XX:+UseParallelOldGC)

#### CMS GC (-XX:+UseConcMarkSweepGC)

#### G1 GC







참고

* https://d2.naver.com/helloworld/1329
* https://mirinae312.github.io/develop/2018/06/04/jvm_gc.html
* https://www.holaxprogramming.com/2013/07/20/java-jvm-gc/
* [https://johngrib.github.io/wiki/jvm-memory/#gc-%EC%A2%85%EB%A5%98-%EC%9A%94%EC%95%BD](https://johngrib.github.io/wiki/jvm-memory/#gc-종류-요약)