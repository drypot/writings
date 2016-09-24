---
title: Hibernate, TopLink, JPA
---

2010-09-11

Hibernate 이야기를 풀어가기 전에 EJB, Hibernate, TopLink, JPA, EclipseLink 간의 관계를 정리합니다.

태초에 EJB 1, 2 가 있었으나 복잡한 사용법 때문에 받아들여지지 못했습니다.
EJB 오브젝트 저장에 대한 대안으로 2001 년부터 Hibernate 가 만들어지기 시작했습니다.
Hibernate 이전에 90 년대부터 TopLink 라는 ORM 이 있었는데 2002 년에 Oracle 이 인수합니다.
디 펙토 스텐다드가 된 것은 Hibernate 였습니다.
그러다 EJB 를 대신할 새로운 공식 저장 스펙을 만들기 위해 Hibernate 와 TopLink 개발진이 모여 논의를 시작합니다.
그 결과로 나온 것이 JPA (Java Persistence API) 입니다.

JPA 제정과정에서 API 는 Hibernate 것을 쓰지 않고 TopLink 것을 쓰게 됩니다.
보셔서 아시겠지만 Hibernate Native API 가 좀 후집니다. ^^.
TopLink 것이 근사합니다.
잘한 결정이라고 봅니다.
이후 Hibernate 는 TopLink 그러니까 JPA 표준을 구현하기로 합니다.
그래서 Hibernate 는 Native API 와 JPA API 두 벌을 가지고 있습니다.
사용하실 때는 JPA API 를 쓰시는 것이 좋습니다.
Native API 로 예제를 만들어 설명하는 Hibernate 책들은 피하시길 권합니다.

JPA 를 제정하면서 Oracle 은 TopLink 를 오픈소스화 하고 코드를 이클립스 재단에 기부합니다.
그래서 이름을 바꾸어 나온 것이 EclipseLink 입니다.
현재 JPA 표준 레퍼런스 구현의 지위를 가지고 있습니다.
Oracle 사이트에서는 계속 TopLink 라는 것을 다운 받을 수 있는데 압축화일 안은 EclipseLink 입니다.
이클립스 재단에서 받는 것과 완전히 동일합니다.
그냥 과거의 브랜드를 유지하기만 할 뿐 차이가 없습니다.
오라클의 TopLink 개발자들은 풀타임으로 오픈소스로 EclipseLink 개발을 하고 있습니다.

Hibernate 와 EclipseLink 의 프락시 오브젝트 생성법은 전혀 다릅니다.
Hibernate 는 좀 퍼포먼스가 떨어지긴 하지만 JVM 에이전트 인자 없이 프락시를 생성하는 방법을 쓰고 있는데
EclipseLink 은 에이전트를 붙여야하고 이게 잘못되면 다른 프레임웍과 골치아픈 문제를 만들 수 있습니다.
JVM 에 에이전트는 하나만 붙일 수 있습니다.

현재는 EclipseLink 사용자보다 Hibernate 사용자가 훨씬 많습니다.
EclipseLink 도큐먼트는 방대하고 잘 정리되어 있으나 현재는 원서밖에 없는 문제도 있습니다.


{:class="go-to-index"}
[Java Hibernate](index)
