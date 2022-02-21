# 1장

## 테이블 조회하기

`select * from 테이블명`  :  정보까지 다 나옴

`desc 테이블명 (describe)`  :  속성 정보만 나옴

예쁘게 조회하기는 생략 (31pg)

## 컬럼명 별칭으로 출력하기

`select Deptno, '111' "DNAME" FROM dept;`
: “속성명”이 모두 동일하게 111로출력됨.
‘’ 과 달라!

`select Deptno (as) "1", '111' "DNAME" FROM dept;`

Deptno를 1로 출력하는 방법

## Distinct 명령어로 중복된 값을 제거하고 출력하기
`SELECT DISTINCT deptno FROM emp;`
- 정렬은 아니고 그냥 중복값만 없애줌.
- SELECT 바로 뒤에 와야함.
`SELECT DISTINCT job, ename FROM emp;`
- job에만 DISTINCT적용되지 않음. job, ename전부에 적용됨.

## 연결 연산자로 컬럼 붙여서 출력하기
`SELECT ename || job FROM emp;`
연결연산자 || 사용
`SELECT ename ||'''s job is '|| job "Name and Job" FROM emp;`
컬럼명 || `내용` -> 컬럼값내용 출력. 이때 ` 쓰려면 `` 두 개 써야함.

## Where 절
`SELECT * FROM emp WHERE ename = ‘smith’;`
- 문자와 날짜를 조회하고 싶을 때에는 꼭 작은 따옴표를 붙여야한다.
- 문자는 대소문자 구별이 필요하고 날짜는 대소문자 구별이 필요없다.

## sql에서 기본 연산자 사용하기
- +-*/는 컬럼명에 바로 해주어도 된다.

- 추가적인 연산자 : 
`IN(a,b,c)` : a나 b나 c인 조건 검색
`LIKE’      : 특별한 패턴이 있는 조건 검색
   - % : 글자 수에 제한이 없고 어떤 글자가 와도 됨.
      `SELECT empno, sal FROM emp WHERE sal LIKE 1%`
      1300 1600 등 출력
   - _ : 글자 수는 한 글자가만 올 수 있고 어떤 글자가 와도 됨. 
      `SELECT ename, sal FROM emp WHERE ename LIKE ‘A_____’`
      ALLEN 출력
`BETWEEN a AND b` : a와 b사이 조건 검색
`IS (NOT) NULL` : WHERE ename IS NOT NULL
`AND` 와 `OR`중 AND 먼저 수행 후 OR 수행 됨.

## 정렬하여 출력하기 – ORDER BY
- 오름차순 : ASC
- 내림차순 : DESC
- ORDER BY 컬럼명 DESC/ASC

## 집합연산자
- UNION (ALL) : 두 집합의 결과를 합쳐서 출력. 중복 값 제거하고 정렬(중복 값 제거 안하고 정렬 안함)
- INTERSECT : 교집합 결과를 출력 및 정렬
- MINUS : 차집합 결과를 출력 및 정렬(순서 중요)
``` SQL
SELECT empno, sal
FROM emp
MINUS
SELECT empno, sal
FROM emp
WHERE sal > 2500;
```

# 2장
- 단일행 함수 : 한번에 하나씩 처리하는 함수
- 복수행 함수 : 여러 건의 데이터를 동시에 입력받아 출력 1건을 만들어줌.

# 문자열처리함수

## INITCAP
`SELECT ename, INITCAP(ename) “INITCAP” FROM emp;`
영어에서 첫 글자만 대문자로 출력하고 나머지는 전부 소문자로 출력하는 함수

## LOWER
## UPPER
## LENGTH , LENGTHB
LENGTH : 입력된 문자열의 길이 계산.
LENGTHB : 입력된 문자열의 바이트 계산.

## CONCAT
|| 연산자와 동일한 기능
`SELECT CONCAT(ename, job) FROM emp WHERE deptno = 10;`

