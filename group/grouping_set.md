# 주제
  - grouping sets 와 group 차이
  (이 때, SELECT절 내 컬럼 순서는 A, B 고정 )
## 1.grouping set와 group 차이
### 1.1 일반적 차이
  - GROUP BY A, B
```
select
case grouping(d.department_name) when 1 then 'All departments' else d.department_name end department_name,
case grouping(e.job_id) when 1 then 'All jobs' else e.job_id end job_id,
count(*),
sum(salary)
from employees e JOIN departments d ON e.department_id = d.department_id
where rownum <= 20
group by d.department_name, e.job_id;
```
  - GROUPING SET(A, B)
```
select
case grouping(d.department_name) when 1 then 'All departments' else d.department_name end department_name,
case grouping(e.job_id) when 1 then 'All jobs' else e.job_id end job_id,
count(*),
sum(salary)
from employees e JOIN departments d ON e.department_id = d.department_id
where rownum <= 20
group by GROUPING SETS(d.department_name, e.job_id);
```

### 1.2 차이 발생하지 않는 경우
  - GROUPING SET((A, B))
```
select
case grouping(d.department_name) when 1 then 'All departments' else d.department_name end department_name,
case grouping(e.job_id) when 1 then 'All jobs' else e.job_id end job_id,
count(*),
sum(salary)
from employees e JOIN departments d ON e.department_id = d.department_id
where rownum <= 20
group by GROUPING SETS((d.department_name, e.job_id));
```
