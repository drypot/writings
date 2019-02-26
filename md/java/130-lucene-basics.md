# Lucene 기초

2011-02-07 17:55

(아래 글을 쓴 시점은 2011-02-07 인데 루씬 API 가 또 많이 바뀌었습니다. 참고로만 보세요.)

### 들어가는 글

루씬은 자바 플랫폼에서 사용할 수 있는 검색엔진입니다.
검색엔진이라는 말이 광범위하게 이해될 수 있는데 루씬 자체에 크라울러 같은 것은 없고,
스트링을 던지면 파싱하고, 인덱싱해주고, 추후 쿼리를 던지면 인덱스 레코드를 돌려주는 정도입니다.
쉽게 이해하기 위해 게시판 시스템의 내부 검색에 활용하기 적합하다고 보시면 되겠습니다.

루씬을 전반적으로 이해하는데 도움될만한 웹 문서는 없습니다.
Lucene in Action 1st Edition 이 한글로 번역되어 있는데 3.X 에서 구조가 많이 바뀌어서 활용하기 어려우실 겁니다.
그래서 현재 루씬을 위한 유일한 문서는 Lucene in Action 2nd Edition 뿐입니다.

MySQL Guide 를 읽지 않고 MySQL 쓰기 어렵듯이,
아무리 도우미 라이브러리를 쓴다고 해도 루씬 문서를 읽지 않고 루씬을 제대로 활용하기는 좀 어렵습니다.
그래서 국내에 루씬 쓰는 사람이 별로 없는 것 같습니다.

게다가 루씬이 편하게 쓸 수 있도록 만들어져 있지를 않습니다.
아무리 간단한 작업을 해도 입출력 관련 도우미 루틴을 직접 만들던지 가져다 쓰셔야 할 겁니다.

현업에서 루씬 원서를 붙들고 있을 수 있는 분이 많지를 않을 것이고, 거기에 검색은 해야겠고,
해서, 제가 루씬 책 보면서 개인적으로 사용하려고 만든 루씬 예제 클래스 4 개를 예로 보여드리겠습니다.

왜 해외의 그럴듯한 루씬 확장 라이브러리 가지고 설명을 안 하고 제 것으로 하냐면,
제가 그것들을 들여다 본 적이 없고, 제 코드가 아주 최소 기능만 구현해서 루씬을 설명하기 좋기 때문입니다.

책에 있는 디테일을 다 전해드리기는 어렵겠지만
전체적인 운영법 이해와 당장 작동하는 코드를 보실 수는 있을 것 같습니다.


### LuceneIndex

루씬은 사용자가 지정한 디렉토리를 하나 잡아서 인덱스 파일들을 관리합니다.

루씬인덱스에 데이터를 추가할 때는 IndexWriter 타입 오브젝트를 사용합니다.
검색을 할 때는 IndexSearcher 타입 오브젝트를 사용하고요.
3.X 에 와서 좀 좋아지긴 했는데도 불구하고 아직 이놈들 쓰기가 편하지가 않습니다.

이 두 오브젝트는 리소스를 많이 먹기 때문에 인덱스 디렉토리별로 한 놈씩만 만들어 써야합니다.
여러 검색 요청이 동시에 들어와도 IndexSearcher 는 기본적으로 하나만 운영한다는 말입니다.

IndexWriter 에 추가되는 내용은 바로 디스크에 저장되지 않습니다.
일단 메모리 버퍼에 가지고 있다가 메모리가 꽉 차면 디스크 flush 합니다.
버퍼 크기는 사용자가 지정할 수 있습니다.

디스크에 flush 했다고 데이터 베이스 상태가 갱신되진 않습니다.
추가된 내용을 검색에 사용 가능한 상태로 만들기 위해서는 commit 을 해야합니다.
루씬에서는 이 용어들의 엄밀한 의미가 다른 환경에서와 달라 헷갈릴 수 있으니 문서를 읽을 때는 주의하셔야 합니다.

루씬에선 변경 내용을 마스터 파일에 바로 업데이트하는 것이 아니고,
변경 단위를 segment 라고 칭하여 별도 파일에 저장하는데,
commit 할 때마다 이 세그먼트 파일들이 계속 늘어납니다.