## SUBSTR
특정 길이의 문자만 골라낼 때
`SELECT SUBSTR(‘abcde’, 3, 2) from dual;`      cd
`SELECT SUBSTR(‘abcde’, -3, 2) from dual;`      cd
`SELECT SUBSTR(‘abcde’, -3, 4) from dual;`      cde
- 마이너스를 붙이면 파이썬처럼 검색
- 글자수 세는 것은 왼쪽 -> 오른쪽
- `SUBSTRB` : 세는 단위가 바이트

## 컬럼에 ‘아아’ 넣으면?
`SELECT ename, ‘A-B’ FROM emp;`
ename개수 만큼 A-B 출력 컬럼명은 ‘A-B’

## INSTR
주어진 문자열이나 컬럼에서 특정 글자의 위치를 찾아줌.
`SELECT INSTR(‘A-B-C-D-E’, ‘-’, 1, 3) FROM dual;`      6
`SELECT INSTR('A-B-C-D-E', '-', -1, 3) FROM dual;`      4
`SELECT INSTR('A-B-C-D-E', '-', -4, 3) FROM dual;`      2
- n번째부터 포함해서 검색. 위치는 맨처음부터 인덱스로 계산
- -하면 검색도 마이너스 방향으로 검색

## LPAD (컬럼명, 맞출 글자수, 채울 문자리스트)
`select ename, LPAD(ename, 10, '123456789') from emp;`
사원이름을 총 10바이트로 출력하되 해당 빈자리에는 해당자리 숫자로 채우기.
‘123456789’가 아닌 1-9 또는 ‘1-9’는 안됨.
- RPAD도 있음

## 응용문제
사원의 이름을 9자리로 출력하되 오른쪽 빈자리는 해당 자리수에 해당하는 숫자가 출력되도록 하시오.
> select ename, RPAD(ename, 9, substr('123456789', length(ename)+1, 9))
> from emp
> where deptno = 10;

## LTRIM
`select ename, LTRIM(ename, 'J') from emp;`
- 제거.
- RTRIM도 있음

## REPLACE
REPLACE (컬럼명, 문자1, 문자2)
- 문자1을 문자2로 바꿔줌.
- student 테이블에서 학생들의 이름과 전화번호, 전화번호에서 국번부분만 *처리해서 출력하시오. 국번은 3자리.
select name, tel, replace(tel, substr(tel, Instr(tel, ')', 1)+1,4), '***') 
from student;

# 숫자 관련 함수

## ROUND/TRUNC(숫자, 출력되길 원하는 숫자자리수)
- 0: 1의자리 숫자까지 나옴
- -1: 10의자리 숫자까지 나옴
- 1: 소수점 첫째자리까지 나옴
- ROUND : 반올림임
- TRUNC : 버림

## MOD/CEIL/FLOOR(숫자)
- MOD : 나머지 값구하는 함수
- CEIL : 주어진 숫자에서 가장 큰 정수를 구하는 함수, 올림
- FLOOR : 주어진 숫자에서 가장 작은 정수를 구하는 함수, 내림

## POWER(숫자, 숫자2)
숫자^숫자2

# 날짜 관련 함수 91pg

# 형변환 함수

# 일반함수

## NVL
NULL값을 만나면 다른 값으로 치환해서 출력하는 함수
`NVL(sal, 0)` sal값이 NULL이면 0 출력

## NVL2
NULL값이 아니라면 다른 값으로 치환해서 출력

## DECODE
if문과 동일, SELECT에 사용
- DECODE (컬럼1, 컬럼2, 맞으면출력, 아니면출력)
- DECODE (컬럼1, 컬럼2, 1=2이면 출력, 
         컬럼3, 1=3이면 출력, 둘다 아니면 출력)
- DECODE 안의 DECODE도 가능

## case문
``` SQL
CASE 조건 WHEN 결과1 THEN 출력1
      [WHEN 결과2 THEN 출력2]
       ELSE 출력3
END “컬럼명”
```
내부에 콤마가 사용되지 않는다.

