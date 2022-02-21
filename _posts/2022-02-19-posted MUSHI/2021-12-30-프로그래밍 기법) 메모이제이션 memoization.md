# 메모이제이션 memoization

프로그래밍에서 중간 계산 값을 변수에 저장해놓는다. 반복되는 계산을 하게 되는 부분은 코딩상으로 계속 계산하게 하는 것 대신에 중간 계산 값을 읽어서 사용하여 계산 시간을 줄이는 기법이다.



예시. 나쁜 예시

```python
def fibonacci(n):
    if n <= 2:
        return 1
    else:
        return fibonacci(n-2) + fibonacci(n-1)
        
for i in range(1,20):
	print(fibonacci(i))
```



예시. 좋은 예시

```python
memory = {1: 1, 2: 1}

def fibonacci(n):
    if n in memory:
        number = memory[n]
    else:
       number = fibonacci(n-1) + fibonacci(n-2)
       memory[n] = number
    return number
        
print(fibonacci(20))    
print(memory)
```



나쁜 예시에서는 fibonacci(1), fibonacci(2)가 들어갈때는 바로 if문에서 끝나게 된다.

하지만, fibonacci(2+n)의 단계에서 부터는 1, 1, 이라는 값이 고정인데도 불구하고 else문에서 재귀함수로 fibonacci를 두번이나 불러주게 된다. 이렇게 재귀로 함수를 두번 부르는 것 보다는 차라리 중간 계산 값(좋은 예시에서 memory)을 지정해놓고 fibonacci 계산을 할때 변수 값을 사용(읽어서)하는게 처리 속도가 더 빠르다.



-----

###### 출처

- AIFFEL LMS 