commit 하다 보면 세그먼트 파일들이 계속 만들어져서 파일이 수백 개가 되다가,
리눅스 환경에서는 파일을 더 이상 열 수 없다는 오류를 내면서 서버가 동작불능 상태가 됩니다.

그래서, 모든 semgent 파일들을 마스터 파일에 업데이트하는 과정이 필요한데
이를 optimizing 이라고 합니다.

예상하셨겠지만, commit, optimizaing 을 수동으로 해줘야합니다.
이를 위해 별도 쓰레드를 운영해야하고, 기본적인 도우미 라이브러리가 필요합니다.

IndexWriter 와 IndexSearcher 관계도 사용하기 편리하지 않습니다.

같은 인덱스 데이터베이스에 대해 IndexWriter 와 IndexSearcher 를 하나씩 만드는데
IndexSeacher 가 생성된 후에 IndexWriter 에 commit 되는 내용은 IndexSearcher 에 반영이 안 됩니다. =,=
그래서, 일정 기간마다 이 IndexSearcher 를 새로 만들어줘야 하는데,
보통 IndexWriter 의 commit 후 IndexSearcher 를 새로 만들게 됩니다.

문제가 여기서 끝나지 않습니다. =,=

검색결과를 표시하는 루틴에서 IndexSearcher 오브젝트를 쓰기 때문에
commit 용 백그라운드 스레드에서 이것을 맘대로 close 해버릴 수가 없습니다.
그래서 IndexSearcher 오브젝트 변수를 두 벌 운영합니다.

