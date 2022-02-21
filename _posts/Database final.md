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

# 정의

## SQL 정의
- 데이터를 Access하기 위해 데이터 베이스와 통신하는 언어

## ProC 정의
- SQL과 연동하여 DB의 데이터를 처리할 수 있게 해주는 C언어 기반의 프로그래밍 언어
- ProC로 작성된 소스코드는 오라클 프로씨 프리컴파일러에 의해 보통 C/C++ 소스코드로 해석된다.
- C로 작성된 소스코드에 오라클 SQL의 DML명령문이나 DDL 명령문이 EXEC구문과 결합된 형태가 추가된 것임을 알 수 있다.

## PL/SQL 정의
- DB내에서 집합적으로 처리할 수 밖에 없는 SQL의 한계를  넘어, 일반 프로그래밍 언어처럼 DB내에서 절차형으로 비교적 자유롭게 프로그래밍을 구현할 수 있도록 오라클 등의 DBMS에서 지원해 주는 절차형 프로그래밍 툴

## 호스트 변수
- 데이터 베이스에서 처리한 결과를 가져와서 사용할 변수
- 선언절에서 명시적으로 선언한다.
- 선언한 대로 대문자 / 소문자의 포맷을 사용한다.
- SQL문에서는 앞에 `:`을 붙인다.
- C문에서는 `:`붙이지 않는다.
- SQL의 예약어를 사용하면 안된다.

## indicator 변수
- 오라클 문의 실행 결과를 측정하기 위한 용도로 사용한다.
- connect를 통해 디비 서버와 연결을 시도했을 때 성공/실패에 대한 리턴값이라고 보면 됨.
- 선언 : short 이름_ind;
- 사용 : `:호스트변수:이름_ind, ...`

# ProC
- SQL문 전에 Exec 붙이고 마지막에 꼭 ; 붙여야 함.

## ProC 연결부분

`exec sql connect 'username' identified by 'passwd';'`
<br/>
`exec sql commit work release;`
: 바로 전 작업뿐만 아니라 commit한 마지막구간 ~ 최근 작업 전부를 commit하는 것. (Insert/ delete/ alter할 때 꼭 고려!)
<br/>
`exec sql rollback work release;`
<br/>
## ProC Commit
- commit 설명
	- 프로그램에서 commit / rollback사용 X : 프로그램은 전체가 하나의 transcaction
	- commit을 통해 디비 변경을 확정
	- commit전에 다른 유저는 변경된 데이터에 접근할 수 없고, 변경전 상태의 data만 열람할 수 있음.
- commit 되면 수행되는 작업들
	- 현재의 트랜젝션에서 변경된사항을 디비에 반영
	- 변경사항을 다른 유저가 열람할 수 있게 함
	- 모든 savepoint를 삭제
	- 모든 레코드 및 테이블의 락을 해제
	- 모든 커서를 해제
	- 트렌젝션 종료
	- DDL 문 실행 전후에 auto Commit

## ProC RollBack
- tosavepoint를 사용하면 현재의 트랜잭션의 처음이 아닌 중간까지 되돌리는 것이 가능함
- Rollback되면 수행되는 작업들
	- 현재 트랜잭션 내에서 디비에 대해 발생했던 변경이 전부 취소됨.
	- 모든 savepoint가 삭제됨
	- 현재 트랜잭션이 종료됨
	- 모든 레코드 및 테이블의 락이 해제됨
	- 모든 커서를 해제함

## ProC 예외처리
- exec sql whenever [condition] [action];
- condition : sqlerror / sqlwarning / not found
- action : continue / do [error_logic] / do break / do continue / goto [label], stop
	- continue : 가능하면 프로그램의 다음 문장을 실행
	- do continue : 루프에서 사용. 다음 루프 실행.

