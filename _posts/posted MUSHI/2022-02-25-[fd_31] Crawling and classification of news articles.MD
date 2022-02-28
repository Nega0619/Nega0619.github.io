오늘은 아이펠 Fundamental 노드 31번을 공부하고 공부한 내용을 포스팅해보았습니다.

------

###### 출처

- AIFFEL LMS 

  문제시 연락 부탁드립니다. :)

---

###### 목차

- HTML과 태그
- 선택자



[TOC]



----

###### 사용하는 패키지 / 모듈

- Mecab

  - $ git clone https://github.com/SOMJANG/Mecab-ko-for-Google-Colab.git

    $ cd Mecab-ko-for-Google-Colab

    $ bash install_mecab-ko_on_colab190912.sh

- beautifulsoup4
- newspaper3k
- konlpy

----



# HTML과 태그

크롤링이란, 웹 페이지로부터 데이터를 추출하는 것입니다. 크롤링하는 소프트웨어는 크롤러라고 합니다.

크롤링을 하기 위해서는 HTML과 태그에 대해서 알아야합니다. 

- HTML이란 Hyper Text Markup Language의 준말입니다.
  - HyperText : 문서와 문서가 링크로 연결되어 있다.
  - Markup : 태그로 이루어져 있다.
- 태그란 정보를 정의하는 형식입니다.



# 선택자

원활한 크롤링을 위해서는 HTML 내의 선택자가 매우 중요합니다. 선택자란, 특정 태그들에 그룹이나 번호를 주는 기능입니다. 

```HTML
<html> 
    <head> 
    </head> 
    <body> 
        <h1> 장바구니
            <p> 라운드티
                <span> 25 </span> 
                <span> 29000 </span> 
                <span> 의류</span> 
                <a href = 'http://www.naver.com'> 이동 </a> 
            </p> 
            <p> 시계
                <span> 28 </span>
                <span> 32000 </span> 
                <span> 악세서리 </span> 
                <a href = 'http://www.facebook.com'> 이동 </a> 
            </p> 
        </h1> 
    </body> 
</html>
```

위와 같은 HTML 문서가 있다고 할 때, `<p>` 태그는 2개, `<span>`태그는 6개, `<a>` 태그는 2개가 있습니다.



- `<p>`
  - Paragraph 문단의 약자로, 하나의 문단을 만들 때 쓰입니다.
