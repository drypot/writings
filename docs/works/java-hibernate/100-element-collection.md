---
title: Element Collection
---

2010-09-30

일반적으로 엔터티에 컬렉션이 나타날 때는 두 엔터티가 OneToMany 관계에 있을 때 입니다.
그런데 엔터티화 하지 않아도 되는 간단한 컬렉션들이 간혹 필요합니다.
Role 속성을 숫자 하나나 스트링으로 처리할 경우나 레코드의 수정일에 관한 역사를 남기고 싶을 경우등.
이런 자료를 저장하기 위해서는 어쨌든 디비 상에 테이블을 만들어야 하지만
오로지 부모 테이블에서만 사용하고 그외 어떤 테이블로 부터도 참조되지 않기 때문에
프라이머리키를 둘 필요가 없습니다.

이런 데이터를 위해서는 엔터티 클래스를 매번 만들 필요가 없고,
부모 엔터티 클래스에 바로 엘리먼트 컬렉션 선언을 해주면 됩니다.

`nick_name` 테이블에 `account_id`, `nick_name` 컬럼이 있다고 하면,

    @Entity
    public class Account {
    
      @ElementCollection
      @CollectionTable(name = "nick_name", joinColumns = @JoinColumn(name = "account_id"))
      @Column(name = "nick_name")
      private Set<String> nickNames = new HashSet<String>();
    
    }

먼저 엔리먼트 컬렉션 어노테이션이 붙었고요.
어떤 테이블을 사용할 것인가를 `CollectionTable` 로 정의하였습니다. foreign key 이름도 적어 주었습니다.
해당 테이블에서 어떤 컬럼의 데이를 가져올 것인가는 `Column` 어노테이션으로 지정합니다.
위와 같이 선언해 두면 관련 컬렉션 데이터를 쉽게 가져다 쓸 수 있습니다.

컬렉션 데이터 추가하는 법

    account2 = new Account();
    account2.getNickNames().add("nick2a");
    account2.getNickNames().add("nick2b");
    em.persist(account2);

가져다 쓰는 법

    Account a2 = em.find(Account.class, account2.getId());
    assertThat(a2.getNickNames().size(), is(2));
    assertThat(a2.getNickNames().contains("nick2a"), is(true));
    assertThat(a2.getNickNames().contains("nick2b"), is(true));
    assertThat(a2.getNickNames().contains("nick3a"), is(false));


{:class="go-to-index"}
[Java Hibernate](index)
