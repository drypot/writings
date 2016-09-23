# Tomcat URI Encoding

2010-11-29 16:06

	<Connector
	    port="8080"
	    protocol="org.apache.coyote.http11.Http11NioProtocol"
	    maxPostSize="536870912"
	    URIEncoding="UTF-8"
	    enableLookups="false"
	/>

데이터를 URL 인자로 보낼 때,
특정 Character Encoding 의 스트링을, 브라우저가 URL Encoding 으로 싸서 보내는데,
Tomcat 이 URL Encoding 은 벗길 수 있지만, 원래 스트링의 Character Encoding 은 알 수가 없습니다.

Connector 에 URIEncoding 어트리뷰트를 적어서 Character Encoding 을 알려줍니다.
Post 시 Body Encoding 은 이것과 상관 없습니다.

tomcat-base/conf/server.xml 파일에 들어있습니다