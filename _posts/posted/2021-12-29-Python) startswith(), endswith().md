# 언제 사용?

여러 문자열들을 가지고와서 원하는 문자로 시작하거나 끝나는 문자열을 검색할때 사용합니다.



# startswith



```python
EmployeeID = ['OB94382', 'OW34723', 'OB32308', 'OB83461', 
                                  'OB74830', 'OW37402', 'OW11235', 'OB82345'] 
Production_Employee = [P for P in EmployeeID if P.startswith('OB')]   # 'OB'로 시작하는 직원 ID를 다 찾아봅니다
Production_Employee
```



실행결과

> ['OB94382', 'OB32308', 'OB83461', 'OB74830', 'OB82345']



# endswith

```python
import os
image_dir_path = os.getenv("HOME") + "/data/pictures"   

photo = os.listdir(image_dir_path )
png = [png for png in photo if png.endswith('.png')]
print(png)
#해당디렉토리에 실행결과의 이미지.PNG들이 존재해야 아래와 같이 뜸.
```



실행결과

> ```
> ['image5.png', 'image2.png', 'image12.png', 'image3.png', 'image8.png', 'image13.png', 'image6.png', 'image4.png', 'image14.png', 'image9.png', 'image10.png', 'image7.png', 'image11.png']
> ```



