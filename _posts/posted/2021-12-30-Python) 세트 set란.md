# 1. 세트란?

파이썬 자료형 중에는 수학적 의미를 가진 '집합'의 개념이 존재합니다. 이를 파이썬에서 `세트`라고 합니다.



-----

###### 자료형이란?

Data Type이라고도 하며, 프로그래밍 언어에서 정수(integer), 실수(float), 문자(character), 문자열(string), 부울(Boolean : True, False 값을 가지는 데이터) 와 같은 여러 종류의 데이터를 식별하는 분류입니다.

자료형은 컴퓨터와 프로그래머에게 어떤 종류의 자료를 다루고 있는지 알려줍니다. 자료형에 따라 사용할 수 있는 연산이나 값이 제한됩니다. (ex. string - string은 안됨 : "abc"-"a" 불가 / integer-integer는 가능 : 456-3 은가능)

------------



## 1.1 세트 자료형의 선언



### 변수 = {값1, 값2, 값3}

{ } 중괄호 안에 값을 저장하며 콤마`,`로 구분해 줍니다.



### set()

`set( 반복 가능한 iterable 객체 )` : return 세트 자료형의 데이터

```
>>> result = set('banana')
>>> result 
```

실행결과

> {'a', 'b', 'n'}

```
>>> result = set(range(3))
>>> result 
```

실행결과

> {0, 1, 2}

```
>>> result = set() 				# 빈 세트 만드는 법.
>>> result 
```

실행결과

> set()

<span style="color:red">result = set()  는 result={}와 같지 않음 XXXXXX dictionary로 선언되기 때문</span>



## 1.2 세트 자료형의 특징

- 순서가 정해져 있지 않음.

- 데이터 값 중복 불가.

  -> 중복 데이터를 넣으면, 중복 데이터는 무시됨.

- 특정 요소만 출력 불가.

  - 전체만 출력 가능

- 세트안에 세트 넣기 불가.

- 내용을 변경할 수 없는 세트도 있음

  - 형식 : `a = frozenset(range(10))` 

  - set안에 frozenset 넣는것은 가능

  - frozenset 안에 frozenset 넣는것도 가능

    ex 1. `result={frozenset("123"), frozenset("456")}` 		

    ex 2.  `result = frozenset({frozenset("123"), frozenset("456"), "789"})`



## 1.3 세트 연산 = 집합 연산



### 1.3.1 합집합

or 연산자를 사용하거나 set.union(세트1, 세트2)메서드 사용



#### | 연산자 사용

```
>>> a = {1, 2, 3}
>>> b = {4, 5, 6}
>>> a | b				# 혹은 a |= b		a = a|b 의 축약형
```

실행 결과

> {1, 2, 3, 4, 5, 6}



#### set.union()사용

```
>>> a = {1, 2, 3}
>>> b = {4, 5, 6}
>>> set.union(a,b)
```

실행 결과

> {1, 2, 3, 4, 5, 6}



#### 세트1.update(세트2)사용

```
>>> a = {1, 2, 3}
>>> b = {4, 5, 6}
>>> a.update(b)				# a에 결과가 저장됨.
```

실행 결과

> {1, 2, 3, 4, 5, 6}



### 1.3.2 교집합

#### & 연산자 사용

```
>>> a = {1, 2, 3}
>>> b = {4, 5, 6}
>>> a & b				# 혹은 a &= b		a = a & b의 축약형
```

실행 결과

> set()



#### set.intersection(a,b) 사용

```
>>> a = {1, 2, 3}
>>> b = {4, 5, 6}
>>> set.intersection(a, b)
```

실행 결과

> set()



#### 세트1.intersection_update(세트2) 사용

```
>>> x = {1, 2, 3}
>>> y = {2, 3, 4}
>>> x.intersection_update(y)			# x에 결과가 저장됨.
```

실행 결과

> {2, 3}



### 1.3.3 차집합

#### `-`연산자 사용

```
>>> x = {1, 2, 3}
>>> y = {2, 3, 4}
>>> x - y				# 혹은 x -= y
```

실행 결과

> {1}

#### set.difference(a,b) 사용

```
>>> x = {1, 2, 3}
>>> y = {2, 3, 4}
>>> set.difference(x,y)
```

실행 결과

> {1}



#### 세트1.difference_update(세트2)

```
>>> x = {1, 2, 3}
>>> y = {2, 3, 4}
>>> x.difference_update(y)			#x에 결과가 저장됨
```

실행 결과

> {1}



### 1.3.4 대칭차집합 a b

#### ^ 연산자 사용

```
>>> x = {1, 2, 3}
>>> y = {2, 3, 4}
>>> x ^ y				# 혹은 x ^= y		x = x^y축약형
```

실행 결과

> {1, 4}



#### set.symmetric_difference() 메서드 사용

```
>>> x = {1, 2, 3}
>>> y = {2, 3, 4}
>>> set.symmetric_difference()
```

실행 결과

> {1, 4}



#### 세트1.symmetric_difference_update(세트2)

```
>>> x = {1, 2, 3}
>>> y = {2, 3, 4}
>>> x.symmetric_difference_update(y)		# x에 결과가 저장됨
```

실행 결과

> {1, 4}



### 1.3.5 부분집합

#### <= 사용

```
>>> i = { 1, 2, 3, 4}
>>> j = {2, 3}
>>> i <= j
False
>>> j <= i
True
```

