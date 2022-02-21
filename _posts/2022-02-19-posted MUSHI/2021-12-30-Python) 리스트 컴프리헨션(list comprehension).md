# 리스트 컴프리헨션 List Comprehension이란?



리스트, 셋(set), 딕셔너리에 사용할 수 있는 기능으로, 기존의 이중 포문을 이용해서 선언해주어야 했던 변수를 한줄로 끝낼 수 있다.



```python
list_1 = [1,2]
list_2 = [i for i in range(100,500,100)]

result_list = []
for i in  list_1:
    for j in list_2:
        result_list.append((i,j))
        
print(result_list)
```

실행 결과

> [(1, 100), (1, 200), (1, 300), (1, 400), (2, 100), (2, 200), (2, 300), (2, 400)]



위와 같은 코드가 아래와 같이 짧아진다.

```python
list_1 = [1,2]
list_2 = [i for i in range(100,500,100)]

result_list = [(i,j) for i in list_1 for j in list_2]

print(result_list)
```

실행 결과

> [(1, 100), (1, 200), (1, 300), (1, 400), (2, 100), (2, 200), (2, 300), (2, 400)]