위 내용을 구현한 루씬 최저 레벨 API 사용 예가 아래에 있습니다.

	package com.drypot.lucene;

	import java.io.File;
	import java.io.IOException;

	import org.apache.lucene.analysis.cjk.CJKAnalyzer;
	import org.apache.lucene.document.Document;
	import org.apache.lucene.index.CorruptIndexException;
	import org.apache.lucene.index.IndexWriter;
	import org.apache.lucene.index.IndexWriter.MaxFieldLength;
	import org.apache.lucene.index.Term;
	import org.apache.lucene.queryParser.QueryParser;
	import org.apache.lucene.search.IndexSearcher;
	import org.apache.lucene.search.Query;
	import org.apache.lucene.search.ScoreDoc;
	import org.apache.lucene.search.Sort;
	import org.apache.lucene.search.SortField;
	import org.apache.lucene.store.FSDirectory;
	import org.apache.lucene.store.LockObtainFailedException;
	import org.apache.lucene.util.Version;
	import org.slf4j.Logger;
	import org.slf4j.LoggerFactory;

	public class LuceneIndex {
		private static final Logger logger = LoggerFactory.getLogger(LuceneIndex.class);
		private static final Version version = Version.LUCENE_30;

		// 인덱스 파일들을 기록할 디렉토리를 지정
		private FSDirectory directory;

		private IndexWriter writer;

		// 검색이 다른 쓰레드에서 진행중일 수 있기 때문에
		// IndexSearcher 변수를 두 벌 운영해서 사용중인 searcher 가 close 되버리는 상황을 방지합니다.
		private IndexSearcher searcher;
		private IndexSearcher discardedSearcher;

		private Thread refreshThread;

		// 적정 commit 횟수가 넘어가면 optimizing 을 하기 위한 카운터
		private int commitCount;

		// createNewIndex 에 true 가 넘어오면 인덱스를 새로 만듭니다.
		public LuceneIndex(String path, boolean createNewIndex) {
			try {
				initDirectory(path);
				initWriter(createNewIndex);
				initSearcher();
				initRefreshThread();
			} catch (RuntimeException e) {
				throw e;
			} catch (Exception e) {
				throw new RuntimeException(e);
			}
		}

		private void initDirectory(String path) throws IOException {
			directory = FSDirectory.open(new File(path));
		}

		private void initWriter(boolean createNewIndex) throws CorruptIndexException, LockObtainFailedException, IOException {
			// 한,중,일 언어권에서는 기본적으로 CJK 분석기를 사용합니다.
			// 원하시면 다른 분석기로 바꾸셔도 됩니다.
			CJKAnalyzer analyzer = new CJKAnalyzer(version);
			writer = new IndexWriter(directory, analyzer, createNewIndex, MaxFieldLength.LIMITED);
		}

		private void initSearcher() throws CorruptIndexException, IOException {
			searcher = new IndexSearcher(directory, true);
			discardedSearcher = null;
		}

		// 백그라운드로 돌면서 10 분에 한번씩 commit 과 관련한 기능 묶음을 실행합니다.
		private void initRefreshThread() {
			refreshThread = new Thread() {
				@Override
				public void run() {
					try {
						while (true) {
							Thread.sleep(1000 * 60 * 10);
							try {
								commit(false);
							} catch (Exception e) {
								e.printStackTrace();
							}
						}
					} catch (InterruptedException e) {
					}
				}
			};
			refreshThread.start();
			logger.info("Started");
		}

		// commit 관련된 주요 기능들이 모여있습니다.
		// commit 이 64 회 반복되면 optimizing 을 합니다. 64 는 제가 임의로 정한 수치.
		// commit 이후에는 IndexSearcher 를 재생성합니다.
		public synchronized void commit(boolean shouldOptimize) {
			try {
				if (commitCount >= 64 || shouldOptimize) {
					commitCount = 0;
					logger.info("Optimizing");
					writer.optimize();
				} else {
					commitCount++;
				}
				writer.commit();
				if (discardedSearcher != null)
					discardedSearcher.close();
				discardedSearcher = searcher;
				searcher = new IndexSearcher(directory, true);
			} catch (RuntimeException e) {
				throw e;
			} catch (Exception e) {
				throw new RuntimeException(e);
			}
		}

		public void dispose() {
			try {
				refreshThread.interrupt();
				writer.close(true);
				if (discardedSearcher != null)
					discardedSearcher.close();
				searcher.close();
				logger.info("Disposed");
			} catch (RuntimeException e) {
				throw e;
			} catch (Exception e) {
				throw new RuntimeException(e);
			}
		}

		// 인덱스를 리빌드 해야할 경우에 사용할 옵션들을 모아 놓았습니다.
		public void setOptionsForReindex() {
			writer.setRAMBufferSizeMB(64);
			writer.setMergeFactor(1000);
		}

		// 게시판의 정상 온라인 서비스 상태에서 사용할 옵션들을 모아 놓았습니다.
		public void setOptionsForOltp() {
			writer.setRAMBufferSizeMB(16);
			writer.setMergeFactor(10);
		}

		// 비즈니스 로직에서 인덱스에 새로 추가할 내용을 Document 오브젝트로 감싸 던지면 IndexWriter 에 추가합니다.
		public void addDocument(Document doc) {
			try {
				writer.addDocument(doc);
			} catch (RuntimeException e) {
				throw e;
			} catch (Exception e) {
				throw new RuntimeException(e);
			}
		}

		// 비즈니스 로직에서 이미 존재하는 인덱스 레코드의 내용을 업데이트할 필요가 있을 때 사용합니다.
		public void updateDocument(Term term, Document doc) {
			try {
				writer.updateDocument(term, doc);
			} catch (RuntimeException e) {
				throw e;
			} catch (Exception e) {
				throw new RuntimeException(e);
			}
		}

		// 검색을 실행하는 최하위 코드입니다.
		// 루씬에 검색을 요청하면 ScoreDoc 어레이를 리턴하는데 이때 사용한 IndexSearcher 를 루프중에도 계속 끌고 다녀야 합니다.
		// ScoreDoc 은 기능이 원시적이라 바로 사용하기 불편하니 LuceneSearchResult 도우미 클래스로 감싸서 리턴합니다..
		public LuceneSearchResult search(String q, String contentFieldName, SortField sortField) {
			IndexSearcher activeSearcher = searcher;
			ScoreDoc[] hits;
			Sort sort = new Sort(sortField);
			try {
				CJKAnalyzer analyzer = new CJKAnalyzer(version);
				QueryParser parser = new QueryParser(version, contentFieldName, analyzer);
				parser.setDefaultOperator(QueryParser.Operator.AND);
				Query query = parser.parse(q);
				hits = activeSearcher.search(query, null, 1000, sort).scoreDocs;
			} catch (RuntimeException e) {
				throw e;
			} catch (Exception e) {
				hits = new ScoreDoc[0];
			}
			return new LuceneSearchResult(activeSearcher, hits);
		}
	}

