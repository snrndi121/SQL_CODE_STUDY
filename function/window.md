# 주제
  - windowing 내 헷갈리는 몇가지 구문

## 1. windowing 내 헷갈리는 몇가지 구문
### 1.1 rows와 range 차이

- [ rows | range ] between 1 preceding and 1 following
```
SELECT department_id, employee_id, FIRST_VALUE(employee_id)OVER(PARTITION BY department_id ORDER BY salary rows between 1 preceding and 1 following) max_sal
FROM employees
WHERE department_id IN (30, 50);    
```
```
SELECT department_id, employee_id, FIRST_VALUE(employee_id)OVER(PARTITION BY department_id ORDER BY salary range between 1 preceding and 1 following) max_sal
FROM employees
WHERE department_id IN (30, 50);
```   
