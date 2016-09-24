---
title: Cascasde
---

2010-10-01

엔터티들을 연관짓는 @OneToOne, @OneToMany 류 어노테이션에 Cascade 엘리먼트를 추가할 수 있습니다.
디비의 foreign key 개념에 등장하는 그 Cascade 와 같습니다.

    @OneToOne( cascade = CascadeType.ALL )
    @PrimaryKeyJoinColumn
    public Heart getHeart() { ... }
    
    @OneToMany( cascade = {CascadeType.PERSIST, CascadeType.MERGE} )
    public Collection<Employer> getEmployers()

임의의 엔터티에 오퍼레이션이 가해질 때
연관 엔터티까지 줄줄이 오퍼레이션을 가할 것인지 지정합니다.

전통적으로 외래키와 케스케이딩은 진정으로 귀찮을 때만 사용하는 것이 좋습니다.
왠만하면 명시적으로 필요한 오퍼레이션을 모두 기술하는 것이 장점이 좀 있습니다.
또 업무하다 보면 엄마는 날리고 자식은 내버려둬야 하는 상황도 자주 있습니다.
구체적인 활용은 개발자 의지대로.

지정할 수 있는 케스케이딩 타입에는 아래와 같은 것들이 있습니다.

    CascadeType.PERSIST
    CascadeType.MERGE
    CascadeType.REMOVE
    CascadeType.REFRESH
    CascadeType.DETACH
    CascadeType.ALL


{:class="go-to-index"}
[Java Hibernate](index)