## ProC SQL문
`exec sql select 컬럼명, 컬럼명 into 호스트변수명, 호스트변수명
from 테이블이름 where 조건;`
<br/>
`exec sql update 테이블 이름 set 컬럼명 = :호스트변수 where 조건`
`exec sql commit work;`
<br/>
`exec sql insert into 테이블명(컬럼1, 컬럼2) values (:호스트변수1, :호스트변수2);`
`exec sql commit work;`
<br/>
`exec sql delete from 테이블 이름 where 조건;`
`exec sql commit work;` 
<br/> 
==우리조에서 쓴 커서==

exec sql `begin` declare section;
호스트변수1;
호스트변수2;
exec sql `end` declaere section;
<br/>
초기화영역. varchar은 memset해준다.
<br/>
exec sql `declare` 커서이름 cursor for
select 컬럼1, 컬럼2 from customer;
<br/>
exec sql `open` 커서이름;
<br/>
`exec sql whenever not found do break;`
<br/>
for(;;)
{
	exec sql `fetch` 커서이름 into
	:호스트변수1, : 호스트변수2;
	
호스트변수로 각각 할 것들
}
exec sql `close` 커서이름;
<br/>
## ProC에서 가변문자길이 쓰기
- declare 섹션에서 id varchar[100]선언
- strcpy((char *) id.`arr`, "카피할 문자열");
- id.len = (short) strlen((char *)id.arr);

## PL/SQL문의 실행 18pg 
- DEVELOPER에서. <br/>
`exec sql 호출될 PL/SQL블록 name;`<br/> <br/>
`exec sql begin sql문 end;`<br/> <br/> `exec sql declare cursor_name cursor for`<br/>
`select 문 exec sql open cursor_name;`<br/> `exec sql fetch cursor_name into variable;` <br/>
`exec sql close cursor_name;`

# PL/SQL
- 선언부(declare) 실행부(begin) 예외처리부(exception)으로 구성됨.
- PL/SQL 블록 작성시 기본 규칙과 권장사항
	- 문장은 여러줄에 걸쳐질 수 있으나 키워드는 분리X
	- 예약어는 식별자명으로 사용불가하나 alias로는 사용가능
	- 리터럴(날짜, 문자)는 ` 사용
	- 주석처리 -- 

## select
- select 컬럼 리스트 into 변수 리스트  from 테이블  where 조건;

``` SQL
set serverouput on;  		-- 결과값을 화면에 출력하기 위해서 사용
declare
	v_empid		employee.employ_id%type
	-- employ_id와 동일한 데이터 형으로 선언함
	v_salary		employee.salary&type
Begin
	select employee_id, salary into v_empid, v_salary
	from employees
	where employee_id = 197; -- 혹은 employee_id = &id;
	
	DBMS_OUTPUT.PUT_LINE(v_empid || ' == '|| v_salary);
END;
/
```

## insert

``` SQL
Begin
	insert into pl_test
	values (pl_seq.Nextval, 'aaa');
