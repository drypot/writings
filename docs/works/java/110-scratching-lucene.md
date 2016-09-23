# Lucene

2009.11.27

슬릭 데이터 몽땅 인덱싱하는 셈플

	import org.apache.lucene.analysis.Analyzer
	import org.apache.lucene.analysis.cjk.CJKAnalyzer
	import org.apache.lucene.util.Version
	import org.apache.lucene.store.Directory
	import org.apache.lucene.store.NIOFSDirectory
	import org.apache.lucene.index.IndexWriter
	import org.apache.lucene.index.IndexWriter.MaxFieldLength
	import org.apache.lucene.document.Document
	import org.apache.lucene.document.Field
	import groovy.sql.Sql

	Analyzer analyzer = new CJKAnalyzer(Version.LUCENE_CURRENT)
	Directory directory = new NIOFSDirectory(new File("c:/app/sleek-lucene/post"))
	IndexWriter writer = new IndexWriter(directory, analyzer, true, MaxFieldLength.LIMITED)

	Sql sql = Sql.newInstance("jdbc:h2:tcp://localhost:9092/sleek;IFEXISTS=TRUE;FILE_LOCK=SOCKET;LOCK_MODE=0","sa","----")

	println "indexing postThead"
	sql.eachRow ("select * from postThread") { postThread ->
		Document doc = new Document()
		doc.add(new Field("postThread", (String)postThread.postThread, Field.Store.YES, Field.Index.NOT_ANALYZED))
		doc.add(new Field("userName", (String)postThread.userName, Field.Store.YES, Field.Index.NOT_ANALYZED))
		doc.add(new Field("title", (String)postThread.title, Field.Store.NO, Field.Index.ANALYZED))
		writer.addDocument(doc)
	}

	println "indexing post"
	sql.eachRow ("select * from post where postThread") { post ->
		Document doc = new Document()
		doc.add(new Field("post", (String)post.post, Field.Store.YES, Field.Index.NOT_ANALYZED))
		doc.add(new Field("postThread", (String)post.postThread, Field.Store.YES, Field.Index.NOT_ANALYZED))
		doc.add(new Field("userName", (String)post.userName, Field.Store.YES, Field.Index.NOT_ANALYZED))
		doc.add(new Field("text", (String)post.text, Field.Store.NO, Field.Index.ANALYZED))
		writer.addDocument(doc)
	}

	writer.optimize()
	writer.close()

	println "done"

2009.12.03

Lucene in Action 이 나온지가 좀 오래되서 현재 API 들하고 틀린 내용이 많은 것 같음.
PDF 기준으로 인덱스 추가와 업데이트를 같은 오브젝트로 못하게 되어 있어서 시껍했는데,
그닌까 인덱스 추가 기능있는 오브젝트 한 참 쓰다가, 있던 도큐먼트 지우고 업데이트 하려면 다른 놈 오픈해야함. =,=
물론 둘이 익스클루시브. =,=
크라울러 베이스에 있던 사람이 만들어서 이렇게 했나 싶었서, 이거 또 못쓰는거 아닌가 했는데,
최근 FAQ 읽어보니 다행히 추가 + 업데이트 한 오브젝트가 할 수 있게 몰아 놓은 것 같음.

리딩 끝났으니 코딩을 좀 해봐야할 듯.

2009.12.04

H2 는 이번에 페이징 모델로 바뀌더니만, 100 메가 먹던 데이터 파일이 260 메가 먹고 있음. =,=
이걸 루씬으로 인덱싱하니 인덱싱 파일 20 메가정도 떨어지네.
M$ FullText 엔진 썼을때는 인덱싱 파일이 본체보다 더 컸었는데,
루씬 옵션줄 때 다 빼버리고 프라이머리키하고 인덱싱 정보만 가지고 있으라 했더니 이리 작아지는 듯.

루씬 좋은 점은 다 조절할 수 있다는 것.
나쁜 점은 그래서 좀 귀찮다는 것. =,=
생각지도 못한 것들을 좀 신경써야함.

그리고, 오브젝트, 메서드, 프로퍼티, 네이밍 센스가 =,=
만든분 어디 사람인지 모르지만 또 비영어권 아닌가 싶음.

대충 다 붙였고,
글쓸 때마다 인덱싱 추가하는 부분 붙이면 되는데,
이거 커밋하는 정책도 몇 가지 중에 골라야하고,
근데 일정시간 지나면 커밋하는 건 없어서 또 수작업 좀 하고, =,=

더 좀 이상한 건 검색쪽 Searcher 를 정기적으로 다시 오픈해야 커밋한 데이터를 쓸 수 있음. =,=
오픈 시점까지만 데이터를 인식해서, =,=
10 분에 한번씩 알아서 재로딩하는 옵션좀 있으면 좋겠는데,
이것도 수작업해야함. =,=

정기적으로 데이터 머지나 옵티마이징 해주라는데,
머 배치작업할 수 있는 상황도 아니고,
OLTP 환경에서 쓰기는 먼가 어수선함. ^^

그래도 전반적으로는 훌륭한 것 같음.
검색이 많이 좋아졌음.

2009.12.04

autocommit 은 왜 빼버렸을까.

메모리 차면 flush 하는 로직은 내장인데,
flush 한다고 리더에 반영되는 것도 아니고 그냥 로우 데이터 하드에 미는데,
이게 끝이 아니고 commit 해야 관리 스트럭쳐가 저장되나 봄.
flush / commit 2 단계인데,
commit 은 완전 수동으로 불러야함. =,=

대충 쓰려고 했는데, 먼가 꽤 포장을 만들어야할 듯. =,=

Compass 인가 루신 레퍼가 있는데 불안해서, =,=
걍 수작업 몇 뼘해야.

2010.08.14

며칠전에 Lucene in Action 2nd Edition PDF 나왔군.
Lucene 3.0 기준.

5 년만에 업데이트. =o=
Edit
drypot 2010-11-29 13:51
2010.08.18

Lucene 은 진짜 이상한 것 같아요.
나름 잘 만든 것 같긴 한데 API 도 이상하고,
손잡이 없이 버튼과 엔진과 바퀴만 있어요. =,=

문서 이틀 보다가 결국 파일 왕창 열고 있는 문제는,,,
몇 시간마다 강제 옵티마이징 시켜 세그먼트들 모으는 것으로,,, =,=
