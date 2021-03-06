# 주제
  - 외부 조인(outer join)과 같은 표현
  - 외부 조인에 대한 조인 조건 Push Down

## 1.외부 조인과 같은 표현

  - (+)식 표현
```
select /* UQ10*/ d.deptino, d.dname, e.avg_sal from tb_dept d, (select deptino, avg(sal) avg_sal from tb_emp group by deptino) e where e.deptino (+) = d.deptino;
```

  - OUTER JOIN식 표현
```
select /*UQ11*/d.deptino, d.dname, e.avg_sal from tb_dept d LEFT OUTER JOIN (select deptino, avg(sal) avg_sal from tb_emp group by deptino) e ON e.deptino = d.deptino;
```

  - 서브쿼리 식 표현
```
select /*UQ12*/d.deptino, d.dname, (select trunc(avg(sal), 2) from tb_emp e where deptino = d.deptino) from tb_dept d;
```

## 2.조건절 Pushing 중 조인조건 Push Down 넣어줌
```
select /* UQ55*/ /*+ no_merge(e) push_pred(e) */  d.deptino, d.dname, e.avg_sal from tb_dept d, (select deptino, avg(sal) avg_sal from tb_emp group by deptino) e where e.deptino (+) = d.deptino;
```