# 정규식 함수로 다양한 조건 조회하기 123pg

# 3장
 
# GROUP 함수 종류

## COUNT
## SUM
## AVG
## MAX, MIN
## STDDEV , VARIANCE
- STDDEV : 표준편차 구하는 함수
- VARIANCE : 분산 구하는 함수

# GROUP BY 절을 이용하여 특정 조건으로 세부적인 그룹화하기

> 주의점
- SELECT절에 사용된 그룹 함수 이외의 컬럼이나 표현식은 반드시 GROUP BY절에 사용되어야 한다. 그렇지 않을 경우 에러 발생 => 컬럼이나 표현식 전부 사용되어야 한다.
``` SQL
SELECT deptno, job, AVG(NVL(sal, 0)) "AVG_SAL"
FROM emp
GROUP BY deptno;
/* 에러 */

SELECT deptno, job
FROM emp
GROUP BY deptno;
/* 에러 */

SELECT deptno, job, AVG(NVL(sal, 0)) "AVG_SAL"
FROM emp
GROUP BY deptno, job;
/* ㅇㅋ */
```
- GROUP BY절에는 반드시 컬럼명이 사용되어야 하며 컬럼 Alias는 사용하면 안된다.

# HAVING절을 사용해 그룹핑한 조건으로 검색하기
그룹 함수를 조건으로 사용하고 싶을 경우에는 WHERE 대신에 HAVING 절을 사용하면 된다. WHERE절에는 사용 안됨!
GROUP BY조건절 같이 써야한다.
**GROUP함수를 조건으로 할 경우에는 WHERE을 사용하면 안된다.** 

``` SQL
SELECT deptno, AVG(NVL(sal,0))
FROM emp
WHERE deptno > 10
GROUP BY deptno
HAVING AVG(NVL(sal, 0)) > 2000;
```
# 반드시 알아야 하는 다양한 분석 함수들 167 pg

## ROLLUP()
각 기준별 소계를 요약해서 보여줌
```
SELECT EMPNO, JOB, AVG(SAL)
FROM EMP
WHERE EMPNO > 7500
GROUP BY ROLLUP(EMPNO, JOB);
```
직원번호별(직원번호, NULL), 직원과 직업별(직원번호, 직업별) 전체그룹집계 (NULL, NULL)

```
SELECT  DEPTNO, JOB, AVG(SAL)
FROM EMP
WHERE EMPNO > 7700
GROUP BY ROLLUP(DEPTNO, JOB);
```            
직원번호별(EMPNO, 직원번호, NULL), 직원과 직업별(EMPNO, 직원번호, 직업별) 전체그룹집계 (EMPNO, NULL, NULL)

## CUBE()
소계와 전체 합계까지 출력하는 함수
rollup과 비슷하지만 cube(deptno, job)하면, deptno별 job별 deptno,job별, 전체그룹별

## GROUPING SETS()
SELECT NAME, GRADE, DEPTNO1, COUNT(*), SUM(WEIGHT)
FROM STUDENT
GROUP BY GROUPING SETS(GRADE, DEPTNO1), NAME;
- NAME, GRADE, NULL
- NAME, NULL, GRADE

SELECT NAME, GRADE, DEPTNO1, COUNT(*), SUM(WEIGHT)
FROM STUDENT
GROUP BY GROUPING SETS(GRADE), NAME, DEPTNO1;
- 그냥 테이블 조회와 동일

SELECT NAME, GRADE, DEPTNO1, COUNT(*), SUM(WEIGHT)
FROM STUDENT
GROUP BY GROUPING SETS(GRADE, DEPTNO1, NAME);
- GRADE, NULL, NULL
- NULL, DEPTNO1, NULL
- NULL, NULL, NAME

## LISTAGG()
SELECT에 사용
LISTAGG(컬럼명, ‘구분자’) WITHIN GROUP (ORDER BY 컬럼명 혹은 조건)
이때 오더바이 조건에 HIREDATE 들어가면 고참->신참 순

