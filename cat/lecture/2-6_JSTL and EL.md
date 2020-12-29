# 웹 프로그래밍 - JSTL & EL

## EL(Expression Language)

### 표현 언어란?

- 값을 표현하는 데 사용되는 스크립트 언어, JSP의 기본 문법을 보완하는 역할

**표현 언어가 제공하는 기능**

- JSP의 스코프(scope)에 맞는 속성 사용
- 집합 객체에 대한 접근 방법 제공
- 수치 연산, 관계 연산, 논리 연산자 제공
- 자바 클래스 메소드 호출 기능 제공
- 표현언어만의 기본 객체 제공



### 표현언어의 표현방법

- ${expr} - 표현언어가 정의한 문법에 따라 값을 표현하는 식
  - \<b>${sessionScope.member.id}\</b>

- ${<표현1>,<표현2>}
  - 표현 1이나 표현 2가 null이면 null을 반환한다.
  - 표현1이 Map일 경우 표현2를 key로한 값을 반환한다.
  - 표현1이 List나 배열이면 표현2가 정수일 경우 해당 정수 번째 index에 해당하는 값을 반환한다.
  - 만약 정수가 아닐 경우에는 오류가 발생한다.
  - 표현1이 객체일 경우는 표현2에 해당하는 getter메소드에 해당하는 메소드를 호출한 결과를 반환한다.

##### 표현언어(EL) 비활성화 : JSP에 명시하기

- <%@ page isELIgnored = "true" %>



## JSTL(JSP Standard Tag Library)

- JSP 페이지에서 조건문 처리, 반복문 처리 등을 html tag형태로 작성하도록 도와준다.

### 코어 태그: 변수 지원 태그

- set(변수 생성), remove(변수 제거)

```jsp
<c:set var ="varName" scope="session" value="someValue" />
<c:set var="varName" scope="request">
some Value
</c:set>
이름 : ${varName} <br>
<c:remove var="varName" scope="request"/>
```

- 프로퍼티, 맵의 처리
  - 특정한 객체의 메서드에 값을 전달 할 수 있다.

```jsp
<c:set target="${some}" property="propertyName" value="anyValue"/>
```

> 이때 some객체가 자바빈일 경우: some.setPropertyName(anyvalue);
>
> 맵일 경우: some.put(propertyName, anyValue);

- 흐름제어 태그 - if 조건문(else 처리는 없다)

```jsp
<c:if test="${n == 0}">
n은 과 0과 같습니다.
</c:if>
```

- 흐름제어 태그 - choose

```jsp
<c:choose>
    <c:when test="${score >=90 }">
    A학점입니다.
    </c:when>
    <c:when test="${score >=80 }">
    B학점입니다.
    </c:when>
    <c:when test="${score >=70 }">
    C학점입니다.
    </c:when>
    <c:otherwise>
    F학점입니다.
    </c:otherwise>            
</c:choose>
```

- 흐름제어 태그 - forEach(범위를 지정할 수도 있다)
  > <c:forEach var = "변수" items="아이템" [begin="시작번호"] [end="끝번호"]> </c:forEach>

```jsp
<c:forEach items="${list}" var="item">
${item } <br>
</c:forEach>
```























참고: [부스트캠프](https://www.edwith.org/boostcourse-web/lecture/16714/)