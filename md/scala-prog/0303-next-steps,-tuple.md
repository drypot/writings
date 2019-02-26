---
title: First Steps, Tuple
---

2011-07-12 21:33

### Use Tuples / 튜플 사용하기

튜플은 또 다른 유용한 컨테이너 오브젝트다.
튜플은 리스트처럼 불변이지만 리스트와 다르게 다양한 타입의 원소를 가질 수 있다.
메서드에서 여러 오브젝트를 리턴하는 경우에 유용하게 사용할 수 있다.
자바에서 이런 일을 하려면 리턴 오브젝트들을 담기 위한 클래스를 만들어야 했겠지만 스칼라에서는 간단히 튜플을 쓰면 된다.
튜플은 만들기 쉽다. 튜플에 담으려는 오브젝트들을 ( ) 로 감싸기만 하면 된다.

    val pair = (99, "Luftballons")
    println(pair._1)
    println(pair._2)

결과

    99
    Luftballons

튜플의 원소를 사용하려면 _N 방식의 구문을 사용한다. 여기서 N 은 숫자이고 1 부터 시작한다.
스칼라는 튜플의 타입을 Tuple2[Int, String] 으로 추론한다.

튜플의 타입은 원소의 갯수와 원소의 타입에 따라 결정된다.
(99, "Luftballons") 의 타입은 Tuple2[Int, String] 이고,
('u', 'r', "the", 1, 4, "me") 의 타입은 Tuple6[Char, Char, String, Int, Int, String] 다.

참고로 원소 갯수에 따른 튜플 타입들은 Tuple22 까지 정의되어 있다.
그러니 이 이상은 사용할 수 없을 꺼다.

원소에 접근하기 위해 pair(1) 대신 pair._1 과 같은 방식을 사용하는 이유는
튜플의 원소 타입이 모두 달라서 pair(n) 의 고정 타입을 결정할 수 없기 때문이다.

원소 번호는 Haskell, ML 등 정적 타입 튜플을 사용하고 있는 다른 언어의 전통을 따라 0 이 아닌 1 부터 시작한다.


{:class="go-to-index"}
[Programming in Scala](index)