## PIVOT / UNPIVOT          185pg(중간확인용)
SELECT MAX (DECODE(DAY, ‘SUN’, DAYNO)) SUN,
      ...
   MAX (DECODE(DAY, ‘SAY’, DAYNO)) SAT
FROM CAL
GROUP BY WEEKNO
ORDER BY WEEKNO;

SELECT * FROM (SELECT WEEKNO “WEEK”, DAY, DAYNO
        FROM CAL)
PIVOT( MAX (DAYNO) FOR DAY IN (‘SUN’ AS “SUN”,
                 ‘MON’ AS “MON”,
                 ‘TUE’ AS “TUE”,
                 ‘WED’ AS “WED”,
                 ‘THUR’ AS “THUR”,
                 ‘FRI’ AS “FRI”,
                 ‘SAT’ AS “SAT” )
달력만듬.
DAYNO가 가로로 나옴.

/* 학년 별로 사람수세기*/
SELECT * FROM (SELECT STUDNO, GRADE FROM STUDENT)
PIVOT (COUNT(STUDNO) FOR GRADE IN (4, 3, 2, 1));
COUNT (*) 하면 안됨.

SELECT * FROM UPIVOT [PIVOT 결과를 테이블로 만든 것]
UNPIVOT ( EMPNO FOR JOB IN (CLERK, MANAGER, PRESIDENT, ANALYST, SALESMAN));

## LAG() 함수
SELECT에 사용
LAG (컬럼명, N, M) : N개를 미루고 1-N까지는 M으로 출력

## LEAD() 함수
LAG와 반대
LEAD(컬럼명, N, M) : N개를 

## RANK() 함수
순위 출력함수
`SELECT RANK(‘SMITH’) WITHIN GROUP (ORDER BY ENAME) “RANK” FROM EMP;`
이때 RANK(값)은 ORDER BY(값의 컬럼명) 이어야 한다.
`SELECT RANK() OVER (ORDER BY ENAME) "RANK" FROM EMP;`
괄호 조심

## DESC_RANK 202pg

# 4장 조인
조인사용시에는 테이블이름.컬럼이름을 습관으로!

## EQUI JOIN(등가조인)
동일한 조건을 가진 데이터를 후행 테이블에서 꺼내오는 방법.
> ORACLE
SELECT E.EMPNO, E.ENAME, D.DNAME
FROM EMP E, DEPT D
WHERE E.DEPTNO = D.DEPTNO;

> ANSI JOIN 
SELECT E.EMPNO, E.ENAME, D.DNAME
FROM EMP E JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;

3개 묶기
SELECT S.NAME, D.DNAME, P.NAME
FROM STUDENT S JOIN DEPARTMENT D
ON S.DEPTNO1 = D.DEPTNO
JOIN PROFESSOR P
ON S.PROFNO = P.PROFNO;

## NON-EQUI JOIN 비등가조인
같은 조건이 아닌 크거나 작거나하는 경우의 조건으로 조회할 때 씀.

> ORACLE
SELECT C.GNAME, TO_CHAR(C.POINT, ‘999.999’), G.GNAME
FROM CUSTOMER C, GIFT G
WHERE C.POINT BETWEEN G.G_START AND G.G_END;

> ANSI JOIN 구문
SELECT C.GNAME, TO_CHAR(C.POINT, ‘999,999’), G.GNAME
FROM CUSTOMER C JOIN GIFT G
ON C.POINT BETWEEN G.G_START AND G.G_END;

## OUTER JOIN 아우터조인
한쪽 테이블에는 데이터가 있는데 한쪽 테이블에는 없는 경우 데이터가 있는 쪽 테이블 내용을 전부 출력하게 하는 방법

- LEFT 조인
> ORACLE
SELECT S.NAME, P.NAME
FROM STUDENT S, PROFESSOR P
WHERE S.PROFNO = P.PROFNO(+);
-> 데이터가 없는 쪽에 (+) 데이터 표시

> ANSI JOIN
SELECT S.NAME, P.NAME
FROM STUDENT S LEFT OUTER JOIN PROFESSOR P
ON S.PROFNO = P.PROFNO;

- RIGHT 조인
SELECT S.NAME, P.NAME
FROM STUDENT S, PROFESSOR P
WHERE S.PROFNO(+) = P.PROFNO;
-> 데이터가 없는 쪽에 (+) 데이터 표시

> ANSI JOIN
SELECT S.NAME, P.NAME
FROM STUDENT S RIGHT OUTER JOIN PROFESSOR P
ON S.PROFNO = P.PROFNO;


- FULL OUTER 조인
SELECT S.NAME, P.NAME
FROM STUDENT S, PROFESSOR P
WHERE S.PROFNO = P.PROFNO(+)
UNION
SELECT S.NAME, P.NAME
FROM STUDENT S, PROFESSOR P
WHERE S.PROFNO(+) = P.PROFNO;
-> 데이터가 없는 쪽에 (+) 데이터 표시
-> FULL OUTER 조인이 없어서 UNION 사용

> ANSI JOIN
SELECT S.NAME, P.NAME
FROM STUDENT S FULL OUTER JOIN PROFESSOR P
ON S.PROFNO = P.PROFNO;

- ORACLE 아우터 조인 주의사항
부서에 대한 정보를 모두 보여주고 부서 번호가 20인 사원의 사원 번호 이름 급여를 보여주시오
`NO!!!!!!`
SELECT D.DEPTNO, D.DNAME, D.LOC, E.EMPNO, E.ENAME, E.SAL
FROM DEPT D, EMP E
WHERE D.DEPTNO = E.DEPTNO(+)
   AND E.DEPTNO = 20
ORDER BY 1;
`YES!!!!!!!!!!!!!!!!!!!!!!!!!!!!!`
SELECT D.DEPTNO, D.DNAME, D.LOC, E.EMPNO, E.ENAME, E.SAL
FROM DEPT D, EMP E
WHERE D.DEPTNO = E.DEPTNO(+)
   AND E.DEPTNO(+) = 20
ORDER BY 1;

- ANSI 아우터 조인 주의사항
직업이 CLERK인 사우너 정보(번호, 이름, 직업)을 출력하고 그 중에 CHICAGO에 위치한 부서에 소속된 사원의 부서 정보(번호, 명, 위치)를 출력하세요.
`NO!!!!!!`
SELECT E.EMPNO, E.ENAME, E.JOB, D.DEPTNO, D.NAME, D.LOC
FROM EMP E LEFT OUTER JOIN DEPT D
ON( E.DEPTNO = D.DEPTNO AND D.LOC = ‘CHICAGO’ )
WHERE E.JOB = ‘CLERK’;
`YES!!!!!!!!!!!!!!!!!!!!!!!!!!!!!`
SELECT E.EMPNO, E.ENAME, E.JOB, D.DEPTNO, D.NAME, D.LOC
FROM EMP E LEFT OUTER JOIN DEPT D
ON( E.DEPTNO = D.DEPTNO AND D.LOC = ‘CHICAGO’ AND E.JOB = ‘CLERK’);

## SELF 조인
원하는 데이터가 하나의 테이블에 다 들어있을 경우사용
> ORACLE
SELECT E1.ENAME, E2.ENAME
FROM EMP E1, EMP E2
WHERE E1.MGR = E2.EMPNO;

> ANSI
SELECT E1.ENAME, E2.ENAME
FROM EMP E1 JOIN EMP E2
ON E1.MGR = E2.EMPNO;




### SET PAGES 50
50개까지 값 출력

### cascade <-> restrict
연쇄 삭제
`drop table 테이블명 cascade constraint;`
`Alter table emp drop foreign key dept_no references test(dept_no) on delete cascade on update cascade;`

# Delete 조건
- on delete cascade
- on delete set null 		: 부모 테이블의 데이터가 지워질 경우 자식 테이블의 값을 null