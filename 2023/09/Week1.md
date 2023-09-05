# Week 1 (2023-09-04 ~ 2023-09-10)

## Spring DB 1편 - 트랜잭션 이해
### 트랜잭션 개념 이해
- auto commit 옵션이 없는 경우 insert, update 등 조작이 이루어졌을 때 commit 전 까지는 다른 세션에서는 조회할 수 없다.
    - uncommited read 옵션을 사용하는 경우는 읽기가 가능하다.
- 보통 이렇게 auto commit 옵션을 끄고 commit 되지 않은 작업 단위를 시작 하는 것을 관례상 트랜잭션의 시작이라고 부른다.

### 락의 이해
- 트랜잭션 중 다른 세션이 update, delete 와 같이 데이터 수정을 가하지 못하도록 DB 자체적으로 방지하는 것을 락이라고 한다.
- 조회만으로도 락을 걸 수 있는데, `select ~ for update` 구문을 이용하면 조회 중에도 다른 세션이 이를 변경할 수 없는 락을 가져온다.