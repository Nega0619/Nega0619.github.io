# JSON 이란?

JSON이란 JavaScript Object Notation의 약자로, JavaScript의 데이터 객체 표현 방식입니다. 웹과 다른 애플리케이션 사이에서 HTTP 요청으로 데이터를 보낼 때 사용하는 표준 파일 포맷 중 하나입니다.

XML과 JSON 형식을 많이 쓰는데 특히 웹 API나 *config* 데이터를 전송할 때 많이 사용됩니다.

- JSON 형식의 예시

  ```json
  book = {
      "title" : "홍길동전",
      "author" : "허균",
      "price" : 8500,
      "publisher" : [{"name":"나라", "telephone" : "02-123-4567"}]
  }
  ```



# json 파싱

파이썬에서 Dictionary를 사용한다.



## 저장

```python
import json

book = {
    "title" : "홍길동전",
    "author" : "허균",
    "price" : 8500,
    "publisher" : [{"name":"나라", "telephone" : "02-123-4567"}]
}

with open("book.json", "w") as f:
    json.dump(book , f)
```



## 읽기

```python
import json

with open("book.json", "r", encoding="utf-8") as f:
    contents = json.load(f)
    print(contents["title"])
    print(contents["publisher"])
```

실행결과

> 홍길동전
>
> [{"name":"나라", "telephone" : "02-123-4567"}]





----

###### 용어정리

- config 데이터란?

  컴피그레이션(configuration)이란 설정을 의미합니다.

  컴퓨터 서버 네트워크 기기 또는 OS 소프트웨어 등의 설정을 가리킵니다.



-----

###### 출처

- AIFFEL LMS 