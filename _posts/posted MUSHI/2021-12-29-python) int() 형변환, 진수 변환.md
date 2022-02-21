# int()

int(매개변수) : return int형 매개변수

int(integer) -> Integer





## string to int

int("string") -> Integer





## n진수 형변환

2/8/16진수를 10진수로 변환



int("number", base=10) -> 10진수를 Integer 변환

int("binary number 2진수 문자열", 2) -> 2진수를 Integer로 변환

int("octal number 8진수 문자열", 8) -> 8진수를 Integer로 변환

int("decimal number 10진수 문자열" , 10) -> 10진수를 Integer로 변환

int("hexadecimal 16진수 문자열", 16) -> 16진수를 Integer로 변환

`int("ffff",16)`의 실행 결과 : 65535

`int("0xffff",16)`의 실행 결과 : 65535

`int(58)`의 실행 결과 : 58

`int("11", base =2)`의 실행 결과 : 3