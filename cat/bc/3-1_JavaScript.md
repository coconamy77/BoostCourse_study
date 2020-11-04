# 웹 앱 개발 - JavaScript

## 자바스크립트 배열

##### 배열의 선언

```javascript
var a = [];
//배열안에는 모든 타입이 다 들어갈 수 있다.
var a = [1,2,3,"hello",null,true,[]];
a.push(5);
a[10000] = 3;//length가 길어진다.
```

##### 배열의 메서드

```javascript
[1,2,3,4].indexOf(3)//2, 존재 하지 않는 값이면 -1반환
[1,2,3,4].join(); //배열로 합쳐진다 -> "1,2,3,4"

[1,2,3,4].concat(2,3); //[1,2,3,4,2,3]->원래 배열은 그대로 있고 새로운 배열이 리턴
var a = [1,2,3,4];
var changed = a.concat();//==[...a,2,3]으로도 사용 가능
console.log(a === changed);//false
```

##### 배열 탐색

```javascript
//forEach
["apple","tomato"].forEach(function(value){
    console.log(value);
})
```

```javascript
var a = [1,2,3,4];
var fun = function(v,i,o){
    console.log(v);
};

a.forEach(fun);
```

```javascript
var a = [1,2,3,4];
var mapped = newarr.map(function(v){
    return v*2;
});
console.log(mapped); //[2,4,6,8] 새로운 배열 반환
```



## 자바스크립트 객체

- key, value구조의 자료구조

##### 객체 선언, 추가, 삭제

```javascript
var member = {name:"summer", age: 20};

member["gender"] = "F";
member.country = "Korea";

delete member.country;
delete member["gender"];
```

##### 객체의 탐색

```javascript
var obj = {"name":"codesquad" , age :22, data: [1,2,3,4,5]};

for(value in obj) {
  console.log(obj[value]);
}

var obj = { "name" : "codesquad" , age : 22, data : [1,2,3,4,5] };

Object.keys(obj).forEach(function(key) {
	console.log(obj[key]);
});
```











참고: [부스트코스](https://www.edwith.org/boostcourse-web/lecture/16746/)