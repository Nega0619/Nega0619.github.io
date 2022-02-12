# 순차 처리와 병렬 처리

![content img](https://d3s0tskafalll9.cloudfront.net/media/images/F-9-6.max-800x600.png)





## 순차 처리 serial processing

```python
import time

num_list = ['p1','p2', 'p3', 'p4']
start = time.time()

def count(name):
    for i in range(0, 100000000):
        pass
        
    print("finish:"+name+"\n")

for num in num_list:
    count(num)

print("time :", time.time() - start)
```





## 병렬 처리 parallel processing

```python
import multiprocessing
import time

num_list = ['p1','p2', 'p3', 'p4']
start = time.time()

def count(name):
    for i in range(0, 100000000):
        a = 1+2
    print("finish:"+name+"\n")
    

if __name__ == '__main__':
    pool = multiprocessing.Pool(processes = 4)
    pool.map(count, num_list)
    pool.close()
    pool.join()

print("time :", time.time() - start)
```





프로세세서,,?를 여러개사용하는것.

병렬처리인데사 왜 시간은 비슷하지?



- `pool = multiprocessing.Pool(processes = 4)` : 병렬 처리 시, 4개의 프로세스를 사용하도록 합니다. CPU 코어의 개수만큼 입력해 주면 최대의 효과를 볼 수 있습니다.
- `pool.map(count, num_list)` : 병렬화를 시키는 함수로, count 함수에 num_list의 원소들을 하나씩 넣어 놓습니다. 여기서 num_list의 원소는 4개이므로 4개의 count 함수에 각각 하나씩 원소가 들어가게 됩니다.
  즉, `count('p1')`, `count('p2')`, `count('p3')`, `count('p4')`가 만들어집니다.
- `pool.close()` : 일반적으로 병렬화 부분이 끝나면 나옵니다. 더 이상 pool을 통해서 새로운 작업을 추가하지 않을 때 사용합니다.
- `pool.join()` : 프로세스가 종료될 때까지 대기하도록 지시하는 구문으로써 병렬처리 작업이 끝날 때까지 기다리도록 합니다.

