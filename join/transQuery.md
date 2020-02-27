# 주제
  - 서브쿼리 unnesting, 서브쿼리에 PK/FK 또는 Unique 인덱스 없는 경우

## 1.서브쿼리 unnesting
  - 기본 세팅
```
create table tb_emp
as select employee_id, department_id
from employees;

create table tb_dept
as select department_id, department_name
from departments;

/* M */
select count(*) from tb_emp;
/* 1 */
select count(*) from tb_dept;
```

### 1.1 서브쿼리에 PK/FK 또는 Unique 인덱스 없는 경우
```
-- case 1, 5 : 메인/서브 쿼리의 조인 컬럼에 대한 인덱스가 모두 없는 경우
-- drop index tb_emp_idx;
-- drop index tb_dept_idx;

-- case 2, 6 : 메인 쿼리의 조인 컬럼에 대한 인덱스만 있는 경우
-- create index tb_emp_idx on tb_emp(department_id);
-- drop index tb_dept_idx;

-- case 4, 8 : 메인/서브 쿼리의 조인 컬럼에 대한 인덱스가 모두 있는 경우
-- create index tb_emp_idx on tb_emp(department_id);
-- create index tb_dept_idx on tb_dept(department_id);

-- case 3, 7 : 서브 쿼리의 조인 컬럼에 대한 인덱스만 있는 경우
-- drop index tb_emp_idx;
-- create index tb_dept_idx on tb_dept(department_id);

select /* driven_emp UKI_QUERY */ /*+ unnest */ 1 from tb_emp where department_id in (select department_id from tb_dept);

select /* driven_emp UKI_QUERY */ /*+ unnest */ 1 from tb_dept where department_id in (select department_id from tb_emp);

```
  - liveSQL 테스트 할 경우 index 생성/삭제 시점 주의하면서 case 별로 따로 실행시켰음.
