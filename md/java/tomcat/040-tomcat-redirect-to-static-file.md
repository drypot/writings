# Tomcat Redirect to Static File

2010-11-29 16:02

이미지 서비스등에서 톰켓에서 인증등 적절한 로직을 통과한 다음
스터틱 파일을 서비스해야할 시나리오가 있습니다.

임시파일 만들어서 아파치가 쏘게 하는 등 복잡하게 하지 말고
톰켓 자제 파일 서비스 성능이 좋으니 톰켓에서 브라우저로 바로 보내던지
톰켓에서 Proxy Socket 을 통해 프런트엔드 서버로 데이터를 바로 보내면 됩니다.

	public void send(HttpServletRequest req, HttpServletResponse res, String type, String dir, String fileName) {
		String path = baseDir + "/" + dir + "/" + fileName;
		File file = new File(path);
		try {
			res.setContentType(type); // "image/jpeg"
			res.setContentLength((int) file.length());
			req.setAttribute("org.apache.tomcat.sendfile.filename", file.getCanonicalPath());
			req.setAttribute("org.apache.tomcat.sendfile.start", 0L);
			req.setAttribute("org.apache.tomcat.sendfile.end", file.length());
		} catch (IOException e) {
			throw new RuntimeException(e);
		}
	}

위에 코드는 톰켓에서 파일을 최대한 효과적으로 전송하는 코드.
DefaultServlet 열어보니 저러고 있더군요. =o=
리퀘스트 어트리뷰트에 적절한 정보 넣어주면 코이요테 커넥터가 NIO 사용해서 파일을 바로 쏩니다.

그런데, 저 코드에는 브라우저 케쉬 관리용 Last-Modified 헤더 출력이나 요청파일 날짜 비교 루틴이 없습니다.
브라우저에서 리퀘스트 올 때마다 멍청하게 계속 쏩니다. =o=

참고 내용은 아래 문서에서 제일 밑바닥
<http://tomcat.apache.org/tomcat-7.0-doc/aio.html>