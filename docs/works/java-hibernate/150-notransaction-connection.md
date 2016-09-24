---
title: No Transaction Connection
---

2010-10-01

JDBC 에서 JPA  로 올 때 헷갈리는 점 / 궁금한 점이 있는데
어떻게 JDBC 에서 하던대로 트랜잭션 없이 디비 입출력을 하는가 하는 것입니다.

결론부터 말하면 JPA 에서는 트랜잭션 없는 디비 입출력은 상정조차 안 하고 있습니다.
그래서 JPA 문서들을 보면 디비 컨넥션을 열고 닫는 설명 없이
트랜잭션을 열고 닫는 설명부터 나옵니다.

Spring / JPA 에서 프로그래머가 하는 일은
@Transaction 스프링 어노테이션으로 트랜잭션 선언을 하는 것입니다.
트랜잭션 열고 닫으면 디비 열고 닫기는 JPA 엔진이 알아서.


{:class="go-to-index"}
[Java Hibernate](index)
