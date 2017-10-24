# SQL summary

#### SELECT FROM
```SQL
SELECT * FROM sample1;
```
- sample1 테이블의 모든 열을 출력

```SQL
SELECT col1, col2 FROM sample1;
```
- sample1 테이블의 특정 열만을 추출

#### WHERE
```SQL
SELECT * FROM sample1 WHERE no=2;
```
- no 값이 2인 행만 추출

```SQL
SELECT * FROM sample1 WHERE no<>2;
```
- no 값이 2가 아닌 행만 추출

```SQL
SELECT * FROM sample1 WHERE name IS NULL;
```
- name 값이 NULL인 행만 추출

```SQL
SELECT * FROM sample1 WHERE name IS NOT NULL;
```
- name 값이 NULL이 아닌 행만 추출

```SQL
SELECT * FROM sample1 WHERE no=12 AND name='Kim';
```
- no 값이 12이며 name이 'Kim'인 행만 추출

```SQL
SELECT * FROM sample1 WHERE no<12 OR no>100;
```
- no 값이 12보다 작거나 100보다 큰 행만 추출

```SQL
SELECT * FROM sample1 WHERE a=1 OR a=2 AND b=1 OR b=2;
```
- WHERE a=1 OR (a=2 AND b=1) OR b=2
- 우선순위: AND > OR (AND를 먼저 탐색)

```SQL
SELECT * FROM sample1 WHERE (a=1 OR a=2) AND (b=1 OR b=2);
```
- WHERE (a=1 OR a=2) AND (b=1 OR b=2)
- 괄호 안의 조건을 먼저 탐색

```SQL
SELECT * FROM sample1 WHERE NOT(a<>0 OR b<>0);
```
- a가 0이 아니거나 b가 0이 아닌 행을 제외한 나머지를 검색

#### LIKE

```SQL
SELECT * FROM sample1 WHERE text LIKE 'ABC%';
```
- text 열에서 ABC로 시작하는 행을 추출

```SQL
SELECT * FROM sample1 WHERE text LIKE '%ABC%';
```
- text 열에서 중간에 ABC가 포함된 행을 추출

```SQL
SELECT * FROM sample1 WHERE text LIKE '%ABC';
```
- text 열에서 ABC로 끝나는 행을 추출

```SQL
WHERE text LIKE '%\%%';
```
- % 검색


#### ORDER BY

```SQL
SELECT * FROM sample1 ORDER BY col1;
```
- col1을 기준으로 하여 오름차순 정렬

```SQL
SELECT * FROM sample1 ORDER BY col1 ASC;
```
- ASC: 오름차순 정렬

```SQL
SELECT * FROM sample1 ORDER BY col1 DESC;
```
- DESC: 내림차순 정렬

```SQL
SELECT * FROM sample1 ORDER BY col1 DESC, col2 ASC;
```
- col1은 내림차순, col2은 오름차순 정렬

#### LIMIT

표준 명령은 아님(Mysql, PostgreSQL에서 사용 가능)

```SQL
SELECT * FROM sample1 LIMIT 3;
```
- 상위 3건만 추출

```SQL
SELECT * FROM sample1 LIMIT 3 OFFSET 3;
```
- 4번째 행부터 상위 3건만 추출

#### 수치연산

```SQL
SELECT *, price*quantity AS amount FROM sample1;
```
- 모든 열 + amount열(price*quantity)을 출력

```SQL
SELECT *, price*quantity AS amount FROM sample1 WHERE price*quantity >= 1000;
```
- WHERE에서 별명(amount) 사용 불가
- WHERE -> SELECT 순서로 처리되기 때문

```SQL
SELECT *, price*quantity AS amount FROM sample1 ORDER BY amount DESC;
```
- ORDER BY에서 별명(amount) 사용 가능
- WHERE -> SELECT -> ORDER BY 순서로 처리되기 때문

```SQL
SELECT  amount, ROUND(amount) FROM sample1;
```
- ROUND(열): 소수점 이하를 반올림(ex: 9521.5 -> 9522)

```SQL
SELECT  amount, ROUND(amount, 1) FROM sample1;
```
- 소수점 둘째자리를 반올림(ex:4421.46 -> 4421.5)

```SQL
SELECT  amount, ROUND(amount, -2) FROM sample1;
```
- 둘째자리를 반올림(ex: 6543 -> 6500)

#### 문자열 연산

#### 날짜 연산

#### CASE

#### INSERT

#### DELETE

#### UPDATE

```
UPDATE sample1 SET b='2014-09-07' WHERE no=2;
```
- no=2 행의 b값을 2014-09-07로 업데이트

```
UPDATE sample1 SET b=NULL;
```
- NULL 초기화

#### COUNT

```SQL
SELECT COUNT(*) FROM sample1;
SELECT COUNT(name), COUNT(email) FROM sample1 WHERE name='A';
```
- 행 개수 계산

```SQL
SELECT DISTINCT name FROM sample1;
```
- 중복 제거

```SQL
SELECT ALL name FROM sample1;
```
- 중복 유지

```SQL
SELECT COUNT(DISTINCT name) FROM sample1;
```
- 중복 제거 후 행 개수 계산

#### COUNT 이외의 집계함수
```SQL
SUM([ALL|DISTINCT] col)
AVG([ALL|DISTINCT] col)
MIN([ALL|DISTINCT] col)
MAX([ALL|DISTINCT] col)
```

#### GROUP BY
```SQL
SELECT * FROM sample1 GROUP BY blood_type;
```
- GROUP BY를 통해 열을 그룹화

```SQL
SELECT name, COUNT(name), SUM(quantity) FROM sample1 GROUP BY name;
```
- GROUP BY와 집계함수의 조합

```SQL
SELECT name, COUNT(name) FROM sample1 GROUP BY name HAVING COUNT(name)=1;
```
- GROUP BY 내에서 HAVING을 이용해 조건식을 지정

#### 서브쿼리
```SQL
DELETE FROM sample1 where a=(SELECT MIN(a) FROM sample1;
```
- DELETE FROM sample1 where a=10; 과 같음.

```SQL
SELECT  
	(SELECT COUNT(*) FROM sample1) AS sq1  
    (SELECT COUNT(*) FROM sample2) AS sq2;
```
- SELECT 구에서 서브쿼리 사용하기

```SQL
UPDATE sample1 SET a=(SELECT MAX(a) FROM sample1);
```
- SET 구에서 서브쿼리 사용하기

```SQL
SELECT * FROM (SELECT * FROM sample1) sq;
SELECT * FROM (SELECT * FROM sample1) AS sq;
```
- FROM 구에서 서브쿼리 사용하기

#### 상관 서브쿼리
```SQL
UPDATE sample1 SET a='Yes' WHERE
	EXISTS (SELECT * FROM sample2 WHERE no2 = no);
```
- 상관 서브쿼리: 부모 명령(UPDATE문)과 자식인 서브쿼리(괄호로 묶인 부분)가 특정 관계를 맺는 것
- (SELECT * FROM sample2 WHERE no2 = no)의 조건을 만족하는 겨우에만 UPDATE문을 실행
- no2: sample2의 열, no: sample1의 열

```SQL
UPDATE sample1 SET a='Yes' WHERE
	NOT EXISTS (SELECT * FROM sample2 WHERE no2 = no);
```
- (SELECT * FROM sample2 WHERE no2 = no)의 조건을 만족하지 않는 겨우에만 UPDATE문을 실행

```SQL
SELECT * FROM sample1 WHERE no IN (SELECT no2 FROM sample2);
```
- IN을 이용하여 집합 안에 값이 존재하는지를 조사


