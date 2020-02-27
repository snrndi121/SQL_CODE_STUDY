# 주제
  - Hash 조인시 실행 계획 with '=' 이외의 연산자

## 1. Hash 조인시 실행 계획 탐구
### 1.1 '=' 연산자 존재
  - '='만
```
-- select * /* UKI_QUERY */
-- from tb_emp x
-- where exists (select 'x' from tb_dept y where x.deptino = y.deptino);
```    

  - 서브 쿼리 내부의 Where 절 존재 서브 쿼리 컬럼 (uncorralated)
```
-- select * /* UKI_QUERY1 */
-- from tb_emp x
-- where exists (select 'x' from tb_dept y where x.deptino = y.deptino and y.dname like '%A%');
```    

  - 서브 쿼리 내부의 Where 절에 메인 쿼리 컬럼 존재(corralated)
  - 메인 쿼리 내부의 Where 절에 메인 쿼리 컬럼 존재
```    
-- select * /* UKI_QUERY2 */
-- from tb_emp x
-- where exists (select 'x' from tb_dept y where x.deptino = y.deptino and x.sal >= 5000);

-- select * /* UKI_QUERY2 */
-- from tb_emp x
-- where exists (select 'x' from tb_dept y where x.deptino = y.deptino)
-- and x.sal >= 5000;
```

### 1.2 '='연산자 제외
- Exists -> In
```
select * /* UKI_QUERY5 */
from tb_emp x
where (x.deptino, 'Marketing') in (select y.deptino, y.dname from tb_dept y);
```

  - 서브 쿼리 내부의 Where 절 존재 서브 쿼리 컬럼 (uncorralated)
```
select * /* UKI_QUERY4 */
from tb_emp x
where exists (select 'x' from tb_dept y where y.lid between 1000 and 2000)
and x.sal >= 5000;
```

  - 서브 쿼리 내부의 Where 절에 메인 쿼리 컬럼 존재(corralated)
```
select * /* UKI_QUERY3 */
from tb_emp x
where exists (select 'x' from tb_dept y where x.deptino <= 500);
```
