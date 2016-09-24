---
title: ManyToMany
---

2010-09-29

Account 와 Role 처럼 ManyToMany 관계가 만들어질 때는
중간에 릴레이션 테이블을 만들어 두개의 ManyToOne 으로 분해되도록 모델링을 합니다.

날 쿼리를 쓰면 중간 릴레이션 테이블이 조인에 자주 등장하여 지저분해 지는데
ORM 에서는 ManyToMany 관계도 쉽게 기술할 수 있습니다.

먼저 `Account` 클래스입니다.
`account`, `role` 그리고 둘을 연결하는 `account_role` 테이블이 있다고 가정합니다.

    @Entity
    public class Account {
    
      @ManyToMany
      @JoinTable(
        name = "account_role",
        joinColumns = @JoinColumn(name = "account_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id")
      )
      private Set<Role> roles = new HashSet<Role>();
    
    }

ManyToMany 에서는 어느 한쪽 클래스에 어노테이션을 집중해서 기술하고 반대편에서는 그것을 참고만 합니다.
여기서는 Account 에 어노테이션을 붙인 경우입니다.

`@ManyToMany` 가 일단 보이고요.
왠지 `Account` 는 `Role` 을 바로 당여야 할 것 같아서 `FetchType.LAZY` 선언하지 않았습니다.

그리고 `@JoinTable` 어노테이션으로 중간 릴레이션 테이블의 이름과
양쪽 테이블에서 사용하는 foreign key 이름을 지정해야 합니다.

어카운트 생성한 후 Role 을 바로 추가하기 위해 `roles` 필드를 초기화 하는 코드도 넣었습니다.

`Role` 클래스입니다.

    @Entity
    public class Role {
    
      @ManyToMany(mappedBy = "roles", fetch = FetchType.LAZY)
      private Set<Account> accounts;
    
    }

사실 `Role` 에서 `Account` 목록을 끌어당겨 볼 일은 거의 없을 겁니다.
일단 그냥 해본다는 생각으로 어노테이션을 보면
`Account` 쪽에서 복잡한 설정은 모두 붙였기 때문에 `Role` 에서는 별도로 지정할 것이 별로 없습니다.
`mappedBy` 로 `Account` 클래스의 어떤 필드를 어노테이션 참조에 쓸 것인가만 알려주면 됩니다.

Role 과 Account 엔터티를 생성하고 관계를 맺는 예제입니다.
아래처럼 기술하면 릴레이션 테이블까지 적절히 데이터가 들어갑니다.

    @PersistenceContext
    EntityManager em;
    
    Role userRole = new Role();
    userRole.setName("user");
    em.persist(userRole);
    
    Role adminRole = new Role();
    adminRole.setName("admin");
    em.persist(adminRole);
    
    Account adminAccount = new Account();
    adminAccount.getRoles().add(userRole);
    adminAccount.getRoles().add(adminRole);
    em.persist(adminAccount);

위에는 `Role` 을 새로 생성하고 영속화 시켰는데,
디비에 이미 들어있는 `Role` 을 사용하려면 일단 끌고 와서 대입하면 되겠지요.

    Role guestRole = em.find(Role.class, guestRoleId);

`Role` 클래스에 간단한 `equals` 메서드와 `hashCode` 를 추가했다면,
  
    class Role {
      @Override
      public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Role role = (Role) o;
        if (id != role.id) return false;
        return true;
      }
    
      @Override
      public int hashCode() {
        return id;
      }
    }


아래와 같은 구문으로 Account 에 지정되어 있는 Role 을 확인할 수 있습니다.

    assertThat(adminAccount.getRoles().contains(userRole), is(true));
    assertThat(adminAccount..getRoles().contains(adminRole), is(true));
    assertThat(adminAccount..getRoles().contains(guestRole), is(false));


{:class="go-to-index"}
[Java Hibernate](index)
