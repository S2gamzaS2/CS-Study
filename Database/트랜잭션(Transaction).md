트랜잭션(Transaction)
=============

> 트랜잭션이란?
  - 데이터베이스의 상태를 변환시키기 위한 논리적인 작업의 단위

> 트랜잭션의 기본 성질(ACID)
  - 원자성(Atomicity)
    - 데이터베이스에 모두 반영되거나 반영되지 않아야한다.
  
  - 일관성(Consistency)
    - 트랜잭션이 완료되면 일관성있는 상태로 데이터베이스에 반영되어야한다.
    - 고정 요소는 트랜잭션 전후 모두 같아야한다.
  
  - 고립성(Isolation)
    - 두 개의 트랜잭션이 동시에 진행될 때 하나의 트랜잭션이 실행중이면 다른 트랜잭션은 끼어들 수 없다.
    - 수행중인 트랜잭션이 완료될때까지 다른 트랜잭션에 끼어들 수 없다.
  
  - 지속성(Durability)
    - 성공적으로 완료된 트랜잭션은 어떠한 경우라도 반영되어야 한다.

> 트랜잭션의 연산 종류
  - COMMIT
    - COMMIT 연산을 통해 트랜잭션의 수행이 성공적으로 완료되어 데이터베이스에 결과를 반영한다.

  - ROLLBACK
    - ROLLBACK 연산을 통해 트랜잭션의 수행이 실패했으므로 데이터베이스를 수행전과 동일한 상태로 되돌린다.

> 트랜잭션의 상태 변화

![트랜잭션 이미지](/Database/images/transaction.png)

  - 활동(Active) : 트랜잭션 실행 상태
  - 부분완료(Partially Committed) : 마지막 연산까지 실행하고 commit 연산이 실행되지 직전의 상태
  - 완료(Committed) : 트랜잭션이 성공하여 commit 연산이 실행된 상태
  - 실패(Failed) : 오류가 발생하여 중단된 상태
  - 철회(Aborted) : 트랜잭션이 비정상적으로 처리되어 rollback을 수행한 상태

### 참고 사이트
- https://velog.io/@shasha/Database-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EC%A0%95%EB%A6%AC
- https://coding-factory.tistory.com/226
- https://gyoogle.dev/blog/computer-science/data-base/Transaction.html
- https://doooyeon.github.io/2018/09/28/transaction.html
