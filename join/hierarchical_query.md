# 주제
  - prior 순서에 따른 차이

## 1. prior 순서에 따른 차이
  - 답 : 차이없음
  - prior 자식 = 부모
```
select level, sys_connect_by_path(first_name, '/'), job_id, manager_id
from employees
start with manager_id is null
connect by prior employee_id = manager_id
order siblings by first_name; //같은 level이라면 first_name 속성으로 정렬하겠다; 생략가능
```

  - 부모 = prior 자식
```
select level, sys_connect_by_path(first_name, '/'), job_id, manager_id
from employees
start with manager_id is null
connect by manager_id = prior employee_id
order siblings by first_name;
```
