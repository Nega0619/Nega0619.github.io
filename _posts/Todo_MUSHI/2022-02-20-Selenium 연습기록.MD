###### 전제사항

- 파이썬이 이미 설치된 상황이다.

--------

###### 참고링크

- https://greeksharifa.github.io/references/2020/10/30/python-selenium-usage/



오늘은 셀레니움이라는 패키지의 설치 및 사용방법에 대해 공부해보았다.

셀레니움은 파이썬 패키지 중 하나로, 웹 자동화에 많이 사용된다고 한다.



# 셀레니움 시작하기



## 설치법

cmd창에서 `pip install selenium`



## 드라이버 다운

사용하고 있는 브라우저 별로 셀레니움 웹드라이버를 다운로드해야한다. 사용하고 있는 웹 버전과 동일한 버전의 셀레니움 브라우저를 다운받아야 한다!

- [Google Chrome](https://sites.google.com/a/chromium.org/chromedriver/downloads)
- [Firefox](https://github.com/mozilla/geckodriver/releases)
- [Microsoft Edge](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/)
- [Safari](https://webkit.org/blog/6900/webdriver-support-in-safari-10/)



## 브라우저 열기

```pyth
from selenium import webdriver

chrome_driver = webdriver.Chrome('chromedriver.exe')

```



- 드라이버 다운하지 않았을 시 혹은 경로를 설정해주지 않았을 경우 뜨는 오류

  ![image-20220220161002912](../assets/img/posts/image-20220220161002912.png)

  - 해결법

    지금 작성하고 있는 파이썬 파일 이름이 test.py라고 했을 때, test.py가 있는 폴더 안에(같은 폴더안에) 드라이버를 넣어주면 정상작동한다.

    경로를 설정해주지 않았을 경우, 기본값은 현재 test.py가 존재하고있는 폴더의 경로이기 때문이다.

- 브라우저 버전과 맞지 않는 셀레니움 드라이버를 다운받았을 경우,

  - 어떤 창이 켜졌다가 바로 꺼지며 다음과 같은 오류를 내뱉는다.

    ![image-20220220162552359](../assets/img/posts/image-20220220162552359.png)



## 브라우저 닫기

```python
driver.close()	# 현재 탭 닫기
driver.quit()	# 현재 브라우저 닫기
```



## 페이지 앞으로 가기 / 뒤로 가기

```
driver.forward()
driver.back()
```



# 엘레멘트에 접근하기



## 엘레멘트란?