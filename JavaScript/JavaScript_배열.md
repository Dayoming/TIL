[MDN - Javascript Array Object](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array)

[W3C - Javascript Array Reference](https://www.w3schools.com/jsref/jsref_obj_array.asp)
# 배열의 선언
```javascript
var a = [];
var a = [1, 2, 3, "hello", null, true, []];
```
`new Array()`문으로 선언할 수 도 있지만 보통은 간단히 `[]`를 사용한다.
배열에는 `length` 속성이 있어 길이를 쉽게 알 수 있다.
배열 안에는 모든 타입이 다 들어갈 수 있다. **함수, 배열 안에 배열, 배열 안에 오브젝트도 가능**하다.
```javascript
var a = [4];
a[1000] = 3;
console.log(a.length); // 결과: 1001
console.log(a[500]); // 결과: undefined
```
배열에 원소 추가는 **index 번호**와 함께 추가할 수 있다.
추가할 index 번호와 마지막에 값이 존재하는 index 번호 사이에는 `undefined` 값이 들어가게 된다.
```javascript
var a = [4];
a.push(5);
console.log(a.length); // 결과: 2
```
따라서 `push` 메서드를 통해 뒤에 순차적으로 추가하는 것이 일반적이다.
# 배열의 유용한 메서드들
배열은 순서가 있고, `push`와 같은 메서드를 제공하고 있어 추가/삭제/변경이 용이하다.
그 밖에도 많은 메서드들이 존재한다.
```javascript
// 배열의 원소에 특정 값이 들어 있는지 확인할 수 있다.
// 존재하지 않으면 -1을 반환한다.
[1, 2, 3, 4].indexOf(3) // 결과: 2

// 배열의 문자열로 합칠 수 있다.
[1, 2, 3, 4].join(); // "1, 2, 3, 4"

// 배열을 합칠 수 있다.
// 기존 배열은 바뀌지 않는다.
[1, 2, 3, 4].concat(2, 3); // [1, 2, 3, 4, 2, 3]

// 스프레드 오퍼레이터
var origin = [1, 2, 3, 4];
var result = [...origin, 2, 3];
console.log(origin, result); // 결과: [1, 2, 3, 4, 2, 3]
```
배열의 여러가지 메서드도 모두 함수이므로 반환값이 존재한다.
**주의할 점은 어떤 메서드는 새로운 배열을 반환하고, 어떤 메서드는 원래 배열의 값을 변경시킨다는 것**이다.
예를들어 `concat`은 원래 배열은 그대로 있고 합쳐진 새로운 배열을 반환한다.
```javascript
var origin = [1, 2, 3, 4];
var changed = origin.concat(); // [1, 2, 3, 4]
console.log(origin === changed); // [1, 2, 3, 4] [1, 2, 3, 4]
```

# 배열 탐색
## ForEach
> **함수를 동작하는 함수**
```javascript
var origin = [1, 2, 3, 4];
var changedArray = [...origin, 2, 3];

// value, index, object
var fun = function(v, i, o) {
	console.log(v);
}

changedArray.forEach(fun);

// 이렇게 사용할 수도 있
changedArray.forEach(function(v, i, o) {
	console.log(v);
})

```
`forEach`의 매개 변수를 function으로 받아서 그 함수를 실행해주고 있다.
## map
```javascript
var newArr = ["apple", "tomato"].map(function(value, index) {
	return index + "번째 과일은 " + value + "입니다";
});
console.log(newArr);
```
`map`은 **반환되는 정보를 모아서 새로운 배열을 반환**한다.
```javascript
// changedArray의 원소를 돌면서 값을 변경시킨 후에 새로운 배열로 만들어서 반환
var mapped = changeArray.map(function(v) {
	return v * 2;
});

console.log(mapped); // [2, 4, 6, 8, 4, 6]
```

## filter