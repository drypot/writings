---
title: Hibernate HSQL vs Spring Data Mongo
---

2011-12-21

비슷한 기능을 하는 HSQL 과 Spring Data Mongo 코드의 비교.
처음 나오는 코드가 HSQL, 다음 나오는 코드가 Spring Data 입니다.

관리 오브젝트 정의

    @PersistenceContext
    EntityManager em;

    @Autowired
    MongoOperations op;

한 레코드 읽기

    public PostThread getThread(int id) {
      return em.find(PostThread.class, id);
    }

    public PostThread getThread(int id) {
      return op.findById(id, PostThread.class);
    }

레코드 추가

    public void insertThread(PostThread thread) {
      em.persist(thread);
    }

    public void insertThread(PostThread thread) {
      thread.setId(seq.getNewThreadId()); // MongoDB 에는 Auto Increment 가 없다.
      op.insert(thread);
    }

업데이트

    // Hibernate 에서는 레코드 필드 업데이트 코드는 만들 필요가 없다.

    // MongoDB 에서는 명시적으로 저장해야 한다.

    public void updateThread(PostThread thread) {
      op.save(thread);
    }

카운트

    public int getThreadCount() {
      return em.createQuery(
          "select count(t) from PostThread t",
          Long.class
        )
        .getSingleResult()
        .intValue();
    }

    public int getThreadCount() {
      return (int)op.count(null, PostThread.class);
    }

조건을 가진 카운트
   
    public int getThreadCount(int categoryId) {
      return em.createQuery(
          "select count(t) from PostThread t " +
          "where categoryId = ?",
          Long.class
        )
        .setParameter(1, categoryId)
        .getSingleResult()
        .intValue();
    }

    public int getThreadCount(int categoryId) {
      return (int)op.count(new Query(new Criteria("categoryId").is(categoryId)), PostThread.class);
    }

리스트

    public List<PostThread> getThreadList(int categoryId, int currentPageBeginRow, int pageRows) {
      return em.createQuery(
          "select t from PostThread t " +
          "where postCategoryId = ? " +
          "order by updateDate desc",
          PostThread.class
        )
        .setParameter(1, categoryId)
        .setFirstResult(currentPageBeginRow)
        .setMaxResults(pageRows)
        .getResultList();
    }

    private List<PostThread> getThreadList(int categoryId, int pageBeginRow, int pageRows) {
      Query query = new Query(new Criteria("categoryId").is(categoryId));
      query.sort().on("udate", Order.DESCENDING);
      query.skip(pageBeginRow).limit(pageRows);
      return op.find(query, PostThread.class);
    }

뷰 카운트 업데이트

    public void updateThreadHit(int threadId) {
      em.createQuery(
        "update PostThread t " +
        "set t.hit = t.hit + 1 " +
        "where t.id = ?1"
      )
      .setParameter(1, threadId)
      .executeUpdate();
    }

    public void updateThreadHit(int threadId) {
      op.updateFirst(
        new Query(new Criteria("threadId").is(threadId)),
        new Update().inc("hit", 1),
        PostThread.class
      );
    }


{:class="go-to-index"}
[Java Spring](index)