- `<span>`
  - `<span>`태그는 `<div>`태그처럼 특별한 기능을 갖고 있지 않으며 CSS와 함께 쓰입니다.
  - display속성이 Block이 아닌, Inline이며, [CSS Display](https://ofcourse.kr/css-course/display-%EC%86%8D%EC%84%B1) 페이지에서 세부 정보를 알 수 있습니다.
  - 줄바꿈안되는 속성
  - `<div>`
    - Division의 약자로, 레이아웃을 나누는데 쓰입니다.
    - 특별한 기능은 없으며 가상의 레이아웃을 설계하는데 쓰이며 CSS와 연동하여 쓰입니다.
    - 줄바꿈이 됨
- `<a>`
  - 하이퍼링크를 걸어주는 태그
  - 속성
    - href : 클릭시 이동할 링크
    - target : 링크를 여는 방법
      - `_self`: 현재 페이지 (기본값)
      - `_blank`: 새 탭
      - `_parent`: 부모 페이지로, iframe 등이 사용된 환경에서 쓰입니다.
      - `_top`: 최상위 페이지로, iframe 등이 사용된 환경에서 쓰입니다.
      - *`프레임이름`*: 직접 프레임이름을 명시해서 사용할 수도 있습니다.

- [HTML 태그](https://ofcourse.kr/html-course/p-%ED%83%9C%EA%B7%B8)



```html
<html> 
    <head> 
    </head> 
    <body> 
        <h1> 장바구니
            <p id='clothes' class='name' title='라운드티'> 라운드티
                <span class = 'number'> 25 </span> 
                <span class = 'price'> 29000 </span> 
                <span class = 'menu'> 의류</span> 
                <a href = 'http://www.naver.com'> 바로가기 </a> 
            </p> 
            <p id='watch' class='name' title='시계'> 시계
                <span class = 'number'> 28 </span>
                <span class = 'price'> 32000 </span> 
                <span class = 'menu'> 악세서리 </span> 
                <a href = 'http://www.facebook.com'> 바로가기 </a> 
            </p> 
        </h1> 
    </body> 
</html>
```

위 코드는 id와 class를 위 코드블록에서 추가한 코드입니다.

- id : CSS의 스타일을 적용할때 매우 유용
- class : 비슷한 성격을 가지는 그룹을 정의해서 한번에 관리하고 싶을때 사용



# BeautifulSoup 패키지

HTML이나 XML 문서로부터 원하는 정보를 추출해주는 파이썬 크롤링 패키지입니다.



```python
soup = BeautifulSoup(html, 'html.parser')
```

`BeautifulSoup(입력 문자열, parser종류)` 형식으로 사용하는 메소드입니다.

- html 자리에 html 문서 혹은 html형식으로 작성된 문자열을 넣어줍니다.
- 'html.parser' : 입력 문자열이 html 형식으로 작성되어 있으니, HTML 문법을 기반으로 파싱하라는 뜻



위와같이 인스턴스를 생성하고나면, 정보를 가져오는 것은 `select()` 메소드를 이용하여 가능합니다.

```python
print(soup.select('body'))
```

실행결과

> ```
> [<body>
> <h1> 장바구니
>             <p class="name" id="clothes" title="라운드티"> 라운드티
>                 <span class="number"> 25 </span>
> <span class="price"> 29000 </span>
> <span class="menu"> 의류</span>
> <a href="http://www.naver.com"> 바로가기 </a>
> </p>
> <p class="name" id="watch" title="시계"> 시계
>                 <span class="number"> 28 </span>
> <span class="price"> 32000 </span>
> <span class="menu"> 악세서리 </span>
> <a href="http://www.facebook.com"> 바로가기 </a>
> </p>
> </h1>
> </body>]
> ```



즉, soup.select('찾는정보') 형식으로 사용합니다.

- [BeautifulSoup로 크롤링하기](https://m.blog.naver.com/kiddwannabe/221177292446)



부모와 자식관계라면 다음과 같이 사용합니다.

`soup.select( 부모 > 자식 )`

다음과 같은 html body 태그가 있으면, 

```html
<body>
  <h1> 장바구니
    <p class="name" id="clothes" title="라운드티"> 라운드티
      <span class="number"> 25 </span>
      <span class="price"> 29000 </span>
      <span class="menu"> 의류</span>
      <a href="http://www.naver.com"> 바로가기 </a>
    </p>
    <p class="name" id="watch" title="시계"> 시계
      <span class="number"> 28 </span>
      <span class="price"> 32000 </span>
      <span class="menu"> 악세서리 </span>
      <a href="http://www.facebook.com"> 바로가기 </a>
    </p>
  </h1>
</body>
```

h1 요소의 자식에서 .name의 자식인 클래스 menu인 경우만 뽑는다면,

```python
print(soup.select('h1 .name .menu'))
```

실행 결과

> ```
> [<span class="menu"> 의류</span>, <span class="menu"> 악세서리 </span>]
> ```



잘못된 입력

```python
print(soup.select('html > h1'))
```

> ```
> []
> ```

h1이 html의 자식이긴 하지만, 바로 아래 자식은 아니고, body의 자식입니다. 즉 html > body > h1인 것입니다. 하지만 html의 바로 아래 자식인것 처럼 사용하면 위와 같이 빈 정보를 출력합니다.



# newspaper3k 패키지

뉴스데이터를 크롤링하기 위해 만들어진 패키지입니다.



```python
from newspaper import Article

url = 'https://news.naver.com/main/read.nhn?mode=LSD&mid=sec&sid1=101&oid=030&aid=0002881076'

#- 언어가 한국어이므로 language='ko'로 설정해주었습니다.
article = Article(url, language='ko')
article.download()
article.parse()
```

위 코드를 실행하면, URL의 뉴스 데이터를 파싱합니다.



- `article.title` : 기사 제목을 출력해줍니다.

- `article.text` : 기사의 내용을 출력해줍니다.



















