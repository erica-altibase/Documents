<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Altibase 7.1.0.5.4 Patch Notes](#altibase-71054-patch-notes)
  - [Fixed Bugs](#fixed-bugs)
    - [BUG-48761 온라인 로그파일 open() 실패 시 Altibase 서버가 비정상 종료합니다.](#bug-48761%C2%A0%EC%98%A8%EB%9D%BC%EC%9D%B8-%EB%A1%9C%EA%B7%B8%ED%8C%8C%EC%9D%BC-open-%EC%8B%A4%ED%8C%A8-%EC%8B%9C-altibase-%EC%84%9C%EB%B2%84%EA%B0%80-%EB%B9%84%EC%A0%95%EC%83%81-%EC%A2%85%EB%A3%8C%ED%95%A9%EB%8B%88%EB%8B%A4)
    - [BUG-48792 SELECT DISTINCT 와 GROUP BY CUBE 절이 뷰에서 사용될 경우 결과 오류가 발생합니다.](#bug-48792%C2%A0select-distinct-%EC%99%80-group-by-cube-%EC%A0%88%EC%9D%B4-%EB%B7%B0%EC%97%90%EC%84%9C-%EC%82%AC%EC%9A%A9%EB%90%A0-%EA%B2%BD%EC%9A%B0-%EA%B2%B0%EA%B3%BC-%EC%98%A4%EB%A5%98%EA%B0%80-%EB%B0%9C%EC%83%9D%ED%95%A9%EB%8B%88%EB%8B%A4)
    - [BUG-48800 FORM 절에 파티션을 지정하고 WHERE 절에서 파티션 키 범위가 아닌 조건을 사용한 SELECT 문에서 결과 오류가 발생할 수 있습니다.](#bug-48800%C2%A0form-%EC%A0%88%EC%97%90-%ED%8C%8C%ED%8B%B0%EC%85%98%EC%9D%84-%EC%A7%80%EC%A0%95%ED%95%98%EA%B3%A0-where-%EC%A0%88%EC%97%90%EC%84%9C-%ED%8C%8C%ED%8B%B0%EC%85%98-%ED%82%A4-%EB%B2%94%EC%9C%84%EA%B0%80-%EC%95%84%EB%8B%8C-%EC%A1%B0%EA%B1%B4%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%9C-select-%EB%AC%B8%EC%97%90%EC%84%9C-%EA%B2%B0%EA%B3%BC-%EC%98%A4%EB%A5%98%EA%B0%80-%EB%B0%9C%EC%83%9D%ED%95%A0-%EC%88%98-%EC%9E%88%EC%8A%B5%EB%8B%88%EB%8B%A4)
  - [Changes](#changes)
    - [Version Info](#version-info)
    - [프로퍼티](#%ED%94%84%EB%A1%9C%ED%8D%BC%ED%8B%B0)
    - [성능 뷰](#%EC%84%B1%EB%8A%A5-%EB%B7%B0)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


Altibase 7.1.0.5.4 Patch Notes
==============================

Fixed Bugs
----------

###BUG-48746 ALTER REPLICATION \~ SYNC 수행 시 altibase\_rp\_conflict.log 에 ERR-11089(errno=62) Invalid column size 에러가 발생하며 Altibase 서버가 비정상 종료합니다.

-   **module** : sm

-   **Category** : Fatal

-   **재현 빈도** : Always

-   **증상** : ALTER REPLICATION \~ SYNC 수행 시
    altibase\_rp\_conflict.log 에 ERR-11089(errno=62) Invalid column
    size 에러가 발생하며 Altibase 서버가 비정상 종료하는 현상을
    수정하였습니다.

-   **재현 방법**

    -   **재현 절차**

            아래 조건을 모두 만족하는 경우 발생할 수 있습니다. 
            1) Altibase 7.1.0.1.2 이상
            2) 메모리 테이블
            3) VARIABLE 컬럼을 포함한 이중화 대상 테이블
            4) ALTER REPLICATION ~ SYNC 수행

    -   **수행 결과**

            Altibase 서버 비정상 종료

    -   **예상 결과**

            ALTER REPLICATION ~ SYNC 정상 수행

-   **Workaround**

        VARIABLE 컬럼을 FIXED 로 변경

-   **변경사항**

    -   Performance view
    -   Property
    -   Compile Option
    -   Error Code

### BUG-48761 온라인 로그파일 open() 실패 시 Altibase 서버가 비정상 종료합니다.

-   **module** : sm

-   **Category** : Fatal

-   **재현 빈도** : Rare

-   **증상** : 온라인 로그파일을 open() 할 때 파일시스템 용량 부족 또는
    open file 수 제한으로 실패할 경우 트레이스 로그 파일에 기록
    후 성공할 때까지 재시도 하도록 수정하였습니다. 그 외의 이유로 open()
    이 실패할 경우 기존처럼 동작합니다.

-   **재현 방법**

    -   **재현 절차**

    -   **수행 결과**

    -   **예상 결과**

-   **Workaround**

-   **변경사항**

    -   Performance view
    -   Property
    -   Compile Option
    -   Error Code

### BUG-48792 SELECT DISTINCT 와 GROUP BY CUBE 절이 뷰에서 사용될 경우 결과 오류가 발생합니다.

-   **module** : qp-dml-execute

-   **Category** : Functional Error

-   **재현 빈도** : Always

-   **증상** : SELECT DISTINCT 와 GROUP BY CUBE 절이 뷰에서 사용될 경우
    결과 오류가 발생하는 현상을 개선하였습니다.

-   **재현 방법**

    -   **재현 절차**

            drop table t1;
            create table t1( i1 integer, i2 integer, i3 integer, i4 integer );
            insert into t1 values (1,1,1,1);
            insert into t1 values (1,1,1,2);
            insert into t1 values (1,1,2,3);
            insert into t1 values (1,2,2,4);
            insert into t1 values (2,2,3,5);
            insert into t1 values (2,2,3,6);
            insert into t1 values (2,3,4,7);
            insert into t1 values (2,3,4,8);
            insert into t1 values (3,1,1,1);
            insert into t1 values (3,1,1,2);
            select * from ( select /*+*/ distinct i1, count( i2) from t1 group by cube(i1) ) ;

    -   **수행 결과**

            I1          COUNT( I2)------------------------------------3           103           43           43           24 rows selected.

    -   **예상 결과**

            I1          COUNT( I2)------------------------------------            101           42           43           24 rows selected.

-   **Workaround**

        group by grouping sets 사용

-   **변경사항**

    -   Performance view
    -   Property
    -   Compile Option
    -   Error Code

### BUG-48800 FORM 절에 파티션을 지정하고 WHERE 절에서 파티션 키 범위가 아닌 조건을 사용한 SELECT 문에서 결과 오류가 발생할 수 있습니다.

-   **module** : qp-dml-pvo

-   **Category** : Functional Error

-   **재현 빈도** : Frequence

-   **증상** : FORM 절에 파티션을 지정하고 WHERE 절에서 파티션 키 범위가
    아닌 조건을 사용한 SELECT 문에서 결과 오류가 발생할 수 있습니다.

-   **재현 방법**

    -   **재현 절차**

            drop table t1;
            create table t1 (i1 int, i2 int)
                 partition by range(i1)
                 (
                     partition p1 values less than (100),
                     partition p2 values less than (200),
                     partition p3 values default
                 );
            insert into t1 values ( 130, 100 );
            select /*+ no_exec_fast */ * from t1 partition(p1) where i1 = 130;
            alter table t1 merge partitions p1, p2 INTO PARTITION p1;
            select /*+ no_exec_fast */ * from t1 partition(p1) where i1 = 130;

    -   **수행 결과**

            -- 첫 select 쿼리
            T1.I1       T1.I2
            ---------------------------
            No rows selected.
            ------------------------------------------------------------
            PROJECT ( COLUMN_COUNT: 2, TUPLE_SIZE: 8, COST: BLOCKED )
             PARTITION-COORDINATOR ( TABLE: SYS.T1, PARTITION: 0/3, FULL SCAN, ACCESS: 0, COST: BLOCKED )
            ------------------------------------------------------------
            -- merge 후 두 번째 select 쿼리
            T1.I1       T1.I2
            ---------------------------
            No rows selected.
            ------------------------------------------------------------
            PROJECT ( COLUMN_COUNT: 2, TUPLE_SIZE: 8, COST: BLOCKED )
             PARTITION-COORDINATOR ( TABLE: SYS.T1, PARTITION: 0/3, FULL SCAN, ACCESS: 0, COST: BLOCKED )
            ------------------------------------------------------------

    -   **예상 결과**

            -- 첫 select 쿼리
            T1.I1       T1.I2
            ---------------------------
            No rows selected.
            ------------------------------------------------------------
            PROJECT ( COLUMN_COUNT: 2, TUPLE_SIZE: 8, COST: BLOCKED )
             PARTITION-COORDINATOR ( TABLE: SYS.T1, PARTITION: 0/3, FULL SCAN, ACCESS: 0, COST: BLOCKED )
            ------------------------------------------------------------
            -- merge 후 두 번째 select 쿼리
            T1.I1       T1.I2
            ---------------------------
            130         100
            1 row selected.
            ------------------------------------------------------------
            PROJECT ( COLUMN_COUNT: 2, TUPLE_SIZE: 8, COST: BLOCKED )
             PARTITION-COORDINATOR ( TABLE: SYS.T1, PARTITION: 1/2, FULL SCAN, ACCESS: 1, COST: BLOCKED )
              SCAN ( PARTITION: P1, FULL SCAN, ACCESS: 1, COST: BLOCKED )
            ------------------------------------------------------------

-   **Workaround**

        no_plan_cache hint

-   **변경사항**

    -   Performance view
    -   Property
    -   Compile Option
    -   Error Code

Changes
-------

### Version Info

  altibase version   database binary version   meta version   cm protocol version   replication protocol version   sharding version
------------------ ------------------------- -------------- --------------------- ------------------------------ ------------------
  7.1.0.1.8          6.5.1                     8.7.1          7.1.6                 7.4.4                          2.1.0

> Altibase 7.1 패치 버전별 히스토리는
> [Version\_Histories](https://github.com/ALTIBASE/Documents/blob/master/PatchNotes/Altibase_7_1_Version_Histories.md)
> 에서 확인할 수 있다.

### 호환성

#### Database binary version

데이터베이스 바이너리 버전은 변경되지 않았다.

> 데이터베이스 바이너리 버전은 데이터베이스 이미지 파일과 로그파일의
> 호환성을 나타낸다. 이 버전이 다른 경우의 패치(업그레이드 포함)는
> 데이터베이스를 재구성해야 한다.

#### Meta Version

메타 버전은 변경되지 않았다.

> 패치를 롤백하려는 경우,
> [메타다운그레이드](https://github.com/ALTIBASE/Documents/blob/master/Manuals/Altibase_7.1/kor/Installation.md#%EB%A9%94%ED%83%80-%EB%8B%A4%EC%9A%B4%EA%B7%B8%EB%A0%88%EC%9D%B4%EB%93%9Cmeta-downgrade)를
> 참고한다.

#### CM protocol Version

통신 프로토콜 버전은 변경되지 않았다.

#### Replication protocol Version

Replication 프로토콜 버전은 변경되지 않았다..

#### Sharding Version

샤딩 버전은 변경 되지 않았다.

> 알티베이스 샤딩 프로토콜 및 메타는 상위, 하위 호환성을 보장하지
> 않는다. 즉, 샤딩 버전이 다른 경우, 재구성해야 한다.

### 프로퍼티

#### 추가된 프로퍼티

#### 변경된 프로퍼티

#### 삭제된 프로퍼티

### 성능 뷰

#### 추가된 성능 뷰

#### 변경된 성능 뷰

#### 삭제된 성능 뷰
