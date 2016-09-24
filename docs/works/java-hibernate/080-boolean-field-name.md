---
title: Boolean Field Name
---

2010-09-29

boolean 타입 자바 필드와 디비 컬럼명에 관해.

    boolean happyDay;

라는 자바 필드가 있을 때 JavaBean 규약에 따라 프로퍼티를 만들면 아래와 같이 됩니다.

    boolean getHappyDay();
    void setHappyDay(boolean value); 

이때 타입이 `boolean` 이라면 `get` 대신 `is` 를 사용하여 아래와 같은 메서드 사용이 가능합니다.

    boolean isHappyDay();

그러니 `is` 접두어는 JavaBean 스펙에 따른 메서드명에만 사용하고
필드와 컬럼명에 와서는 안 되겠군요.

예로 `isHappyDay` 라는 필드나 `is_happy_day` 라는 디비 컬럼명은 피하는 것이 좋겠습니다.
자바빈 메서드화 하면 `getIsHappyDay` 라고 쓰거나 `isIsHappyDay` 라고 써야하니까요.

저도 헷갈려서 막썼는데, 어제 JPA 스펙보다보니 언급이 좀 있어서,


{:class="go-to-index"}
[Java Hibernate](index)
