---
title: ManyToOne
---

2010-09-29

ORM 을 쓰면 테이블 조인을 안 하고 관련 타켓 테이블의 데이터 / 오브젝트를 가져올 수 있습니다.
세세히 들여다 보면 골치아픈 문제가 꽤 되지만
자주 등장하는 몇 가지 패턴만 알고 있으면 코딩에 큰 무리는 없을 것 같습니다.

### Many To One

`account` 테이블이 있고 (One 사이드에 해당),
`coupon` 테이블이 있고 (Many사이드에 해당),
`coupon` 테이블의 `account_id` (foreign key) 컬럼에 의해 두 테이블이 관계 맺고 있는 경우.

    @Entity
    public class Coupon {
      @Id @GeneratedValue
      private int id;
    
      @ManyToOne(fetch = FetchType.LAZY)
      @JoinColumn(name = "account_id")
      private Account account;
    }

`Coupon` 클래스에서는 위와 같이 `@ManyToOne`, `@JoinColumn` 어노테이션으로 관계를 기술합니다.
아마도 왠만하면 Lazy Fetch 를 사용할 것이므로 `@ManyToOne` 에 `FetchType.LAZY` 붙여놔 봤습니다.
쿠폰을 읽을 때 무조건 어카운트 오브젝트도 끌고 오려면 `FetchType.LAZY` 를 삭제하면 되겠지요.

`@JoinColumn` 은 스팩대로라면 없어도 되는데 아래 다시 설명할 `ImprovedNamingStrategy` 문제 때문에 붙여두었습니다.
기본 스펙에 따르면 '프로퍼티 이름' + _ + '타겟 테이블 아이디' 를 가지고 조인 컬럼명을 자동 생성한다고 되어 있습니다.
그러니까 `account_id` 로 자동생성되어야 하는데, `ImprovedNamingStrategy` 때문에 그냥 `account` 컬럼을 찾으려고 합니다.

저렇게 어노테이션 두 개를 붙여두면 쿠폰 오브젝트에서 관련 어카운트 오브젝트가 필요할 때는 쉽게 가져올 수 있습니다.

    coupon.getAccount();

우리가 만들어 주는 `getAccount` 는 `account` 를 단순 리턴하는 수준이면 됩니다.
하이버네이트가 리턴한 프락시가 사용하는 고기능 getAccount 는 하이버네이트가 런타임에 생성합니다.


### One To Many

반대로  coupon table 의 account_id 컬럼 데이터를 가지고
account 쪽에서 관련 coupon 컬렉션을 당겨 쓸 수 있겠지요.
쿠폰 쪽의 ManyToOne 을 어카운트 쪽에서 반대로 바라보면 OneToMany 가 됩니다.

    @Entity
    public class Account {
      @Id @GeneratedValue
      private int id;
    
      @OneToMany(mappedBy = "account", fetch = FetchType.LAZY)
      private Set<Coupon> coupons;
    }

`@OneToMany` 붙여주시고, 데이터 형은 컬렉션 **인터페이스** 중에 하나 써주시면됩니다.
`Set`, `List`, `Map`.
하이버네이트가 자체 타입 오브젝트를 생성해서 붙여야 하므로
`HashSet`, `ArrayList`, `HashMap` 같은 컨트리트 타입은 쓰실 수 없습니다.

OneToMany 쪽에는 `mappedBy` 정의가 필요합니다.
`account` 입장에서는 관계에 관한 정보를 가지고 있지 않기 때문에
필요한 어노테이션들이 `Coupon` 클래스의 어떤 프로퍼티에 붙어있는지 알려줄 필요가 있습니다.
`mappedBy` 에 써주는 이름은 테이블 컬럼명이 아니고 자바 필드명입니다.


### ImprovedNamingStrategy Problem

조인 컬럼명이 스팩대로 지정 안 되는 것은 자바 camelCase 를 mysql_case 로 바꾸어 주는
`ImprovedNamingStrategy` 문제입니다.
 
<http://matthew.mceachen.us/blog/hibernate-naming-strategies-20.html>{:target="_blank"}

`@ManyToOne` 등장할 때마다 `@JoinColumn` 을 명시해 주셔도 되겠고,
귀찮으신 분들은 위 블로그 내용대로 `ImprovedNamingStrategy` 를 수정한 오브젝트를
스프링에 대신 박아 쓰시면 될 것 같습니다.


{:class="go-to-index"}
[Java Hibernate](index)
