# 주제
  - 정렬 연산 대체 가능한 경우


## 1.정렬 연산 대체 가능한 경우
  - 인덱스의 선행 컬럼 순서대로 order by 기술될 경우에만.
  - 예외적으로, 선행 컬럼을 '='로 비교하면 order by 에서 생략하거나 순서가 바뀌는 경우라도 대체 가능
  - 현재 composite index : [ c1, c2 ]
```
# normal
select /* where, order : full-col UQ40 */ * from tb_sam where c1 = 3 and c2 < 500 order by c1, c2;
select /* where : pre-col, order by: pre-col UQ41 */* from tb_sam where c1 < 3 order by c1;

# exceptional
select /* replaced UQ52 */* from tb_sam where c1 = 3 order by c2;
select /* replaced UQ53 */* from tb_sam where c1 < 3 order by c2, c1;
select /* replaced UQ54 */* from tb_sam where c1 = 3 order by c2, c1;
select /* not replaced UQ50 */* from tb_sam where c1 = 3 and c2 < 500 order by c2, c1;
select /* not replaced UQ51 */* from tb_sam where c1 < 3 order by c2;
select /* not replaced UQ55 */* from tb_sam where c2 = 3 order by c1
select /* not replaced UQ56 */* from tb_sam where c2 = 500 order by c2, c1;
```
