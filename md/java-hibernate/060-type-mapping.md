---
title: Type Mapping
---

2010-09-27

데이터베이스 마다 사용할 수 있는 타입이 다르고
기초적인 타입에 대해서도 같은 명칭으로 의미가 다른 타입을 지정하고 있는 경우가 많습니다.
하이버네이트는 가상의 mapping type 이란 것을 정의하고
어플리케이션에서 하이버네이트 mapping type 을 사용하면
런타임에 연결되는 SQL Dialect 정의에 따라서 적절한 SQL 타입을 사용하는 방식을 쓰고 있습니다.

이렇게 3 단계로 가면, 2 단계로 갈 때보다 유연성은 좋아지겠으나
별로 알고 싶지 않은 하이버네이트 mapping type 이란 것에 대해서 알 필요가 생겨버립니다.
자바 기본 타입들이 어떤 하이버네이트 타입으로 매핑되는지
어떤 하이버네이트 타입이 MySQL 타입으로 매핑되는지, 두배로 신경써야합니다.

어쨌든, 프레임웍에서는 유연성을 강조할 수 밖에 없으니 개발자가 알아야 할 것이 좀 늘어납니다.

<http://docs.jboss.org/hibernate/stable/core/reference/en/html/mapping.html#mapping-types-basictypes>{:target="_blank"}

레퍼런스의 해당 섹션인데 저거 가지고는 좀 부족하고 직접 써보면서 내공이 좀 쌓여야 할 것 같습니다.
`MySQLDialect` 가 하이버네이트 mapping type 을 어떻게 해석하는지 이건 공식 도큐먼트가 안 보이는 것 같습니다.


{:class="go-to-index"}
[Java Hibernate](index)
