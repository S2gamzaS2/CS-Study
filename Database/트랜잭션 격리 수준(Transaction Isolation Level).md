트랜잭션 격리 수준(Transaction Isolation Level)
=============

> 트랜잭션 격리 수준이란?
  - 트랜잭션이 동시에 처리될 때 서로 얼마나 고립되어 있는지를 나타내는 정도이다.

> 트랜잭션 격리 수준 순서
  - SERIALIZABLE(직렬화 가능)
    - 가장 엄격한 격리 수준
    - SELECT일 경우에도 잠금을 설정함
    - 동시처리 능력이 떨어지고 성능저하 발생
    - 안전한 작업이 필요한 경우 사용

  - REPEATABLE READ(반복가능한 읽기)
    - 변경전의 데이터는 언두라는 공간에 저장함
    - 트랜잭션은 고유의 순번이 존재함
    - 두개의 트랜잭션이 발생했을 때 순번이 큰 트랜잭션에서 업데이트가 발생했을 때 순번이 작은 트랜잭션은 언두에 있는 내용을 읽음
    - 동일한 트랜잭션에서도 새로운 래코드가 추가되는 경우 조회 결과가 달라질 수 있는데 이것을 Phantom Read(유령 읽기)라고 함

  ![반복가능한읽기 이미지](/Database/images/repeatableread.png)
  
  - READ COMMITTED(커밋된 읽기)
    - 커밋된 데이터만 읽을 수 있음
    - Non Repeatable Read(반복불가능한 읽기), Phantom Read(유령 읽기) 모두 발생

  ![커밋된읽기 이미지](/Database/images/readcommitted.png)
  
  - READ UNCOMMITTED(커밋되지않은 읽기)
    - 커밋되지않은 데이터도 접근할 수 있음
    - 하나의 트랜잭션이 완료되지 않았지만 다른 트랜잭션에서 조회할 수 있는 문제를 Dirty Read(오손 읽기)라고 함
   
  ![커밋되지않은읽기 이미지](/Database/images/readuncommitted.png)

### 참고 사이트
- https://mangkyu.tistory.com/299
- https://joont92.github.io/db/%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EA%B2%A9%EB%A6%AC-%EC%88%98%EC%A4%80-isolation-level/
- https://velog.io/@yujiniii/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EA%B2%A9%EB%A6%AC-%EC%88%98%EC%A4%80
