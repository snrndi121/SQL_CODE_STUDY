# 주제
  - ROLLUP 인수 순서에 따른 차이(내부/외부)

#1. ROLLUP 인수 순서에 따른 차이
#1.1 내부 순서
  - ROLLUP(A), b
```
select
case grouping(d.department_name) when 1 then 'All departments' else d.department_name end department_name,
case grouping(e.job_id) when 1 then 'All jobs' else e.job_id end job_id,
count(*),
sum(salary)
from employees e JOIN departments d ON e.department_id = d.department_id
where rownum <= 20
group by ROLLUP(d.department_name), e.job_id;
```
  - A, ROLLUP(B)
```
select
case grouping(d.department_name) when 1 then 'All departments' else d.department_name end department_name,
case grouping(e.job_id) when 1 then 'All jobs' else e.job_id end job_id,
count(*),
sum(salary)
from employees e JOIN departments d ON e.department_id = d.department_id
where rownum <= 20
group by d.department_name, ROLLUP(e.job_id),;
```

  - ROLLUP(B), A
```
select
case grouping(d.department_name) when 1 then 'All departments' else d.department_name end department_name,
case grouping(e.job_id) when 1 then 'All jobs' else e.job_id end job_id,
count(*),
sum(salary)
from employees e JOIN departments d ON e.department_id = d.department_id
where rownum <= 20
group by ROLLUP(e.job_id), d.department_name;
```
  - B, ROLLUP(A)
```
select
case grouping(d.department_name) when 1 then 'All departments' else d.department_name end department_name,
case grouping(e.job_id) when 1 then 'All jobs' else e.job_id end job_id,
count(*),
sum(salary)
from employees e JOIN departments d ON e.department_id = d.department_id
where rownum <= 20
group by e.job_id, ROLLUP(d.department_name);
```

#1.2 외부 순서
  - ROLLUP(A, B)
```
select
case grouping(d.department_name) when 1 then 'All departments' else d.department_name end department_name,
case grouping(e.job_id) when 1 then 'All jobs' else e.job_id end job_id,
count(*),
sum(salary)
from employees e JOIN departments d ON e.department_id = d.department_id
where rownum <= 20
group by ROLLUP(d.department_name, e.job_id);
```
- ROLLUP(B, A)
```
select
case grouping(d.department_name) when 1 then 'All departments' else d.department_name end department_name,
case grouping(e.job_id) when 1 then 'All jobs' else e.job_id end job_id,
count(*),
sum(salary)
from employees e JOIN departments d ON e.department_id = d.department_id
where rownum <= 20
group by ROLLUP(e.job_id, d.department_name);
```
