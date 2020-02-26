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
-- case 1
select /* driven_emp UKI_QUERY1 */ 1 from tb_emp where department_id in (select department_id from tb_dept);

-- case 2
create index tb_emp_idx on tb_emp(department_id);
select /* driven_emp UKI_QUERY2 */ 1 from tb_emp where department_id in (select department_id from tb_dept);

-- case 4
create index tb_dept_idx on tb_dept(department_id);
select /* driven_emp UKI_QUERY3 */ 1 from tb_emp where department_id in (select department_id from tb_dept);

-- case 3
drop index tb_emp_idx;
select /* driven_emp UKI_QUERY4 */ 1 from tb_emp where department_id in (select department_id from tb_dept);

-- case 5
select /* driven_emp UKI_QUERY5 */ 1 from tb_dept where department_id in (select department_id from tb_emp);

-- case 6
create index tb_emp_idx on tb_emp(department_id);
select /* driven_emp UKI_QUERY6 */ 1 from tb_dept where department_id in (select department_id from tb_emp);

-- case 8
create index tb_dept_idx on tb_dept(department_id);
select /* driven_emp UKI_QUERY8 */ 1 from tb_dept where department_id in (select department_id from tb_emp);

-- case 7
drop index tb_emp_idx;
select /* driven_emp UKI_QUERY7 */ 1 from tb_dept where department_id in (select department_id from tb_emp);

```
  - liveSQL 테스트 할 경우 index 생성/삭제 시점 주의하면서 case 별로 따로 실행시켰음.
