# DAU, WAU, MAU
- DAU (Daily Active User): 일간 활성 고객 수
- WAU (Weekly Active User): 주간 활성 고객 수
- MAU (Monthly Active User): 월간 활성 고객 수


## Stickiness = DAU / WAU or MAU

``` sql
SELECT d.order_date AS dt
     , COUNT(DISTINCT d.customer_id) AS dau
     , COUNT(DISTINCT w.customer_id) AS wau
     , ROUND(COUNT(DISTINCT d.customer_id) / COUNT(DISTINCT w.customer_id), 2) AS stickiness
FROM records AS d
LEFT JOIN records AS w 
  ON w.order_date BETWEEN DATE_ADD(d.order_date, INTERVAL -6 DAY) AND d.order_date
WHERE d.order_date BETWEEN '2020-11-01' AND '2020-11-30'
GROUP BY d.order_date
ORDER BY dt
```
