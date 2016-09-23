# Intellij IDEA : 디스어셈블러 연결하는 법

2011-08-30

http://arhipov.blogspot.com/2011/08/whats-cool-in-intellijidea-part-iii.html?spref=tw

요약하면,

Settings 윈도우를 부릅니다. 맥에서는 cmd-,
External Tools 항목에 가서 Add.

Name: javap
Group: JDK, 정도
Program: javap
Parameters: -c -p -classpath $OutputPath$ $FileClass$

저장하시고,

에디터에서 컨텍스트 메뉴 부르시면 JDK 라는 메뉴가 생겼을 겁니다.
javap 실행.

순식간에 실행되어 버리는데 Run 콘솔 찾아서 결과 확인.
Edit
drypot 2011-08-30 15:30
그냥 ASM Bytecode Outline 플러그인 설치하는 것이 낫군요. =o=