End;
/
```

# 5장 DDL명령

## create
- 테이블 복사하기
	- create table dept4 `AS` select dcode, dname from dept2; 

## Alter
- 새로운 컬럼 추가하기
	- alter table dept6 `add` (Loc Varchar(20);
- 컬럼 이름 변경하기
	- alter table dept6 `rename column` location2 `to` loc;
	- location2 -> loc 이름변경
- 데이터 크기 변경하기
	-  alter table dept7 `modify` (loc varchar(20);
- 컬럼 삭제하기
	- alter table dept7 `drop column` loc (cascade constraints );
		- ()부분은 외래키일시에 추가해야함.

## Truncate 명령
- 사용중인 공간 전부 내놓기
- truncate table dept7;

## drop
- drop table dept7;


## data dictionary
- 딕셔너리에 저장되어 있는 정보 : 
	- 제약조건 정보
	- 사용자에 대한 정보
	- 권한이나 프로파일 롤에대한 정보
	- 감사(audit)에 대한 정보
	- 오브젝트들이 사용하고 있는 공간의 정보
	- 오라클 데이터베이스의 메모리 구조와 파일에 대한 구조 정보
- Dictionary
	- static dictionary
		- USER_XXX : 해당 사용자가 생성한 오브젝트들만 조회가능
		- ALL_XXX : 해당 사용자가 접근할 수 있는 모든 오브젝트
		- DBA_XXX : DBA권한을 가진 사람만  접근할 수 있는 오브젝트
	- dynamic dictionary
		- V$_XXX
- `ANALYZE TABLE ST_TABLE COMPUTE STATICS;` 
- 위를 실행시켜야 아래 코드가 정상작동한다.
- `SELET NUM_ROWS(레코드수), BLOCKS(블록주소) FROM USER_TABLES WHERE TABLE_NAME = 'ST_TABLE';`

# 6장 DML 

## INSERT
- INSERT INTO 테이블명(속성명1, 2, ..) VALUES (속성명1, 2, ..);
- insert와 서브쿼리 이용, 여러 행 넣기
	- insert into professor3 select * from professor;
- insert와 all을 이용하여 여러 테이블에 여러 행 생성하기
	- insert all 
	- when profno between 1999 then into prof3 values (profno, name)
	- when profno  between 2000 and 2999 then into prof 4 values (profno, name)
	- select profno, name
	- from professor;

## update
- update 테이블명 set column = 값 where 조건;

## delete
- delete from 테이블명 where 조건;

## merge
- 테이블1 + 테이블2 = 테이블1
- merge into 테이블1 (total : 전체를 부른다는 것.)
- using 테이블2
- on (병합 조건절)
- when matched then 		: 서로 같다면
	- 조건이 만족한다면 update / delete 실행될 것.
- update set 업데이트내용
- delete where 조건
- when not matched then		: 서로 다르다면
	- 조건이 만족하지 않는다면 insert문 실행
- insert values (컬럼이름);

## transcaction 관리하기

# 7장 Constraint 제약 조건 

- 주키 : 개체 무결성 , Not Null , unique
- 외래키 : 참조 무결성, 생략 가능, Null가능
	- 상대 테이블에 존재하는 data를 가져온다. (보통 주키를 가져옴.)
	- 주키가 아닌 키를 가져온다면 키 속성이 unique여야 한다.
<br/>

## 제약조건 정의
- 테이블에 올바른 데이터만 입력받고 잘못된 데이터는 들어오지 못하도록 컬럼마다 정하는 규칙을 의미
	- default는 제약조건 아님
<br/>

## 제약조건 종류
- NOT NULL
- UNIQUE
- PRIMARY KEY			: NOT NULL + UNIQUE
- FOREIGN KEY			: 다른 테이블의 컬럼을 참조해서 검사
- CHECK					: 이 조건에서 설정된 값만 입력을 허용하고 나머지는 거부
	- 테이블에서 컬럼별로 설정
	- PRIMARY KEY제외하고는 중복 설정 가능

## 제약조건 조회하기
- 테이블 명은 꼭 대문자로 입력해야한다.
- `SELECT * FROM ALL_CONSTRAINTS 
WHERE table_name = 'EMP';`

## 제약조건 삭제하기
- ALTER TABLE 테이블명 DROP CONSTRAINT 제약조건명;

## 제약조건  생성하기
- 테이블 생성 할 때 조건 주기
	-  제약조건 이름 주기
		- create table NewEmp1 (<br/>
no Number(4) `Constraint emp1_no_pk` primary key, <br/>
name Varchar(12) `Constraint emp_name_nn` Not Null );
<br/>

	-  제약조건 이름 안주기
		- create table NewEmp1(<br/>
	no Num(4) primary key, <br/>
	name varchar(12) not null <br/> );
	- 제약조건의 이름을 안주면 제약조건을 (비)활성화, 삭제 등의 관리 작업을 못함. 그래서 주는 것을 권장
- 테이블 생성 후 조건 주기
	- 기본 : alter 테이블 명 add Constraint 제약조건별칭 제약조건(컬럼명)
		- `alter table NewEmp1 Add Constraint emp1_nk UNIQUE(name);`
	- Not Null 은 특별하게도 Modify 써야함
		- `alter table NewEmp Modify ( p_no constraint pNo_nn Not NUll);`
	- foreign key 제약조건 생성시, 부모 reference key에 primary key / unique 설정 해줘야함.

## 제약조건 관리하기
- DISABLE
	- UNIQUE INDEX가 자동으로 삭제됨.
	- 기본 옵션은 NOVALIDATE
	- NOVALIDATE : 해당 제약조건이 없는 듯이 행동
		- `ALTER TABLE 테이블명 DISABLE NOVALIDATE CONSTRAINT 제약조건명;`
	- VALIDATE : 해당 제약조건이 없지만 컬럼의 데이터를 변경할 순 없게 한다. 하지만 INSERT, DELETE, ALTER 다 안됨..?????? 왜필요한건가.
		- `ALTER TABLE 테이블명 DISABLE VALIDATE CONSTRAINT 제약조건명;`
		- 테이블의 내용을 변경할 수 없도록
- ENABLE
	- NOVALIDATE
		- ENABLE한 시점부터 새롭게 테이블로 입력되는 데이터만 제약조건을 적용하여 검사
	- VALIDATE
		- 테이블 이미 존재하는 애들 + 새롭게 입력되는 데이터 제약조건 검사

# 8장 INDEX
## 인덱스 종류
- UNIQUE INDEX
	- 속도가 아주 빠름
	- 중복되는 데이터 들어올 수 없다.
	- `create unique index 인덱스명 on 테이블이름(컬럼명1 asc/desc, ... );`
- NON-UNIQUE INDEX
	- 중복되는 데이터가 들어가야할 때
	- `create index 인덱스명 on 테이블명(컬럼명1 asc/desc, ..);`
- Function Based INDEX
	- Index Suppressing Error : 인덱스를 where절에 오는 조건 컬럼이나 조인 컬럼에 만들어야 하는데 이렇게 만들어놓고 sql에서 다른 조건을 사용하거나 하면 인덱스를 사용할 수 없게되는 에러
	- 쿼리 조건이 변경되면 기존의 인덱스를 활용할 수없다.
	- `create index idx_prof_pay_fbi on professor(pay+1000);`
- DESCENDING INDEX
	- desc : 큰값 -> 작은값
	- 날짜 큰값 = 최근
	- `create index idx_prof_pay on professor pay(desc);`
- Composition INDEX
	- 두개 이상의 컬럼을 합쳐서 인덱스를 만드는 것
	- where절의 조인 컬럼이 2개 이상 and로 연결되어 함께 사용되는 경우에 많이 사용
	- 인덱스 순서가 중요함.
	- `create index idx_emp_com on emp (sex, ename);`
- Bitmap INDEX
	- `create bitmap index idx_emp_sex_bit on emp(sex);`
## 인덱스 주의사항
- DML에 취약하다
	- insert
		- index split 현상 : 데이터가 테이블 도중에 들어가야한다면 2개의 블록으로 나뉘어서 잠깐 저장되는 현상. 느려짐.
	- delete
		- 보통 : 그 자리가 비워지고 다른 데이터가 사용가능하다고 말해줌.
		- 인덱스는 사용안된다는 표시만 함. 즉 데이터가 1만건 있어도 인덱스는 10만건 있을수있음.
	- update
		- 인덱스에는 update 없음. 
- 타 SQL 실행에 악영향

## 인덱스 조회하기
-  사용자가 생성한 인덱스 조회 : user_indexes , user_ind_columns
	- `select table_name, column_name, index_name from user_ind_colmuns where table_name = 'EMP1';`
	- `select table_name, index_name from user_indexs where table_name = 'EMP1';`
	- 컬럼명과 from 주의!
	- 결과 값은 동일함.
- 데이터베이스 전체에 생성된 내역을 조회하려면 DBA_INDEXES / DBA_IND_COLUMNS

## 인덱스 모니터링
- 시작하기 `alter index idx_emp_ename monitoring usage;`
- 중단하기 `alter index idx_emp_ename nomonitoring usage;`
- 사용여부 확인 `select index_name, used from v$object_usage where index_name = 'idx_emp_ename';`

## INDEX REBUILD하기
## INVISIBLE INDEX
## Index활용 399pg
- 정렬한 효과
- 최대 최소 찾기
- ROWID
	
# 10장 서브쿼리 438
- 서브쿼리가 먼저 수행되어 결과를 만들고 그 결과값을 메인쿼리에 전달
- 그 반대의 경우도 있음
### 구별
- select (sub query) : 스칼라 서브 쿼리
- from (sub query) : 인라인 뷰
- where (sub query) : 서브쿼리

## Sub Query
- where절 , 괄호로 묶기
- order by 올수없음

### 단일행
- 수행값이 1개의 행
- where절에서 사용되는 연산자  
	- <> 같지않다. 
	- = 
	- >
	- >= 
	- < 
	- <=

### 다중행 
- 수행값에서 행 수가 2개이상
- where절에서 사용되는 연산자 : 
	- IN			: 서브 쿼리와 같은 값을 찾는다.
	- EXISTS		: 서브 쿼리의 값이 있을 때만 메인 쿼리 실행
	- >ANY			: 서브쿼리 결과중에서 최솟값 반환
	- <ANY		: 서브쿼리 결과중에서 최댓값을 반환
	- <ALL			: 서브쿼리 결과중에서 최솟값을 반환
	- >ALL			: 서브쿼리 결과중에서 최댓값을 반환
	- 주의 !
		- SAL > ANY (100, 200, 300) : SAL > 100
		- SAL < ALL(100, 200, 300) : SAL < 100
		- ==ALL은 방향의 반대값을 출력해준다==

## 다중컬럼
- 서브쿼리의 결과가 여러 컬럼인 경우
- 실습해야함???????!???!?!?!???

## 상호연관
- 메인 쿼리 값을 서브쿼리에 주고 서브쿼리를 수행한 후 그 결과를 다시 메인 쿼리로 반환해서 수행하는 서브쿼리

## 스칼라 서브 쿼리
- 2건 이상의 데이터 반환을 요청하는 경우 -- 에러
- 2개 이상의 컬럼을 조회할 경우 -- 에러



<br/> <br/> <br/> <br/> <br/> <br/>
### cascade <-> restrict
연쇄 삭제
`drop table 테이블명 cascade constraint;`
`Alter table emp drop foreign key dept_no references test(dept_no) on delete cascade on update cascade;`

### Delete 조건
- on delete cascade
- on delete set null 		: 부모 테이블의 데이터가 지워질 경우 자식 테이블의 값을 null

### 외래키 주기
- create 시
	- foreign key (생성할테이블의 컬럼명) references 참조할테이블(참조할컬럼명) 
- alter 시
	- alter table 테이블명 add constraint 제약조건명 foreign key(속성명)<br/> references 참조할테이블(참조할컬럼명) on delete cascade;

### 연봉표시
- to_char(pay, '$999,999,999')

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

## DESC_RANK 202p



### SET PAGES 50
50개까지 값 출력

### cascade <-> restrict
연쇄 삭제
`drop table 테이블명 cascade constraint;`
`Alter table emp drop foreign key dept_no references test(dept_no) on delete cascade on update cascade;`

# Delete 조건
- on delete cascade
- on delete set null 		: 부모 테이블의 데이터가 지워질 경우 자식 테이블의 값을 null
