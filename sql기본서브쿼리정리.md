# 1. GROUP BY
- 조건은 **HAVING**에서 처리한다.
- select부분은 집계로 나와야함
- group by customer_id 하면, customer_id는 Unique함

# 2. 단일행 서브쿼리
- 부등호 쓴다 <. >, =, <=, >=

  
``` sql
SELECT *
FROM tips
WHERE total_bill > (SELECT AVG(total_bill) FROM tips)
```

# 3. 다중행 서브쿼리
- in, not in 쓴다
- 조건이 하나가 아닌 여러개일 경우
  
``` sql
SELECT *
FROM tips
WHERE day IN ('Sun', 'Sat')

SELECT *
FROM tips
WHERE day IN (
  SELECT day
  FROM tips
  GROUP BY day
  HAVING SUM(total_bill) >= 1500
) -- 서브쿼리의 결과값은 컬럼 1개, 로우 N개
```

# 4. 다중컬럼서브쿼리
- 원하는 조건의 행들을 모든 열을 뽑아 오고 싶을때
``` sql
SELECT *
FROM tips
WHERE (day, total_bill) IN (
  SELECT day, MAX(total_bill)
  FROM tips
  GROUP BY day
)
```

# 5. FROM 서브쿼리
- 테이블을 새로 만든다고 생각하기
- WITH 또는 서브쿼리 편한대로 사용하기
  - WITH (일시적으로 만드는 것임): WITH 테이블명 AS (서브쿼리)
  - 서브쿼리: 무조건 as 테이블명 써야함

```sql
SELECT ROUND(AVG(daily.sales), 2) daily_sales_avg
FROM (
  SELECT day
       , SUM(total_bill) sales
  FROM tips
  GROUP BY day
) AS daily


WITH daily AS (
  SELECT day
       , SUM(total_bill) sales
  FROM tips
  GROUP BY day
)

SELECT ROUND(AVG(daily.sales), 2) daily_sales_avg
FROM daily
```

# 6.JOIN _ 이부분 더 공부해서 수정하기
- INNER JOIN
- OUTER JOIN
``` sql
WITH totalbill_by_day AS (
  SELECT
    SUM(total_bill) total_bill_sum,
    day
  FROM tips
  GROUP BY day)

SELECT
  t.day,
  t.total_bill,
  round(t.total_bill/td.total_bill_sum*100,2) pct
FROM 
  tips t 
INNER JOIN totalbill_by_day td
  ON td.day = t.day
ORDER BY
  t.total_bill DESC;
```


# 상관쿼리
- 자기자신을 또 다른 테이블로 만드는 쿼리
- 요새는 window함수가 있어서 거의 사용하지 않으나 코테 문제에 간혹 나오는 경우가 있어 숙지가 필요함
  - 이동평균: DATE_ADD(기준열, INTERVAL 1 DAY), DATE_SUB(기준열, INTERVAL 1 DAY)
  - 누적합계산하기:
    
``` sql
-- 이동평균
SELECT 
  measured_at,
  pm10,
  (SELECT AVG(m2.pm10) FROM measurements m2 WHERE m2.measured_at BETWEEN DATE_SUB(m1.measured_at, INTERVAL 1 DAY) AND DATE_ADD(m1.measured_at, INTERVAL 1 DAY)) AS pm10_runnung_average
FROM measurements m1 
-- 행 하나만 생각하기

-- 누적합계산하기
SELECT 
  measured_at,
  pm10,
  (SELECT SUM(m2.pm10) FROM measurements m2 WHERE m2.measured_at <= m1.measured_at) AS pm10_runnung_total
FROM measurements m1 
-- 행 하나만 생각하기
```

=============================================================


## 예시 서브쿼리
``` sql
-- Q. 전체 total_bill 합에서 total_bill의 퍼센트 구하기

SELECT 
  day,
  total_bill,
  round(total_bill / (SELECT SUM(total_bill) FROM tips)*100,2) AS pct 
FROM tips
ORDER BY total_bill DESC;
```
``` sql
-- Q. 각 사이즈별로 가장 큰 금액 추출
SELECT
  *
FROM
  tips
WHERE
  (size, total_bill) IN (
    SELECT
      size,
      MAX(total_bill)
    FROM
      tips
    GROUP BY
      size
  )
ORDER BY
  size;
```

## 이외의 문법
- 조건
  - IF (조건, TRUE일 경우 반환, False일 경우 반환)
  - CASE WHEN THEN END
 
    
``` sql
(CASE WHEN time = 'Lunch' THEN total_bill END) lunch
IF(time = 'Dinner', total_bill, null) dinner 
```

- 날짜 포맷
  - DATE() --> 무조건 2017-01-01 이런식으로 반환됨
  - DATE_FORMAT( 입력, '%Y-%m-%d)

- NULL
  - IS NOT NULL: null 값이 아닌거

- 날짜 사이 뽑을 때
  - BETWEEN A and B

