---
title: EntityManager
---

2010-10-01

엔터티 메니저에 중요한 메서드 간략이 적어둡니다.

    void persist(Object entity)

새 엔터티 등록, 프라이머리 키는 왠만하면 ORM 엔진이 넣어주는 줄거리가 좋겠지요.

    T merge(T entity) 

detach 됐거나 수작업 생성한 엔터티를 EM 에 다시 붙임.

    void remove(Object entity)
  
EM 에 붙어있는 오브젝트에 해당하는 디비 레코드 삭제, 쌩 delete 날리셔도.

    T find(Class<T> entityClass, Object primaryKey)

디비에서 해당 레코드 읽어와 EM 에 오브젝트 적재후 오브젝트 리턴.

오브젝트의 컬럼 몇 개 수정하려면 `find` 로 오브젝트 받아서 `setXXX` 로 대입하신 다음 내버려 두면 디비에 들어갑니다.
아니면 수작업으로 오즈젝트 통채로 만들어서 merge.
아니면 쌩 update.

    T getReference(Class<T> entityClass, Object primaryKey)

find 와 다르게 프락시를 리턴합니다.

엔터티 클래스에 릴레이션을 기술해 놓은 경우 엔터티 오브젝트를 `persist`  할 때
연관 엔터티들을 대입해 둘 필요가 있는데 이때 find 로 실제 데이터 가져 오지 말고
`getReference` 로 프락시 받아서 대입해 주셔도 됩니다.

    void flush() 

EM 에 쌓여 있는 수정 명령들 디비에 적용, 보통은 트랜잭션 끝날 때 자동으로 호출됩니다.

트랜잭션에 대해 추가로 적으면, ORM 이 보기에 트랜잭션 안 에서 일어날 수 있는 상황은 딱 두가지 입니다.
정상적으로 트랜잭션 전 과정을 마쳤을 경우 자동으로 commit.
익셉션이 발생하면 자동으로 rollback.

    void refresh(Object entity)

서버 트리거등으로 디비 내용과 EM 케쉬 내용이 달라진 것 같으면 오브젝트 재로딩.

    void detach(Object entity)
  
EM 에서 엔터티 떼버림. 저장 작업과 오브젝트가 상관 없어 집니다.

참고로 트랜잭션이 끝나면 EM 이 가지고 있던 오브젝트들은 자동으로 모두 detach 상태로 갑니다.

    Query createQuery(String qlString)

JP-QL 쿼리 생성, 많이 보셨을 것 같고요.

    TypedQuery<T> createQuery(String qlString, Class<T> resultClass)

리턴 결과로 줄 당 오브젝트 한 개일 경우만.

    Query createNativeQuery(String sqlString)

날 쿼리 생성.

    T unwrap(Class<T> cls)

JPA API 를 실제 구현하고 있는 오브젝트를 돌려줍니다. 하이버네이트 경우 `Session.class`

    void close()

인간이 이 메서드를 호출할 일은 없습니다.

    EntityTransaction getTransaction()
  
인간이 이 메서드를 호출할 일은 없습니다.

기타 크리테리아 관련 메서드는 모두 생략.  
고통을 즐기는 타입이시라면,


{:class="go-to-index"}
[Java Hibernate](index)
