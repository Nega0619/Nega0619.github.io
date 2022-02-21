# 람다 표현식 lambda expression 이란? 



람다란, 런타임에 생성해서 사용할 수 있는 익명 함수입니다. 자바에서  사용하는 익명함수와 비슷한 역할을 하는데, 람다 표현식은 사용법이 좀 더 간결합니다.



## 사용 예시

`print( (lambda a, b : a * b)(5,6))` 실행 결과 : 30

즉, 사용방법은 다음과 같습니다.

`(lambda 사용할 변수 : 변수를 이용한 수식)(변수에 들어갈 값)`



## map 함수

`map(함수 f, iterable객체)`  iterable객체의 요소를 하나씩 함수 f에 넣어줌. 이때 f()가 아니라 f만 (함수 이름만!) 매개변수로 넣어줌.



`list(map(lambda a,b : a*b ,(5,6),(10,100)))` 실행 결과 : [50, 600]

- print(map~~~)안 한 이유 : map의 주소값을 뱉어내기 때문.
  - 결과값을 뱉어내게 하기 위해 list함수에 map객체를 넣어줬음.
  - list뿐만 아니라 tuple도 가능함



`tuple(map(lambda a : a *100,[5]))` 실행결과 : (500,)

- [5] -> (5) 변경은 안됨. tuple로 들어갈 줄 알았는데 int형으로 들어가서 TypeError: int object is not iterable 뜸.
  - [5] -> (5,)로 넣어야 tuple로 들어감.



## reduce



## filter





----

###### 더공부할 사이트

https://wikidocs.net/64

위키독스 람다.



-------

###### 출처

- AIFFEL LMS 

