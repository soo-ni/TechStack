# 프로그램의 입력과 출력

> 입출력은 **프로그래밍 설계**와 관련이 있다. 
>
> 프로그래머는 프로그램을 만들기 전에 어떤 식으로 동작하게 할 것인지 설계 부터 하게 되는데 그때 가장 중요한 부분이 바로 입출력의 설계이다. 
>
> 특정 프로그램만이 사용하는 함수를 만들 것인지 아니면 모든 프로그램이 공통으로 사용하는 함수를 만들 것인지, 더 나아가 오픈 API로 공개하여 외부 프로그램들도 사용할 수 있게 만들 것인지 그 모든 것이 입출력과 관련이 있다.

## 함수

입력값을 가지고 어떤 일을 수행한 다음에 그 결과물을 출력한다.

### 함수를 사용하는 이유

1. **반복을 줄일 수 있다.** 반복되는 부분이 있을 경우 "반복적으로 사용되는 가치 있는 부분"을 한 뭉치로 묶어서 "어떤 입력값을 주었을 때 어떤 결괏값을 돌려준다"라는 식의 함수를 작성한다.
2. **프로그램 흐름을 일목요연하게 볼 수 있다.** 프로그램 흐름도 잘 파악할 수 있고 오류가 어디에서 나는지도 바로 알아차릴 수 있다.

### 파이썬 함수의 구조

```python
def 함수명(매개변수):
    <수행할 문장1>
    <수행할 문장2>
    ...
```

### 매개변수와 인수

* 매개변수(parameter): 함수에 입력으로 전달된 값을 받는 변수
* 인수(arguments): 함수를 호출할 때 전달하는 입력값

```python
def add(a, b):  # a, b는 매개변수
    return a+b

print(add(3, 4))  # 3, 4는 인수
```

### 입력값과 결괏값에 따른 함수의 형태

#### 1. 일반적인 함수

```
def 함수이름(매개변수):
    <수행할 문장>
    ...
    return 결과값
```

#### 2. 입력값이 없는 함수

```
def 함수이름():
    <수행할 문장>
    ...
    return 결과값
```

#### 3. 결괏값이 없는 함수

돌려받을 값을 a 변수에 대입하여 출력해 보면 **None**이 출력된다.

```
def 함수이름(매개변수):
    <수행할 문장>
    ...
```

#### 4. 입력값도 결괏값도 없는 함수

```
def 함수이름():
    <수행할 문장>
    ...
```

### 매개변수 지정하여 호출

```python
>>> def add(a, b):
...     return a+b
... 

>>> result = add(a=3, b=7)
>>> print(result)
10
```

### 입력값의 개수 미지정

```
def 함수이름(*매개변수): 
    <수행할 문장>
    ...
```

```python
# ex.1
>>> def add_many(*args): 
...     result = 0 
...     for i in args: 
...         result = result + i 
...     return result 
... 
>>>
```

```python
# ex.2
>>> def add_mul(choice, *args): 
...     if choice == "add": 
...         result = 0 
...         for i in args: 
...             result = result + i 
...     elif choice == "mul": 
...         result = 1 
...         for i in args: 
...             result = result * i 
...     return result 
... 
>>>
```

#### kwargs

매개변수 이름 앞에 `**`을 붙이면 매개변수 kwargs는 딕셔너리가 되고 모든 `key=value` 형태의 결괏값이 그 딕셔너리에 저장된다.

```python
>>> def print_kwargs(**kwargs):
...     print(kwargs)
...

>>> print_kwargs(a=1)
{'a': 1}
>>> print_kwargs(name='foo', age=3)
{'age': 3, 'name': 'foo'}
```

### 함수 결괏값은 하나

```python
>>> def add_and_mul(a,b): 
...     return a+b, a*b
>>> result = add_and_mul(3,4)
result = (7, 12)   # tuple로 출력됨

>>> result1, result2 = add_and_mul(3, 4)
result1, result2 = (7, 12)   # result1=7, result2=12
```

### 매개변수 초기화

초기화시키고 싶은 매개변수는 뒤쪽에 놓아야한다.

