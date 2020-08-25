# 제어문

## if문

프로그래밍에서 조건을 판단하여 해당 조건에 맞는 상황을 수행한다.

### 기본 구조

```python
if 조건문:
    수행할 문장1
    수행할 문장2
    ...
else:
    수행할 문장A
    수행할 문장B
    ...
```

### 들여쓰기

들여쓰기는 언제나 같은 너비로 해야 한다. 들여쓰기는 공백(`Spacebar`) 4개를 사용하는 것을 권장한다.

### 조건문

#### 비교 연산자

`<`, `>`, `==`, `!=`, `>=`, `<=`

#### and, or, not

#### x in s, x not in s

| in          | not in          |
| :---------- | :-------------- |
| x in 리스트 | x not in 리스트 |
| x in 튜플   | x not in 튜플   |
| x in 문자열 | x not in 문자열 |

#### elif

### 조건부 표현식

`조건문이 참인 경우` if `조건문` else `조건문이 거짓인 경우`

```python
message = "success" if score >= 60 else "failure"
```

## while문

조건문이 참인 동안 계속해서 while문 안의 내용을 반복적으로 수행한다.

break를 이용해 빠져나갈 수 있고, continue를 이용해 맨 처음으로 돌아갈 수 있다.

## for문

### for문의 기본 구조

리스트, 튜플, 문자열의 첫 번째 요소부터 마지막 요소까지 **차례로 변수에 대입**되어 문장이 수행된다.

```python
for 변수 in 리스트(또는 튜플, 문자열):
    수행할 문장1
    수행할 문장2
    ...
```

### 예제를 통한 이해

#### 전형적인 for문

```python
>>> test_list = ['one', 'two', 'three'] 
>>> for i in test_list:
...    print(i)
```

#### 다양한 for문

```python
>>> a = [(1,2), (3,4), (5,6)]
>>> for (first, last) in a:
...     print(first + last)
```

#### for문 응용

```python
marks = [90, 25, 67, 45, 80]

number = 0 
for mark in marks: 
    number = number +1 
    if mark >= 60: 
        print("%d번 학생은 합격입니다." % number)
    else: 
        print("%d번 학생은 불합격입니다." % number)
```

### for문과 continue

for문 안의 문장을 수행하는 도중에 continue문을 만나면 for문의 처음으로 돌아간다.

### for문과 함께 사용하는 range 함수

range(시작 숫자, 끝 숫자) 형태로, **끝 숫자는 포함되지 않는다**.

```
>>> a = range(10)
>>> a
range(0, 10)
```

```python
marks = [90, 25, 67, 45, 80]
for number in range(len(marks)):
    if marks[number] < 60:
        continue
    print("%d 번 학생 축하합니다. 합격입니다." % (number+1))
```

```python
for i in range(2, 10):
    for j in range(1, 10):
        print(i*j, end=" ")
    print('')   
```

>**TIP** 
>
>**매개변수 end를 넣어 준 이유**
>
>해당 결괏값을 출력할 때 다음줄로 넘기지 않고 그 줄에 계속해서 출력한다. 그다음에 이어지는 `print(' ')`는 2단, 3단 등을 구분하기 위해 두 번째 for문이 끝나면 결괏값을 다음 줄부터 출력한다.

### 리스트 내포 사용

```
[표현식 for 항목 in 반복가능객체 if 조건문]

[표현식 for 항목1 in 반복가능객체1 if 조건문1
       for 항목2 in 반복가능객체2 if 조건문2
       ...
       for 항목n in 반복가능객체n if 조건문n]
```

ex.

```python
>>> a = [1,2,3,4]
>>> result = [num * 3 for num in a]
>>> print(result)
[3, 6, 9, 12]

>>> a = [1,2,3,4]
>>> result = [num * 3 for num in a if num % 2 == 0]
>>> print(result)
[6, 12]
```



### 참고

* https://wikidocs.net/book/1