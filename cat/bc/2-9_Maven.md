# 웹 프로그래밍 - Maven
## Maven 이란?
- 애플리케이션을 개발하기 위해 반복적으로 진행해왔던 작업들을 지원하기 위하여 등장한 도구
- Maven을 사용하면 빌드, 패키징, 문서화, 테스트와 테스트 리포팅, git, 의존성관리, svn등과 같은 형상관리서버와 연동, 배포 등의 작업을 손쉽게 할 수 있다.
- CoC(Convention over Configuration)에 익숙하면 쉽게 사용할 수 있다.
	- 일종의 관습, 예를 들어 소스파일의 위치, 컴파일 파일들의 위치 등을 미리 정해놓은 것

### Maven을 사용할 때의 장점
- 편리한 의존성 라이브러리 관리
	- 직접 다운로드 받거나 하는 것을 하지 않아도 라이브러리를 사용할 수 있다.
- 팀원들과 일관된 방식으로 빌드를 수행할 수 있다.
- 다양한 플러그인을 자동화

### Maven 기본
- pom.xml
- 태그의 의미
	- project : pom.xml 파일의 최상위 루트 엘리먼트(Root Element)입니다.
	- modelVersion : POM model의 버전입니다. 
	- groupId : 프로젝트를 생성하는 조직의 고유 아이디를 결정합니다. 일반적으로 도메인 이름을 거꾸로 적습니다.
	- artifactId : 해당 프로젝트에 의하여 생성되는 artifact의 고유 아이디를 결정합니다. Maven을 이용하여  pom.xml을 빌드할 경우 다음과 같은 규칙으로 artifact가 생성됩니다. artifactid-version.packaging. 위 예의 경우 빌드할 경우 examples-1.0-SNAPSHOT.jar 파일이 생성됩니다.
	- packaging : 해당 프로젝트를 어떤 형태로 packaging 할 것인지 결정합니다. jar, war, ear 등이 해당됩니다.
	- version : 프로젝트의 현재 버전. 추후 살펴보겠지만 프로젝트가 개발 중일 때는 SNAPSHOT을 접미사로 사용합니다. Maven의 버전 관리 기능은 라이브러리 관리를 편하게 합니다.
	- name : 프로젝트의 이름입니다.
	- url : 프로젝트 사이트가 있다면 사이트 URL을 등록하는 것이 가능합니다.

### Maven 프로젝트 시작하기
1. 이클립스 File -> New -> Project
2. Maven Project 선택 -> Next(Archetype 선택) -> org.apache.maven.archetypes // maven-archetype-webapp 선택 -> Next
	- archetype: 일종의 프로젝트 템플릿, 자동으로 여러 파일, 라이브러리를 셋팅해준다.
3. project 정보 입력 -> finish
4. 이때는 프로젝트의 java 버전이 1.5 또는 1.7이다. 이 버전을 1.8로 변경해준다.(properties - Java Compiler에서 확인 가능)
5. pom.xml에 추가(<plugins> 밑에 추가)
```
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-compiler-plugin</artifactId>
	<version>3.6.1</version>
	<configuration>
		<source>1.8</source>
		<target>1.8</target>
	</configuration>
</plugin>
```
6. properties - Maven/Java EE Integration -> Enable Project Specific Settings 앞의 체크박스를 선택
7. 이후에도 1.8이 되지 않았을 경우
	1. Java Bulid Path - Edit - 버전 1.8 선택 -> 저장
	2. properties - Project Facets -> Java 버전 변경 
	3. 사용자 폴더 하위에 .m2 폴더 삭제
	4. 이클립스내에서 프로젝트 삭제(disk에서는 삭제 x) 후 리빌드
8. webapp/index.jsp 의 오류 해결하기
- pom.xml에 추가
```
<dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
</dependency>
```
- scope태그설명 : tomcat안에 서블릿 라이브러리가 있기 때문에 런타임때는 제외해도 된다.
	- compile : 컴파일 할 때 필요. 테스트 및 런타임에도 클래스 패스에 포함됩니다. -> scope 을 설정하지 않는 경우 기본값입니다.
	- runtime : 런타임에 필요. JDBC 드라이버 등이 예가 됩니다. 컴파일 시에는 필요하지 않지만, 실행 시에 필요한 경우입니다.
	- test : 테스트 코드를 컴파일 할 때 필요. 테스트 시 클래스 패스에 포함되며, 배포 시 제외됩니다.
9. JSTL 라이브러리 추가
- pom.xml
```
<dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
</dependency>
```
10. EL 사용을 위해 다이나믹 웹 모듈 3.1이 되도록 설정
- WEB-INF/web.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" version="3.1">
  <display-name>Archetype Created Web Application</display-name>
</web-app>
```
-  .settings/org.eclipse.wst.common.project.facet.core.xml (navigator을 이용하여 연다.)
```
<?xml version="1.0" encoding="UTF-8"?>
<faceted-project>
  <fixed facet="wst.jsdt.web"/>
  <installed facet="jst.web" version="3.1"/>
  <installed facet="wst.jsdt.web" version="1.0"/>
  <installed facet="java" version="1.8"/>
</faceted-project>
```
11. project update





참고 : [부스트코스](https://www.edwith.org/boostcourse-web/lecture/16723/)
