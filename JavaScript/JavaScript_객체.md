> 자바스크립트 객체는 `key`와 `value`로 이루어진 `dictionary` 자료구조

[MDN - Working With Objects](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Working_with_Objects#%EA%B0%9D%EC%B2%B4_%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0)
# 객체
* key, value 구조의 자료구조
* Javascript로 데이터를 표현하기 위해서는 Array, Object를 사용
* Object 형태는 {} 로 그 자료를 표현하며, 서버와 클라이언트 간에 데이터를 교환할 때 Object 포맷과 비슷한 방법으로 데이터를 보냄.
```javascript
var obj = {name : "crong", age : 20}
```
자바스크립트 객체구조를 본따 서버와 웹 브라우저간에 데이터를 주고 받을때 정의한 [[JSON]](JavaScript Object Notation)이라는 것이 있다.

```javascript
// object 안에 object, array가 들어가는 것도 가능하다.
var myFriend = {key : "value", addition: [{name: 'boostcourse'}, {age: 2}]};
// .key를 이용해 객체를 불러올 수도 있다.
console.log(myFriend.key);
myFriend["address"] = "seoul";

// 객체 안의 객체도 탐색할 수 있다.
console.log(myFriend.addition[0].name); // boostcourse
```
## 객체의 탐색
### for-in
`for-in`은 object를 탐색하기 위한 목적으로 자주 사용한다.
```javascript
var myFriend = {key : "value", addition: [{name: 'boostcourse'}, {age: 2}]};

for (key in myFriend) {
	console.log(value) // key, addition
	console.log(myFriend[key]) // value, [{name: 'boostcourse'}, {age: 2}]
}
```
### Object.keys()
`forEach`와 함께 사용할 수 있다.
```javascript
var myFriend = {key : "value", addition: [{name: 'boostcourse'}, {age: 2}]};

// 키 값을 배열로 출력
console.log(Object.keys(myFriend)); // ['key', 'addition']

Object.keys(myFriend).forEach(function(v) {
	console.log(myFriend[v]);
}); // value, [{name:'boostcourse'}, {age: 2}]
```
## 실습1
**[데이터](https://gist.github.com/crongro/ade2c3f74417fc202c8097214c965f27)에서 숫자 타입으로만 구성된 요소를 뽑아 배열을 만들어보세요.**
```javascript
result = []

for (key1 in data) {
    if (typeof data[key1] === "object") {
        for (key2 in data[key1]) {
            if (typeof data[key1][key2] === "number") {
                result.push(data[key1][key2]);
            }
        }
    } else if (typeof data[key1] === "number") {
        result.push(data[key1])
    }
}

console.log(result)
```
## 실습2
**[데이터](https://gist.github.com/crongro/a9a118977f82780441db664d6785efe3)에서 `type`이 `sk`인, `name`으로 구성된 배열만 출력해보세요.**
```javascript
names = []

function funcFindSk(data) {
    Object.keys(data).forEach(function(key) {
        if (data[key].type == "sk") {
            names.push(data[key].name);
        }

        if (typeof data[key].childnode === "object") {
            funcFindSk(data[key].childnode);
        }
    });
}

funcFindSk(data);

console.log(names);
```