⊂

#### 세트1.issubset(세트2)

```
>>> i = { 1, 2, 3, 4}
>>> j = {2, 3}
>>> i.issubset(j)
False
>>> j.issubset(i)
True
```



### 1.3.6 진부분집합



#### < 사용

```python
>>> a= {1,2,3}
>>> b={2}
>>> a<b
False
>>> b<a
True
```



### 1.3.7 상위 집합



#### >= 사용

```
>>> i = {1, 2, 3, 4}
>>> j = {2, 3}
>>> i>=j
True
>>> j>=i
False
>>> 
```



#### 세트1.issuperset (세트2)

```
>>> i = {1, 2, 3, 4}
>>> j = {2, 3}
>>> i.issuperset(j)
True
>>> j.issuperset(i)
False
```





## 1.4 세트 자료형의 활용



### 1.4.1 데이터 검색



#### 값 in 세트변수

```python
>>> alphabet={'a', 'b', 'c', 'd', 'e'}
>>> 'c' in alphabet
```

실행결과

> True

```python
>>> alphabet={'a', 'b', 'c', 'd', 'e'}
>>> '5' in alphabet
```

실행결과

> False



#### 값 not in 세트

```python
>>> alphabet={'a', 'b', 'c', 'd', 'e'}
>>> 'c' not in alphabet
```

실행결과

> False

```python
>>> alphabet={'a', 'b', 'c', 'd', 'e'}
>>> '5' not in alphabet
```

실행결과

> True



### 1.4.2 데이터 추가

#### add()

```
>>> alphabet={'a', 'b', 'c', 'd', 'e'}
>>> alphabet.add(5)
>>> alphabet
```

실행결과

> {'b', 5, 'a', 'e', 'd', 'c'}



### 1.4.3 데이터 삭제



#### remove()

특정 요소 삭제 시, 요소가 없으면 에러를 발생시킴.

![image-20211230000228037](../../../../../Typora Images/image-20211230000228037.png)



#### discard()

특정 요소 삭제 시, 요소가 없어도 에러 발생 X

![image-20211230000334022](../../../../../Typora Images/image-20211230000334022.png)



#### pop()

임의 요소를 삭제한다.

![image-20211230000537768](../../../../../Typora Images/image-20211230000537768.png)

#### clear()

모든 요소를 삭제한다.

![image-20211230000614537](../../../../../Typora Images/image-20211230000614537.png)



### 1.4.4 데이터 갯수 세기

len()함수를 사용합니다.

![image-20211230000754973](../../../../../Typora Images/image-20211230000754973.png)

## 1.5 세트 자료형의 할당과 복사

![image-20211231171701202](../../../../../Typora Images/image-20211231171701202.png)

###### RAM : Random Access Memory



a 세트를 b 변수에 복사하고 싶다면 어떻게 하면 될까요? a를 선언해주고, `b = a`로 할당해주면 되는 걸까요?

![image-20211231171715649](../../../../../Typora Images/image-20211231171715649.png)

 ![image-20211230001438242](../../../../../Typora Images/image-20211230001438242.png)실상은 아닙니다. 



id(매개변수) 메서드는 매개변수의 고유 주소 값( 객체가 메모리에 위치해 있는 주소 )를 나타냅니다.

a 세트 변수를 b에 복사 하고 싶다는 것은, a 세트 객체와 b세트 객체가 따로 존재해야 한다는 것을 의미합니다. 그렇게 되면, a의 주소 값과 b의 주소 값이 달라야 하는데 위를 보면 같은 메모리 번지를 참고 하고 있습니다. 하나의 객체를 가리키는 방법이 하나 더 늘어난 것 뿐이라는 의미입니다.

즉, a와 b는 같은 객체를 가리키고 있으므로 a가 수정되면 b도 수정되게 됩니다. 다음과 같이요.

![image-20211230002058735](../../../../../Typora Images/image-20211230002058735.png)

a 와 b를 독립적인 다른 객체로 만들려면 copy()메서드를 사용하여 모든 요소를 복사해야 합니다. 이를 사용하여 객체를 변경하게 되면 a와 b는 독립적인 객체이므로, a의 변화가 b에 영향을 주지 않습니다.

![image-20211231171645839](../../../../../Typora Images/image-20211231171645839.png)

![image-20211230002253181](../../../../../Typora Images/image-20211230002253181.png)





## 1.6 세트 표현식



### for을 이용한 세트 생성

![image-20211230012946790](../../../../../Typora Images/image-20211230012946790.png)



### if를 이용한 세트 생성

![image-20211230020531592](../../../../../Typora Images/image-20211230020531592.png)

# 퀴즈

<img src="../../../../../Typora Images/image-20211230100922367.png" alt="image-20211230100922367"  />

답 :  <span style="color:white">b</span>

![image-20211230103733453](../../../../../Typora Images/image-20211230103733453.png)

답 :  <span style="color:white">B, E</span>



![image-20211231150948469](../../../../../Typora Images/image-20211231150948469.png)

답 :  <span style="color:white">c, d</span>



![image-20211230142741056](../../../../../Typora Images/image-20211230142741056.png)

답 :  <span style="color:white">a,c </span>





-----

###### 출처

- https://dojang.io/mod/page/view.php?id=2314 - 주 내용

- https://ko.wikipedia.org/wiki/%EC%9E%90%EB%A3%8C%ED%98%95 - 자료형