### LuceneSearchResult

LuceneSearchResult 클래스는
ScoreDoc 어레이드와 IndexSearcher 를 같이 가지고 다니는 역할 +
메서드 명 이뿌게 만드는 역할을 합니다.

아래 코드를 보시면 아시겠지만 루신의 검색 결과 어레이에는 Document ID 만 들어있기 때문에
실제 Doucment 를 끄집어 내려면 검색 당시 사용했던 IndexSearcher 를 다시 써야합니다.
게다가 Java Convention 을 무시하는 경우가 많아서 getDocument() 대신에 doc() 같은 메서드 명을 쓰고 있습니다.
처음 보면 좀 헷갈리지요.

	package com.drypot.lucene;

	import org.apache.lucene.document.Document;
	import org.apache.lucene.search.IndexSearcher;
	import org.apache.lucene.search.ScoreDoc;

	public class LuceneSearchResult {

		private IndexSearcher activeSearcher;
		private ScoreDoc[] hits;

		public LuceneSearchResult(IndexSearcher activeSearcher, ScoreDoc[] hits) {
			this.activeSearcher = activeSearcher;
			this.hits = hits;
		}

		public int getSize() {
			return hits.length;
		}

		public Document getDocument(int i) {
			try {
				return activeSearcher.doc(hits[i].doc);
			} catch (RuntimeException e) {
				throw e;
			} catch (Exception e) {
				throw new RuntimeException(e);
			}
		}

	}

### SearchService

LuceneIndex 와 LuceneSearchResult 가 프로젝트 독립적으로 사용하려고 만든 도우미 클래스라면
SearchService 는 프로젝트 별로 꾸며야하는 비즈니스 로직의 일부입니다.

SearchService 에서 Service 의 의미는 스프링에서 컨트롤러, 도메인, 서비스, 뷰 등을 말할 때 바로 그 서비스입니다.

코드에 이해하기 어려운 부분은 없는데,
루씬이 레코드를 저장할 때 각 필드를 어떻게 달리 다루는가는 이해하실 필요가 있습니다.
필드별로 두 가지 선택해줘야할 것이 있습니다.

한 가지는, 각 필드를 형태소 분석할 것인지 말 것인지 정하는 것입니다.
쉽습니다. 글번호는 형태소 분석을 안 해도 되겠죠? 걍 그대로 넣으면 됩니다. 날짜도 안 해도 됩니다.
하지만, 제목, 글 내용은 형태소 분석을 해둬야겠지요?
이걸 정해줘야합니다.

또 한 가지는, 필드 원본 값을 저장해야하나 말아냐하나 입니다.
값을 전달하면 당연히 저장해야할 것같은데 그렇지 않은 값들이 있습니다.
주로 형태소 분석하는 필드들이 이에 해당하는데,
글 내용은 형태소 분석을 해서 검색을 위한 정보만 만들어 놓으면 됐지
원본 글을 루씬 데이터베이스에 중복 저장할 필요가 없을 수도 있습니다.
필요하면 RDB 에서 다시 가져오면 되니까요.

필드처리 부분만 신경쓰면서 아래 검색서비스 로직 코드를 보시면 될 것 같습니다.

스프링 프레임웍 위에 있던 코드라 스프링 와이어링 구문들이 좀 보입니다.

