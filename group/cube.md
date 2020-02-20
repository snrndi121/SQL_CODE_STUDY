# 주제
  - cube와 같은 표현(rollup, GROUP BY)

# 1.cube와 같은 표현
  - GROUP BY
```
SELECT d.department_name, e.job_id, count(*), sum(salary)
FROM employees e JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name, e.job_id
UNION ALL
SELECT 'All departments', e.job_id, count(*), sum(salary)
FROM employees e JOIN departments d ON e.department_id = d.department_id
GROUP BY e.job_id
UNION ALL
SELECT d.department_name, 'All jobs', count(*), sum(salary)
FROM employees e JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name
UNION ALL
SELECT 'All departments', 'All jobs', count(*), sum(salary)
FROM employees e JOIN departments d ON e.department_id = d.department_id;
```
  - rollup 활용
```
SELECT
CASE grouping(d.department_name) WHEN 1 THEN 'All departments' ELSE d.department_name END department_name,
CASE grouping(e.job_id) WHEN 1 THEN 'All jobs' ELSE e.job_id END job_id,
count(*),
sum(salary)
FROM employees e JOIN departments d ON e.department_id = d.department_id
GROUP BY ROLLUP(d.department_name, e.job_id)
UNION ALL
SELECT
'All departments', e.job_id, count(*), sum(salary)
FROM employees e JOIN departments d ON e.department_id = d.department_id
GROUP BY e.job_id;
```