### 함수 안에서 함수 밖의 변수를 변경

1. return 사용

2. global 명령어 사용

   ```python
   a = 1 
   def vartest(): 
       global a 
       a = a+1
   
   vartest() 
   print(a)
   ```

### lambda

함수를 한줄로 간결하게 만들 때 사용하며, return 명령어가 없어도 결괏값을 돌려준다.

```
lambda 매개변수1, 매개변수2, ... : 매개변수를 이용한 표현식
```



## 사용자 입력과 출력

### 사용자 입력

1. input
   - input은 입력되는 모든 것을 문자열로 취급한다.
2. pormpt 띄워서 입력받기
   - input("질문 내용")

### print

* 큰따옴표(")로 둘러싸인 문자열은 + 연산과 동일

* 문자열 띄워쓰기는 콤마

* 한 줄에 결괏값 출력하기

  ```python
  >>> for i in range(10):
  ...     print(i, end=' ')
  ...
  0 1 2 3 4 5 6 7 8 9
  ```



## 파일 읽고 쓰기

### 파일 생성

open 함수는 "파일 이름"과 "파일 열기 모드"를 입력값으로 받고 결괏값으로 **파일 객체**를 돌려준다.

파일을 쓰기 모드로 열면 해당 파일이 이미 존재할 경우 원래 있던 내용이 모두 사라지고, 해당 파일이 존재하지 않으면 새로운 파일이 현재 디렉터리에 생성된다. 

```python
f = open("새파일.txt", 'w')
f.close()
```

| 파일열기모드 | 설명                                                       |
| :----------- | :--------------------------------------------------------- |
| r            | 읽기모드 - 파일을 읽기만 할 때 사용                        |
| w            | 쓰기모드 - 파일에 내용을 쓸 때 사용                        |
| a            | 추가모드 - 파일의 마지막에 새로운 내용을 추가 시킬 때 사용 |

### 파일 쓰기

```python
f = open("C:/doit/새파일.txt", 'w')
for i in range(1, 11):
    data = "%d번째 줄입니다.\n" % i
    f.write(data)
f.close()
```

### 프로그램의 외부에 저장된 파일을 읽는 방법

#### 1. readline() 함수

```python
# 첫 번째 줄 출력
f = open("C:/doit/새파일.txt", 'r')
line = f.readline()
print(line)
f.close()

# 모든 줄 출력
f = open("C:/doit/새파일.txt", 'r')
while True:
    line = f.readline()
    if not line: break
    print(line)
f.close()
```

#### 2. readlines() 함수

파일의 모든 줄을 읽어서 각각의 줄을 요소로 갖는 **리스트**로 돌려준다.

```python
f = open("C:/doit/새파일.txt", 'r')
lines = f.readlines()
for line in lines:
    print(line)
f.close()
```

#### 3. read 함수

파일의 내용 전체를 문자열로 돌려준다.

```python
f = open("C:/doit/새파일.txt", 'r')
data = f.read()
print(data)
f.close()
```

### 파일에 새로운 내용 추가

원래 있던 값을 유지하면서 단지 새로운 값만 추가하는 경우 파일을 추가 모드('a')로 연다.

```python
f = open("C:/doit/새파일.txt",'a')
for i in range(11, 20):
    data = "%d번째 줄입니다.\n" % i
    f.write(data)
f.close()
```

### with문과 사용

with 블록을 벗어나는 순간 열린 파일 객체 f가 자동으로 close되어 파일을 열고 닫는 것을 자동으로 처리한다.

```python
with open("foo.txt", "w") as f:
    f.write("Life is too short, you need python")
```

>**TIP**
>
>**sys module로 매개변수**
>
>```python
>import sys
>
>args = sys.argv[1:]
>for i in args:
>    print(i)
>```
>
>* prompt
>
>  * argv[0]: sys1.py
>  * argv[1]: aaa
>  * argv[2]: bbb
>  * argv[3]: ccc
>
>  ```
>  C:\doit>python sys1.py aaa bbb ccc
>  aaa
>  bbb
>  ccc
>  ```



### 참고

* https://wikidocs.net/book/1