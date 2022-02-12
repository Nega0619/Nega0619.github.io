파이썬에서 가장 자주 쓰는 print() 메서드.

여러가지 옵션을 주어서 다르게 사용하도록 해보았다.





# print("문자열", "123",sep='ㅁ')

sep은 print함수에서 문자열 여러개를 출력할 때 문자열 사이에 sep다음 문자를 끼워준다.

```python
>>> print("문자열", "123",sep='ㅁ')
문자열ㅁ123
>>> print("문자열", "123","abc",sep='y')
문자열y123yabc
```







# print("문자열", end='a')

```python
>>> for i in range(10):
	print(i,end=' ')

	
0 1 2 3 4 5 6 7 8 9 
>>> for i in range(10):
	print(i, end='%%')

	
0%%1%%2%%3%%4%%5%%6%%7%%8%%9%%
```



