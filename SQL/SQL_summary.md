# SQL summary

#### SELECT FROM
`SELECT * FROM sample1;`
- sample1 테이블의 모든 열을 출력

`SELECT col1, col2 FROM sample1;`
- sample1 테이블의 특정 열만을 추출

#### WHERE
`SELECT * FROM sample1 WHERE no=2;`
- no 값이 2인 행만 추출

`SELECT * FROM sample1 WHERE no<>2;`
- no 값이 2가 아닌 행만 추출

`SELECT * FROM sample1 WHERE name IS NULL;`
- name 값이 NULL인 행만 추출

`SELECT * FROM sample1 WHERE name IS NOT NULL;`
- name 값이 NULL이 아닌 행만 추출

`SELECT * FROM sample1 WHERE no=12 AND name='Kim';`
- no 값이 12이며 name이 'Kim'인 행만 추출

`SELECT * FROM sample1 WHERE no<12 OR no>100;`
- no 값이 12보다 작거나 100보다 큰 행만 추출

`SELECT * FROM sample1 WHERE a=1 OR a=2 AND b=1 OR b=2;`
- WHERE a=1 OR (a=2 AND b=1) OR b=2
- 우선순위: AND > OR (AND를 먼저 탐색)

`SELECT * FROM sample1 WHERE (a=1 OR a=2) AND (b=1 OR b=2);`
- WHERE (a=1 OR a=2) AND (b=1 OR b=2)
- 괄호 안의 조건을 먼저 탐색

`SELECT * FROM sample1 WHERE NOT(a<>0 OR b<>0);`
- a가 0이 아니거나 b가 0이 아닌 행을 제외한 나머지를 검색

#### LIKE

`SELECT * FROM sample1 WHERE text LIKE 'ABC%';`
- text 열에서 ABC로 시작하는 행을 추출

`SELECT * FROM sample1 WHERE text LIKE '%ABC%';`
- text 열에서 중간에 ABC가 포함된 행을 추출

`SELECT * FROM sample1 WHERE text LIKE '%ABC';`
- text 열에서 ABC로 끝나는 행을 추출

`WHERE text LIKE '%\%%';`
- % 검색


#### ORDER BY

`SELECT * FROM sample1 ORDER BY col1;`
- col1을 기준으로 하여 오름차순 정렬

`SELECT * FROM sample1 ORDER BY col1 ASC;`
- ASC: 오름차순 정렬

`SELECT * FROM sample1 ORDER BY col1 DESC;`
- DESC: 내림차순 정렬

`SELECT * FROM sample1 ORDER BY col1 DESC, col2 ASC;`
- col1은 내림차순, col2은 오름차순 정렬

#### LIMIT

표준 명령은 아님(Mysql, PostgreSQL에서 사용 가능)

`SELECT * FROM sample1 LIMIT 3;`
- 상위 3건만 추출

`SELECT * FROM sample1 LIMIT 3 OFFSET 3;`
- 4번째 행부터 상위 3건만 추출

#### 수치연산

`SELECT *, price*quantity AS amount FROM sample1;`
- 모든 열 + amount열(price*quantity)을 출력

`SELECT *, price*quantity AS amount FROM sample1 WHERE price*quantity >= 1000;`
- WHERE에서 별명(amount) 사용 불가
- WHERE -> SELECT 순서로 처리되기 때문

`SELECT *, price*quantity AS amount FROM sample1 ORDER BY amount DESC;`
- ORDER BY에서 별명(amount) 사용 가능
- WHERE -> SELECT -> ORDER BY 순서로 처리되기 때문

`SELECT  amount, ROUND(amount) FROM sample1;`
- ROUND(열): 소수점 이하를 반올림(ex: 9521.5 -> 9522)

`SELECT  amount, ROUND(amount, 1) FROM sample1;`
- 소수점 둘째자리를 반올림(ex:4421.46 -> 4421.5)

`SELECT  amount, ROUND(amount, -2) FROM sample1;`
- 둘째자리를 반올림(ex: 6543 -> 6500)

#### 문자열 연산

#### 날짜 연산

#### CASE

#### INSERT

#### DELETE

#### UPDATE

#### COUNT

`SELECT COUNT(*) FROM sample1;`
`SELECT COUNT(name), COUNT(email) FROM sample1 WHERE name='A';`
- 행 개수 계산

`SELECT DISTINCT name FROM sample1;`
- 중복 제거

`SELECT ALL name FROM sample1;`
- 중복 유지

`SELECT COUNT(DISTINCT name) FROM sample1;`
- 중복 제거 후 행 개수 계산

#### COUNT 이외의 집계함수
`SUM([ALL|DISTINCT] col)`
`AVG([ALL|DISTINCT] col)`
`MIN([ALL|DISTINCT] col)`
`MAX([ALL|DISTINCT] col)`

#### GROUP BY
`SELECT * FROM sample1 GROUP BY blood_type;`
- GROUP BY를 통해 열을 그룹화

`SELECT name, COUNT(name), SUM(quantity) FROM sample1 GROUP BY name;`
- GROUP BY와 집계함수의 조합



