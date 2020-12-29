# 웹 프로그래밍 - JavaScript
## 변수
- var, let, const
- 어떤 것을 사용하는가에 의해서 scope, 즉 변수의 유효번위가 달라진다.
- const는 한번 할당한 값에 재할당이 안된다.

## 연산자
``` 
const name = "crong";
const result = name || "default"; //name이 false일때, "default"가 입력 
```
### 비교연산자
- 비교는 ==보다는 ===를 사용
	- ===는 정확한 타입까지 비교->더 정확

## 문자열 처리
- JavaScript에는 문자가 따로 없이 모두 문자열이다.
	- 'a' 역시 String이다.
- 문자열에 다양한 메서드가 있다.
```
"ab:cd".split(":"); //["ab","cd"]
"ab:cd".replace(":", "$"); //"ab$cd"
" abcde  ".trim();  //"abcde" 
```

## 함수
### 선언
- 여러개의 인자를 받아서 그 결과를 출력한다.
- 매개변수와 인자 값의 개수가 일치하지 않아도 실행이 된다.
``` 
function printName(firstname) {
    return myname + " " +  firstname;
} 
```

### 표현식
- 변수값에 함수 표현식을 담아둔 것
- 함수 실행 전에 호출할 경우, 에러가 발생한다.
	- 자바스크립트는 함수 실행 전에 파서가 함수를 한번 훑는데, 이때 변수/함수를 기억해 놓는다. 함수 표현식은 변수로 지정해놓는 것이기 때문에 에러가 발생한다.
``` 
var inner = function(){
	return "inner value";
}
//function inner(){ return "inner value";} ->이것과 다르다.!
var result = inner();//inner value
```

### 반환값과 undefined
- 반환값이 없을경우 undefined가 반환된다.

### arguments 객체
- 함수가 실행되면 그 안에는 arguments라는 특별한 지역변수가 자동으로 생성된다.
- 선언한 파라미터보다 많은 인자를 보내서 arguments로 활용해 사용할 수 있다. 
- 함수(1,2,3): arguments=>{'0':1, '1':2, '2':3} =>arguments[2]=3

### arrow function
```
function getName(name) {
	return "Kim "+name;
}
//위 함수와 같은 arrow함수
var getName = (name) => "Kim "+name;
```


참고 : [부스트코스](https://www.edwith.org/boostcourse-web/lecture/16695/)