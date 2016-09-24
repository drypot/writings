---
title: Spring + JUnit + MySQL + Rollback
---

2010-11-17

Spring Transactional 사용하는 JUnit 테스트 모듈에서 Database 작업을 할 때는
기본적으로 테스트 케이스 종료와 함께 Rollback 이 이루어진다.
Commit 이 기본이 아니고, Rollback 이 기본이다.
문제는 MySQL 에 Rollback 개념이 없어서 데이터 삽입명령이 있으면 디비에 들어가버리고
Rollback 을 안 하는 듯 보일 수도 있다는 것.

EntityManager 케쉬 운영 특성상 어떤 경우는 데이터가 들어갈 수 있고
어떤 경우는 Commit 을 기다리다가 Rollback 되어 사라져 버릴 수가 있다.
이 때는 MySQL 덤프해보면 데이터가 안 들어가 있다.

그러니, TestCase 돌릴 때는 HSQLDB, H2 같은 롤백 지원되는 디비를 쓰시던지
그냥 Commit 시켜버리는 것이 일관적일 수 있다.

Commit / Rollback 선택은 @Rollback 어노테이션으로 한다.
토비3에 관련 페이지는 553.

    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration(locations = "/test-spring-config.xml")
    public class PostDaoTest {
    
      @PersistenceContext private EntityManager em;
      @Autowired private PostDao dao;
    
      @Before
      public void setUp() throws Exception {
      }
    
      @Test
      @Transactional
      @Rollback(false)
      public void testPersist() {
      }
    }


{:class="go-to-index"}
[Java Spring](index)
