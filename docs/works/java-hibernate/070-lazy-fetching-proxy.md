---
title: Lazy Fetching
---

2010-09-28

일반적인 날 쿼리에서는 테이블간 조인 절차를 사람이 일일이 기술합니다.
체인 좀 길어지면 짜증이납니다.
ORM 에서는 절차적인 코드 대신에 도메인 클래스에 선언적으로 조인을 기술합니다.
한번 적어 놓으면 끝이기 때문에 디비에서 데이터 땡겨오는 구문에서는 조인 조건이 나오지 않습니다.

먼저 ORM 을 쓰면서 예전 방식으로 수작업 조인하도록 되어 있는 도메인 오브젝트입니다.

    @Entity
    public class Coupon {
      @Id
      @GeneratedValue
      private int id;
    
      private int accountId;
    }

`accountId` 라는 필드가 있는데 이것을 `Account` 클래스에 대한 외래키라고 한다면
`Account` 오브젝트 가져오기 위해서는 `accoutId` 값을 가지고 아래 처럼 수작업 `find` 를 해야 합니다.

  em.find(Account.class,  accountId)

다음 코드는 선언적으로 클래스를 결합하는 예입니다.

    @Entity
    public class Coupon {
      @Id
      @GeneratedValue
      private int id;
    
      @ManyToOne
      @JoinColumn(name = "account_id")
      private Account account;
    }
    

`int accountId` 그냥 두고 `accountId` 와 `account` 모두 사용하게 할 수 있으나 어노테이션이 더 필요하니 `accountId` 필드를 삭제하고 `Account account` 필드를 쓰도록하겠습니다.

조인 관계를 기술하는 어노테이션 두개가 추가 되었습니다.
위와 같이 선언적으로 조인을 기술해두면 별도의 리딩 명령 없이
간단히 다른 클래스/테이블 오브젝트를 쓸 수 있습니다.

   coupon.getAccount()

그런데 위와 같이 조인 어노테이션을 하면
`Coupon` 오브젝트를 읽을 때 무조건 관련 `Account` 오브젝트도 읽습니다.
`Account` 오브젝트를 사용할 때만 로딩하기 위해서는 아래와 같이
`@ManyToOne` 에 `fetch` 어트리뷰트를 추가해 줍니다.

    @Entity
    public class Coupon {
      @Id
      @GeneratedValue
      private int id;
    
      @ManyToOne(fetch = FetchType.LAZY)
      @JoinColumn(name = "account_id")
      private Account account;
    }

Lazy Loading 을 쓰면 `coupon.getAccount()` 메서드 실행시
실제 Account 오브젝트 대신에 하이버네이트가 만든 Account Proxy 오브젝트가 리턴됩니다.
이 Proxy 오브젝트는 메서드가 처음 호출될 때 실제 Account 데이터를 디비에서 읽어 옵니다.
이쯤에 와서 아래 블로그에 설명된 문제가 두개 발생합니다.

Advanced Hibernate: Proxy Pitfalls\\
<http://blog.xebia.com/2008/03/08/advanced-hibernate-proxy-pitfalls/>{:target="_blank"}

첫번째 문제는 Proxy 가 메서드는 처리하지만 실제 필드에는 값을 넣지 못한다는 것입니다.
account.getName() 을 호출하면 정상적인 값이 넘어오지만,
account.name 필드에 직접 접근하면 값이 null 인 상태가 계속 됩니다.
그러니 Lazy 로딩을 쓸 때는 해당 오브젝트에 무조건 메서드를 통해 접근해야합니다.

두번째는 리던되는 오브젝트의 클래스가 하이버네이트가 만든 프락시 클래스이기 때문에
클래스를 비교하는 작업이 있다면 조금 주의해야 합니다.


{:class="go-to-index"}
[Java Hibernate](index)
