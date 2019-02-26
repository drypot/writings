---
title: JUnit + Hibernate
---

2010-09-30

도메인 오브젝트, DAO, 서비스 오브젝트 테스트 할 때,
수작업 데이터 넣고 테스트하려면 어려우니
테스트 데이터도 유닛테스팅 초기화시에 하이버네이트가 넣게 하는 것이 쉽습니다.

익숙하지 않은 방식이라면 처음에 시작해 보기 매우 껄끄러울 터인데
일단 JUnit 에 익숙해지면 println 한 줄도 JUnit 안에서 하고 싶어집니다.

    package net.directmedia.todaysfortune.domain;
    
    import org.joda.time.LocalDate;
    import org.junit.Before;
    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.test.context.ContextConfiguration;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
    import org.springframework.transaction.annotation.Transactional;
    
    import javax.persistence.EntityManager;
    import javax.persistence.PersistenceContext;
    import java.math.BigInteger;
    
    import static org.hamcrest.core.Is.is;
    import static org.junit.Assert.assertThat;
    
    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration(locations = "/spring-config-for-test.xml")
    public class AccountTest {
    
      @PersistenceContext
      EntityManager em;
    
      private Account account1;
      private Account account2;
      private Account account3;
    
      @Before
      public void setUp() throws Exception {
        em.createNativeQuery("truncate table account").executeUpdate();
        em.createNativeQuery("truncate table nick_name").executeUpdate();
    
        account1 = new Account();
        account1.setBirthDate(new LocalDate());
        account1.setBirthDateSolar(true);
        account1.setName("snowman1");
        account1.setUdid("udid1");
        account1.setPopularity(11);
        em.persist(account1);
    
        account2 = new Account();
        account2.setBirthDate(new LocalDate());
        account2.setBirthDateSolar(true);
        account2.setName("snowman2");
        account2.setUdid("udid2");
        account2.setPopularity(22);
        account2.getNickNames().add("nick2a");
        account2.getNickNames().add("nick2b");
        em.persist(account2);
    
        account3 = new Account();
        account3.setBirthDate(new LocalDate());
        account3.setBirthDateSolar(false);
        account3.setName("snowman3");
        account3.setUdid("udid3");
        account3.getNickNames().add("nick3a");
        account3.getNickNames().add("nick3c");
        account3.getNickNames().add("nick3b");
        account3.setPopularity(33);
        em.persist(account3);
    
        em.flush();
        em.clear();
      }
    
      @Test
      @Transactional
      public void testCount() {
        assertThat(((Long) em.createQuery("select count(*) from Account").getSingleResult()), is(3L));
        assertThat(((BigInteger) em.createNativeQuery("select count(*) from nick_name").getSingleResult()).longValue(), is(5L));
      }
    
      @Test
      @Transactional
      public void testGet() {
        assertThat(em.find(Account.class, account1.getId()), is(account1));
        assertThat(em.find(Account.class, account2.getId()), is(account2));
        assertThat(em.find(Account.class, account3.getId()), is(account3));
      }
    
      @Test
      @Transactional
      public void testNickName() {
        Account a1 = em.find(Account.class, account1.getId());
        assertThat(a1.getNickNames().size(), is(0));
    
        Account a2 = em.find(Account.class, account2.getId());
        assertThat(a2.getNickNames().size(), is(2));
        assertThat(a2.getNickNames().contains("nick2a"), is(true));
        assertThat(a2.getNickNames().contains("nick2b"), is(true));
        assertThat(a2.getNickNames().contains("nick3a"), is(false));
    
        Account a3 = em.find(Account.class, account3.getId());
        for (String nickName : a3.getNickNames()) {
          System.out.println(nickName);
        }
        assertThat(a3.getNickNames().size(), is(3));
      }
    
    }

잘 만들어진 테스트 예제는 아니지만
도메인 오브젝트나 서비스 로직 테스트할 때 이런 식으로 구성해서 돌리면 됩니다.

`/spring-config-for-test.xml` 파일은 `src/test/resources` 에 들어있는 파일인데,
테스트용 스프링 환경을 초기화하기 위한 스프링 설정입니다.
디비 컨넥션 풀 빼버린 것만 빼면 런타임 스프링과 거의 흡사합니다. 

톰켓 4-5 번 띄웠다 죽였다 할 시간이면 유닛테스트 한벌 만들 수 있습니다.
한번 만들기 귀찮지만 일단 만들어 두면 코드 테스트할 때 사람이 하던 사이클을 프로그램이 대신해 줍니다.


{:class="go-to-index"}
[Java Hibernate](index)
