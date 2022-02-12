# enumerate란?

리스트, 문자열, 튜플 등이 있는 경우 순서와 리스트의 값을 함께 반환해주는 기능입니다.



```python
>>> list = [i for i in range(5)]
>>> list
[0, 1, 2, 3, 4]
>>> for i, value in enumerate(list):
	print(i,"번째 값 : ", value)

	
0 번째 값 :  0
1 번째 값 :  1
2 번째 값 :  2
3 번째 값 :  3
4 번째 값 :  4
```



 사용법 핵 간편!