하이버네이트와는 연동시키지 않았습니다.
이 코드 만들 때는 하이버네이트 쓰기 전이었기도 하고,
여차하면 하이버네이트를 내버릴 수 있게 만들려고 JDBC 를 사용하였습니다.
JDBC 인터페이스는 스프링 SimpleJDBC 가 아니고 제가 사제로 만든 JDBC 라이브러리를 쓰고 있는데,
이건 딱 보시면 뭐하는지 이해가 되실 터이니 쓰고 계신 DB API 로 바꾸시면 되겠습니다.

	package com.drypot.sleek.service;

	import java.io.File;

	import javax.annotation.PostConstruct;
	import javax.annotation.PreDestroy;
	import javax.sql.DataSource;

	import org.apache.lucene.document.DateTools;
	import org.apache.lucene.document.Document;
	import org.apache.lucene.document.Field;
	import org.apache.lucene.index.Term;
	import org.apache.lucene.search.SortField;
	import org.joda.time.DateTime;
	import org.slf4j.Logger;
	import org.slf4j.LoggerFactory;
	import org.springframework.beans.factory.annotation.Autowired;
	import org.springframework.stereotype.Component;

	import com.drypot.lucene.LuceneIndex;
	import com.drypot.lucene.LuceneSearchResult;
	import com.drypot.sleek.App;
	import com.drypot.sleek.domain.persist.Post;
	import com.drypot.sleek.domain.persist.PostThread;
	import com.drypot.sleek.service.support.PostSearcher;
	import com.drypot.sql.SqlConnection;
	import com.drypot.sql.SqlRow;
	import com.drypot.sql.SqlStatement;

	@Component
	public class SearchService {
		private Logger logger = LoggerFactory.getLogger(SearchService.class);

		// 환경설정 읽어 놓는 오브젝트, 루씬 데이터베이스 저장에 사용할 디렉토리 패스를 가져옴.
		@Autowired private App app;

		// 루씬 디비를 리빌드할 때 사용할 데이터소스.
		@Autowired private DataSource dataSource;

		// 지난 글을 통해 설명드렸습니다. 루씬 입출력을 편하게 만들기 위한 도우미 클래스.
		private LuceneIndex index;

		// 검색결과를 출력할 때 날짜 순으로 소팅하기 위한 상수 선언.
		private SortField sortField = new SortField("cdate", SortField.STRING, true);

		// 스프링 오브젝트 초기화 메서드 지정 어트리뷰트
		@PostConstruct
		public void init() {
			// 여러 루씬 인덱스 그룹을 운영할 수도 있으니 루씬 루트 폴더를 안 쓰고
			//  게시글 전용 post 서브 폴더를 사용하기로 했습니다.
			String luceneDataDir = app.getConfig().getString("luceneData/dir");
			String postDir =  luceneDataDir + "/post";
			logger.info("Lucene Data Directory: " +  luceneDataDir);

			// 폴더가 있으면 기존 인덱스를 사용하고,
			// 폴더가 없으면 루씬 인덱스 초기화 후, RDB 내용을 전부 읽어 루씬 인덱스를 리빌드 합니다.
			// 리빌드 코드는 한참 아래에
			if (new File(postDir).exists()) {
				index = new LuceneIndex(postDir, false);
			} else {
				index = new LuceneIndex(postDir, true);
				reindex();
			}
		}

		@PreDestroy
		public void dispose() {
			index.dispose();
		}

		public void newPost(PostThread postThread, Post post, DateTime now) {
			newPost(post.getId(), postThread.getTitle(), post.getUserName(), post.getText(), now);
		}

		public void newPost(Post post, DateTime now) {
			newPost(post.getId(), "", post.getUserName(), post.getText(), now);
		}

		// 새글이 들어왔을 때 루씬 인덱스에 새 레코드를 추가합니다.
		private void newPost(int postId, String title, String userName, String text, DateTime now) {
			// IndexWriter 오브젝트에는 Document 타입으로 정보를 전달합니다.
			Document doc = new Document();

			// 날짜 항목을 넣고 있고요, 소팅에만 사용할 필드는 Store.NO, Index.NOT_ANALYZED 지정을 합니다.
			// 긴 이야기는 루씬 문서를 참고할 필요.
			doc.add(new Field("cdate", DateTools.dateToString(now.toDate(), DateTools.Resolution.SECOND), Field.Store.NO, Field.Index.NOT_ANALYZED));
			// 글번호, 저장은 해야겠고, 형태소 분석은 하지 않습니다.
			doc.add(new Field("postId", Integer.toString(postId), Field.Store.YES, Field.Index.NOT_ANALYZED));
			// 어짜피 모든 항목을 한꺼번에 검색할 것이라 제목, 내용, 작성자명을 한 필드에 합해 넣었습니다.
			// 원본은 저장하지 않고요, 형태소 분석은 해야겠죠.
			doc.add(new Field("content", title + " " + userName + " " + text, Field.Store.NO, Field.Index.ANALYZED));
			index.addDocument(doc);
		}

		public void editPost(PostThread postThread, Post post, DateTime now) {
			editPost(post.getId(), postThread.getTitle(), post.getUserName(), post.getText(), now);
		}

		public void editPost(Post post, DateTime now) {
			editPost(post.getId(), "", post.getUserName(), post.getText(), now);
		}

		// 글 수정에 사용할 코드인데, 새 글추가의 경우와 거의 비슷하지만 기존 레코드의 값을 덮어쓰기 위해
		// 글 번호를 값으로 갖는 Term 을 함께 전달합니다.
		public void editPost(int postId, String title, String userName, String text, DateTime createDate) {
			Document doc = new Document();
			String postIdStr = Integer.toString(postId);
			doc.add(new Field("cdate", DateTools.dateToString(createDate.toDate(), DateTools.Resolution.SECOND), Field.Store.NO, Field.Index.NOT_ANALYZED));
			doc.add(new Field("postId", postIdStr, Field.Store.YES, Field.Index.NOT_ANALYZED));
			doc.add(new Field("content", title + " " + userName + " " + text, Field.Store.NO, Field.Index.ANALYZED));
			index.updateDocument(new Term("postId", postIdStr), doc);
		}

		// 검색결과를 만들어주는 코드.
		// 루씬 검색결과에는 글 원문이 없기 때문에 RDB 내용과 섞어 출력하기 위해
		// 도우미 클래스 PostSearcher 를 리턴합니다.
		public PostSearcher getPostSearcher(String query, int page) {
			if (query.length() == 0) {
				return new PostSearcher(null, null, 0);
			}
			LuceneSearchResult luceneResult = index.search(query, "content", sortField);
			return new PostSearcher(dataSource, luceneResult, page);
		}

		//
		// reindex
		//

		// 여기서 부터 인덱스 리빌드 코드입니다.
		// 특이사항은 없고요,  RDB 레코드 쭉 읽어 위에 소개된 메서드 가지고 루씬 디비에 때려 넣습니다.

		public void reindex() {
			logger.info("Reindex Starting");
			reindexOptimizingOptions();
			logger.info("Reindex Completed");
		}

		private void reindexOptimizingOptions() {
			index.setOptionsForReindex();
			reindexPostThreads();
			index.commit(true);
			index.setOptionsForOltp();
		}

		private void reindexPostThreads() {
			SqlConnection conn = new SqlConnection(dataSource);
			SqlStatement postQuery = conn.prepareStatement("select id, username, text, cdate from post where postthreadid = ?");
			for (SqlRow postThread : conn.prepareStatement("select id, title, cdate from postthread").executeQuery()) {
				reindexPostThread(postThread.getInt(), postThread.getString(), postThread.getDateTime(), postQuery);
			}
			conn.close();
		}

		private void reindexPostThread(int postThreadId, String title, DateTime postThreadCDate, SqlStatement postQuery) {
			for (SqlRow post : postQuery.addParam(postThreadId).executeQuery()) {
				reindexPost(title, postThreadCDate, post.getInt(), post.getString(), post.getString(), post.getDateTime());
			}
		}

		private void reindexPost(String title, DateTime postThreadCDate, int postId, String userName, String text, DateTime postCDate) {
			boolean isFirst = postCDate.equals(postThreadCDate);
			if (isFirst) {
				newPost(postId, title, userName, text, postCDate);
			} else {
				newPost(postId, "", userName, text, postCDate);
			}
		}

	}

