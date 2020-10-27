# 웹 프로그래밍 - Web UI 개발
## window 객체

- 전역객체

- window에는 많은 메서드 존재

```javascript
window.setTimeout()
setTimeout()//window는 전역객체라서 생략 가능하다.
```

### setTimeout 활용

- 인자로 함수를 받고, 보통 나중에 실행되는 콜백함수라고도 한다.
- 함수를 반환하기도 한다.

```javascript
function run() {
    setTimeout(function() {
        var msg = "hello codesquad";
        console.log(msg);  //이 메시지는 즉시 실행되지 않습니다.
    }, 1000);
}

run();
```

- setTimeout의 실행은 비동기(asynchronous)로 실행되어 동기적인 다른 실행이 끝나야 실행된다.



## Dom과 querySelector

### Dom(Document Object Model)

- 브라우저에서 HTML코드를 DOM이라는 객체형태의 모델로 저장된 정보를 DOM Tree라고 한다
- 즉, HTML element는 Tree형태로 저장된다. 
- 브라우저는 DOM Tree를 찾고 조작하는 걸 쉽게 도와주는 여러가지 메서드(DOM API)를 제공한다.
- Document - 최상위 객체, root

### getElementById()

- id 정보를 통해서 찾기

### **Element.querySelector()**

- DOM을 찾는데 특히 유용한 querySelector 메서드
- 태그,  #id, .class로 찾을 수 있다.(첫번째)
- querySelectorAll()-> 모든것이 나온다.



## Browser Event, Event object, Event handler

### Event

- HTML 엘리먼트별로 어떤 이벤트가 발생했을 때

- 이벤트 등록: 

```javascript
var el = document.querySelector(".outside");
el.addEventListener("click", function(){
//do something..
}, false);
```

- 이벤트 객체

```javascript
var el = document.getElementById("outside");
el.addEventListener("click", function(evt){
 console.log(evt.target);
 console.log(evt.target.nodeName);
}, false);
```

- event.target
  - 이벤트가 발생한 element	



## Ajax

### Ajax(XMLHTTPRequest 통신)

- 비동기로 서버로부터 데이터를 가져오는 것

- 주로 XML, Plain Text, JSON 등 다양한 포맷의 데이터를 주고 받는다. 일반적으로 JSON사용

```javascript
function ajax(data) {
 var oReq = new XMLHttpRequest();
 oReq.addEventListener("load", function() {
   console.log(this.responseText);
 });    
 oReq.open("GET", "http://www.example.org/getData?data=data");//parameter를 붙여서 보낼수있음. 
 oReq.send();
}
```





























참고 : [부스트코스](https://www.edwith.org/boostcourse-web/lecture/16700/)