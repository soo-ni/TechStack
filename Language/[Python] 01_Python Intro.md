# Python이란 무엇인가?

## 파이썬이란?

<img src='https://wikidocs.net/images/page/5/pahkey_KRRKrp.png'>

Python은 1990년 암스테르담의 귀도 반 로섬(Guido Van Rossum)이 개발한 인터프리터 언어이다.

파이썬 프로그램은 공동 작업과 유지 보수가 매우 쉽고 편하다. 그 때문에 이미 다른 언어로 작성된 많은 프로그램과 모듈이 파이썬으로 재구성되고 있다. 

## 파이썬의 특징

>- [파이썬은 인간다운 언어이다](https://wikidocs.net/6#_1)
>  - 파이썬은 사람이 생각하는 방식을 그대로 표현할 수 있는 언어이다.
>- [파이썬은 문법이 쉬워 빠르게 배울 수 있다](https://wikidocs.net/6#_2)
>- [파이썬은 무료이지만 강력하다](https://wikidocs.net/6#_3)
>  - 파이썬은 **오픈소스**이다.
>  - 프로그램의 전반적인 뼈대는 파이썬으로 만들고, 빠른 실행 속도가 필요한 부분은 C로 만들어서 파이썬 프로그램 안에 포함시킨다. 파이썬 라이브러리 중에는 순수 파이썬만으로 제작된 것도 많지만 C로 만든 것도 많다. **C로 만든 것은 대부분 속도가 빠르다.**
>- [파이썬은 간결하다](https://wikidocs.net/6#_4)
>  - 파이썬은 가장 좋은 방법 1가지만 사용하는 것을 선호한다. 또한, 다른 사람이 작업한 소스 코드도 한눈에 들어와 이해하기 쉽기 때문에 공동 작업과 유지 보수가 아주 쉽고 편하다.
>  - 파이썬 프로그램은 줄을 맞추지 않으면 실행되지 않는다.
>- [파이썬은 프로그래밍을 즐기게 해준다](https://wikidocs.net/6#_5)
>  - 파이썬은 다른 것에 신경 쓸 필요 없이 내가 하고자 하는 부분에만 집중할 수 있게 해준다.
>- [파이썬은 개발 속도가 빠르다](https://wikidocs.net/6#_6)
>  - ***"Life is too short, You need python." (인생은 너무 짧으니 파이썬이 필요해.)***

## 파이썬으로 할 수 있는 일

어떤 프로그래밍 언어가 어떤 일에 효율적인지를 안다는 것은 프로그래머의 생산성을 크게 높일 수 있는 힘이 된다. 

파이썬으로 할 수 있는 일

> - [시스템 유틸리티 제작](https://wikidocs.net/7#_2)
>    - 파이썬은 운영체제(윈도우, 리눅스 등)의 **시스템 명령어를 사용할 수 있는 각종 도구**를 갖추고 있기 때문에 이를 바탕으로 갖가지 시스템 유틸리티를 만드는 데 유리하다.
>    - *유틸리티: 컴퓨터 사용에 도움을 주는 여러 SW*
> - [GUI 프로그래밍](https://wikidocs.net/7#gui) 
>    - GUI(Graphic User Interface) 프로그래밍을 위한 도구들이 잘 갖추어져 있어 GUI 프로그램을 만들기 쉽다.
>    - *ex. Tkinter(티케이인터)*
> - [C/C++와의 결합](https://wikidocs.net/7#cc)
>    - 파이썬은 접착(glue) 언어라고도 부른다.
>    - `C`나 `C++`로 만든 프로그램을 파이썬에서 사용할 수 있으며, 파이썬으로 만든 프로그램 역시 `C`나 `C++`에서 사용할 수 있다.
> - [웹 프로그래밍](https://wikidocs.net/7#_3)
>    - 파이썬은 웹 프로그램을 만들기에 매우 적합한 도구이며, 실제로 파이썬으로 제작한 웹 사이트는 셀 수 없을 정도로 많다.
> - [수치 연산 프로그래밍](https://wikidocs.net/7#_4) 
>    - **사실 파이썬은 수치 연산 프로그래밍에 적합한 언어는 아니다.** 수치가 복잡하고 연산이 많다면 C 같은 언어로 하는 것이 더 빠르기 때문이다. 
>    - 하지만 파이썬은 NumPy라는 수치 연산 모듈을 제공한다. 
>    - ***NumPy module*: 이 모듈은 C로 작성했기 때문에 파이썬에서도 수치 연산을 빠르게 할 수 있다.**
> - [데이터베이스 프로그래밍](https://wikidocs.net/7#_5)
>    - 파이썬은 사이베이스(Sybase), 인포믹스(Infomix), 오라클(Oracle), 마이에스큐엘(MySQL), 포스트그레스큐엘(PostgreSQL) 등의 데이터베이스에 접근하기 위한 도구를 제공한다.
>    - 또한, 피클(pickle) module을 사용해 자료를 변형 없이 그대로 파일에 저장하고 불러오는 일을 맡아 한다.
> - [데이터 분석, 사물 인터넷](https://wikidocs.net/7#_6)
>    - 파이썬으로 만든 판다스(Pandas) 모듈을 사용하면 데이터 분석을 더 쉽고 효과적으로 할 수 있다.
>    - 파이썬은 이 라즈베리파이를 제어하는 도구로 사용된다. 

파이썬으로 할 수 없는 일

> * [시스템과 밀접한 프로그래밍 영역](https://wikidocs.net/7#_8)
>   * **대단히 빠른 속도를 요구**하거나 **하드웨어를 직접 건드려야 하는 프로그램**에는 어울리지 않는다.
>   * 리눅스 같은 **운영체제**, 엄청난 횟수의 **반복과 연산이 필요한 프로그램** 또는 **데이터 압축 알고리즘 개발 프로그램** 등을 만드는 것은 어렵다. 
> * [모바일 프로그래밍](https://wikidocs.net/7#_9)
>   * 파이썬으로 안드로이드 및 아이폰 앱(App)을 개발하는 것은 아직 어렵다. 

## 파이썬 둘러보기

### 파이썬 기초 실습

```
Python 3.7.3 (v3.7.3:ef4ec6ed12, Mar 25 2019, 21:26:53) [MSC v.1916 32 bit (Intel)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> # prompt
```

* 파이썬 대화형 인터프리터(Python shell): 사용자가 입력한 소스 코드를 실행하는 환경

### 파이썬 기초 문법

#### 출력

```
# Print
>>> a = "Python"
>>> print(a)
Python
>>> a
'Python'
```

#### 조건문

```
# if
>>> a=3
>>> if a>1:
...    print("a is greater than 1")
...
a is greater than 1
```

#### 반복문

```
# for
>>> for a in [1, 2, 3]:
...    print(a)
...
1
2
3

# while
>>> i=0
>>> while i<3:
...    i=i+1
...    print(i)
...
1
2
3
```

#### 함수

```
>>> def add(a, b):
...    return a+b
...
>>> add(3,4)
7
```

## 파이썬과 에디터

파이썬 IDLE(Integrated Development and Learning Environment)은 파이썬 프로그램 작성을 도와주는 통합개발 환경이다. 

> - [IDLE로 파이썬 프로그램 작성하기](https://wikidocs.net/17684#idle)
> - [명령 프롬프트 창에서 파이썬 프로그램 실행하기](https://wikidocs.net/17684#_1)
> - 추천 에디터
>   - [비주얼 스튜디오 코드](https://wikidocs.net/17684#_3)
>   - [파이참](https://wikidocs.net/17684#_4)



### 참고

* https://wikidocs.net/book/1