### PostSearcher

PostSearcher 는 재활용해 쓰실 수 있는 부분이 거의 없을 것 같습니다.
루씬 검색 결과 + RDB + 페이징 정보를 섞어 PostSearchResult 타입 List 오브젝트를 돌려주는 예입니다.

	package com.drypot.sleek.service.support;

	import java.util.ArrayList;
	import java.util.List;

	import javax.sql.DataSource;

	import com.drypot.lucene.LuceneSearchResult;
	import com.drypot.sleek.domain.PostSearchResult;
	import com.drypot.sql.SqlConnection;
	import com.drypot.sql.SqlRow;
	import com.drypot.sql.SqlStatement;
	import com.drypot.util.paging.DescPager;
	import com.drypot.util.paging.Pager;

	public class PostSearcher {

		private LuceneSearchResult luceneSearchResult;
		private Pager pager;
		private List<PostSearchResult> list;

		public PostSearcher(DataSource dataSource, LuceneSearchResult luceneSearchResult, int page) {
			this.luceneSearchResult = luceneSearchResult;
			if (luceneSearchResult == null) return;
			pager = new DescPager(luceneSearchResult.getSize(), 15, page, 10);
			fillList(dataSource);
		}

		public boolean hasNoQuery() {
			return luceneSearchResult == null;
		}

		public boolean hasNoResult() {
			return list.isEmpty();
		}

		public Pager getPager() {
			return pager;
		}

		public List<PostSearchResult> getList() {
			return list;
		}

		private void fillList(DataSource dataSource) {
			fillPrepareStatement(dataSource);
		}

		private void fillPrepareStatement(DataSource dataSource) {
			SqlConnection conn = new SqlConnection(dataSource);
			try {
				SqlStatement s = conn.prepareStatement(
					"select postthread.id, postthread.postcategoryid, postthread.title, post.username, post.cdate, " +
					"substr(post.text, 1, 256) as text, (postthread.cdate = post.cdate) as isfirst " +
					"from post join postthread on post.postthreadid = postthread.id and post.id = ?"
				);
				fillCalcRange(s);
			} finally {
				conn.close();
			}
		}

		private void fillCalcRange(SqlStatement s) {
			int begin = pager.getCurrentPageBeginRow();
			int end = begin + pager.getCurrentPageRows();
			fillLoop(begin, end, s);
		}

		private void fillLoop(int i, int end, SqlStatement s) {
			ArrayList<PostSearchResult> list = new ArrayList<PostSearchResult>(15);
			while (i < end) {
				int postId = Integer.parseInt(luceneSearchResult.getDocument(i).get("postId"));
				list.add(fillGetRow(s, postId));
				i++;
			}
			this.list = list;
		}

		private PostSearchResult fillGetRow(SqlStatement s, int postId) {
			SqlRow row = s.addParam(postId).executeQuery().getSingleRow();
			return new PostSearchResult(postId, row.getInt(), row.getInt(), row.getString(), row.getString(), row.getDateTime(), row.getString(), row.getBoolean());
		}

	}

,

	package com.drypot.sleek.domain;

	import org.joda.time.DateTime;

	public class PostSearchResult {

		private int postId;
		private int postCategoryId;
		private int postThreadId;
		private String title;
		private String userName;
		private DateTime createDate;
		private String text;
		private boolean first;

		public PostSearchResult (int postId, int postThreadId, int postCategoryId, String title, String userName, DateTime createDate, String text, boolean first) {
			this.postId = postId;
			this.postThreadId = postThreadId;
			this.postCategoryId = postCategoryId;
			this.title = title;
			this.userName = userName;
			this.createDate = createDate;
			this.text = text;
			this.first = first;
		}

		public int getPostId() {
			return postId;
		}

		public int getPostCategoryId() {
			return postCategoryId;
		}

		public int getPostThreadId() {
			return postThreadId;
		}

		public String getTitle() {
			return title;
		}

		public String getUserName() {
			return userName;
		}

		public DateTime getCreateDate() {
			return createDate;
		}

		public String getText() {
			return text;
		}

		public boolean isFirst() {
			return first;
		}

	}

