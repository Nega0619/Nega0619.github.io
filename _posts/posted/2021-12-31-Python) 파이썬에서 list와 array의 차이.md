# List 란?

동적 배열(Dynamic Array)의 데이터 자료 구조



- 데이터들이 떨어진 위치에 저장되며 pointer로 연결된다.
  - pointer : 해당 데이터의 다음 데이터 주소값을 가리키고 있다.
- 임의 접근 불가
  - 순차 접근 / 시퀀셜 액세스 Sequential Access를 이용해야한다.



# Array 란?

연속(Sequence)형 데이터의 자료구조



- 데이터들이 연속된 메모리 영역에 순차적으로 저장
- 임의 접근 가능 : 인덱스 번호를 이용해서 빠르게 접근





# List와 Array의 차이



- 시간 계산

  | Type      | Read | Write/Update/Delete |
  | :-------- | :--- | :------------------ |
  | **Array** | O(1) | O(n)                |
  | **List**  | O(n) | O(1)                |

  데이터를 읽을때는 arraylist가 빠르지만 데이터를 입력/삭제/수정할때는 리스트 자료형이 더 빠르다.



- 담을 수 있는 자료형

  - list

    element사이에 여러가지 자료형을 넣을 수 있다.

  - array

    단 한가지의 자료형만 허락하며 선언시 어떤 자료형을 사용할지 옵션으로 준다.

    `arr.array('i', [1, 2, 3])` 

    - 'i' : integer 자료형으로만 추가 가능

      [다른 옵션 참고 시 클릭](https://docs.python.org/ko/3/library/array.html)

    - [1,2,3] : 실제 array의 데이터들..

    

- 사용 방법

  - list

    따로 모듈 추가 없이 바로 사용이 가능하다.

  - array

    `import array` 로 모듈을 import 해주어야 사용이 가능하다.

    





-----

###### 출처

- AIFFEL LMS 

- https://blog.martinwork.co.kr/theory/2018/09/22/what-is-difference-between-list-and-array.html

  

