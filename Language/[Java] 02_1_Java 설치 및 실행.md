# Java 설치 및 실행

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

<img src='https://t1.daumcdn.net/cfile/tistory/991D064B5AE999D512' width=600 align='center'>

<p align='center'>[출처:  https://aljjabaegi.tistory.com/387]</p>



1. Java Source Code (**.java**)  파일 생성한다.

2. **Build** 작업 실행 시 Java 컴파일러의 **javac**라는 명렁어를 사용한다.

3. 컴퓨터가 읽을 수 없는 반기계어인 Byte Code (**.class**) 파일 생성한다.

4. Byte Code는 **클래스 로더 (Class Loader)**에 의해서 JVM 내로 로드한 후, 
   실행 엔진에 의해 기계어로 해석돼 **Runtime Data Area(JVM 메모리)**에 배치한다.

   

> **TIP** 
>
> 실행엔진 (Execution Engine)
> : 인터프리터 방식으로 실행하다가 JIT Compiler 방식으로 실행한다.
>
> <img src='https://t1.daumcdn.net/cfile/tistory/99EE83415AE977BE34' width=350>
>
> <p align='center'>[출처:  https://aljjabaegi.tistory.com/387]</p>
>
> * Interpreter: 바이트 코드를 한줄씩 읽어 실행한다.
> * JIT(Just-In-Time) Compiler: 바이트 코드 전체를 컴파일 후 해석된 코드(NativeCode)를 캐시에 보관해 해당 코드를 직접 실행한다.

