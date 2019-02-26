---
title: Caching
---

2010-10-05

EntityManager 를 쓰면 무조건 1 차 케쉬 개념이 들어가나
EM 은 웹 리퀘스트 기간만 존재하고 매번 다시 만들어지기 때문에
읽기 전용 엔터티에 대한 케슁을 하는데는 미흡합니다.

하이버네이트 레퍼런스에 한뼘 나와 있어서 못 보고 넘어갔는데
ORM 에 2 차 케쉬 개념이 있군요.

예상할 수 있는 대로 읽기 전용 엔터티를 모든 EM 간에 공유하게 해주는 것입니다.
@OneToMany getRoles() 같은 릴레이션 메서드에도 선언할 수 있습니다.

    @Entity
    @Cacheable
    public class Forest { ... }

또는

    @OneToMany
    @JoinColumn(name="CUST_ID")
    @Cache(usage = CacheConcurrencyStrategy.NONSTRICT_READ_WRITE)  // 하이버네이트 네이티브
    public SortedSet<Ticket> getTickets() {
      return tickets;
    }

여기 까지는 JPA 스팩 정도에 나오는 해피한 시나리오고요.
실제 케슁을 쓰려면 하이버네이트 같은 실제 ORM 엔진 구현에 따라 기동법이 다르고
하이버네이트만 해도 여러 케슁 엔진들이 나누어집니다.
거기에 들어가는 어노테이션도 비표준으로 내려갑니다.


{:class="go-to-index"}
[Java Hibernate](index)
