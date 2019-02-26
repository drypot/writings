---
title: Sorted Collection
---

2010-09-30

JPA 문서를 아무리 뒤져도 컬렉션 프로퍼티에 sql order by 지정하는 것이 없어 보이네요.
JPA @OrderBy 라고 있는데 디비에서는 그냥 쌩으로 가져와서 메모리에서 소팅합니다. 쀅~
JPA @OrderColumn 이라고 있는데 이것도 좀 거시기하고, desc 도 없고.

하이버네이트 확장 기능 찾아봤더니 좀 쓸만한 어노테이션이 나옵니다.

    @ElementCollection
    @CollectionTable(name = "nick_name", joinColumns = @JoinColumn(name = "account_id"))
    @Column(name = "nick_name")
    @OrderBy(clause = "nick_name desc")
    private List<String> nickNames = new ArrayList<String>();

위에 @OrderBy 는 JPA 패키지에 있는 것이 아니고, 하이버네이트쪽에 있는 겁니다.
임포트하실 때 주의해주세요.

SQL 출력 보면 SQL 문에 order by 박아서 보냅니다.


{:class="go-to-index"}
[Java Hibernate](index)
