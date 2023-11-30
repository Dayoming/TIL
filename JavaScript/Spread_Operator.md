## ES6 Spread Operator
> [!스프레드 연산자, 전개 구문, 펼침 연산자... ES6에서 추가된 문법]
> 
> [참고자료](https://paperblock.tistory.com/62)
### Spread Operator 기본 문법
배열, 문자열, 객체 등 반복 가능한 객체(Iterable Object)를 **개별 요소로 분리**할 수 있다.
```javascript
// Array
var arr1 = [1, 2, 3, 4, 5];
var arr2 = [...arr1, 6, 7, 8, 9];

console.log(arr2); // [1, 2, 3, 4, 5, 6, 7, 8, 9]

// String
var str1 = 'paper block';
var str2 = [...str1];
console.log(str2); // ["p", "a", "p", "e", "r", " ", "b", "l", "o", "c", "k"]
```
### 배열에서의 Spread Operator
#### 배열 병합 (Array Concatenation)
기존에 두 개의 배열을 결합하는 데 있어서 `concat` 메서드를 이용했는데, ES6에서는 spread 연산자를 이용하여 좀 더 깔끔한 배열 병합이 가능하다.
```javascript
var arr1 = [1, 2, 3];
var arr2 = [4, 5, 6];

var arr = [...arr1, ...arr2];
console.log(arr); // [1, 2, 3, 4, 5, 6]
```
또한 spread 연산자를 이용하면 다양한 형태의 배열 병합을 비교적 간단하게 수행할 수 있다.
```javascript
var arr1 = [1, 2, 3];
var arr2 = [4, 5, 6];
arr1.push(...arr2)

console.log(arr1); // [1, 2, 3, 4, 5, 6]

var arr1 = [1, 2];
var arr2 = [0, ...arr1, 3, 4];

console.log(arr2); // [0, 1, 2, 3, 4]
```
#### 배열 복사 (Copying Arrays)
Javascript에서 **배열을 새로운 변수에 할당하는 경우 새로운 배열은 기존 배열을 참조**한다. 따라서 **새로운 배열을 변경하는 경우 원본 배열 역시 변경**된다.
```javascript
var arr1 = ["철수", "영희"];
var arr2 = arr1;

arr2.push("바둑이");
console.log(arr2); // ["철수", "영희", "바둑이"]
// 원본 배열도 변경된다.
console.log(arr1); // ["철수", "영희", "바둑이"]
```
배열 참조가 아닌 배열 복사를 위해서 기존에는 `slice`또는 ES5의 `map`을 이용하여 배열을 복사했다.
```javascript
// Javascript array 복사
var arr1 = ['철수','영희']; 
var arr2 = arr1.slice();

arr2.push('바둑이'); 
console.log(arr2); // [ "철수", "영희", "바둑이" ]
// 원본 배열은 변경되지 않습니다.
console.log(arr1); // [ "철수", "영희" ]


// ES5 map 
var arr1 = ['철수','영희']; 
var arr2 = arr1.map((item) => item);

arr2.push('바둑이'); 
console.log(arr2); // [ "철수", "영희", "바둑이" ]
// 원본 배열은 변경되지 않습니다.
console.log(arr1); // [ "철수", "영희" ]
```
ES6의 spread 연산자를 사용하면 다음과 같이 새로운 **복사된 배열을 생성**할 수 있다.
```javascript
var arr1 = ["철수", "영희"];
var arr2 = [...arr1];

arr2.push("바둑이");

console.log(arr2); // ["철수", "영희", "바둑이"]
// 원본 배열은 변경되지 않습니다.
console.log(arr1); // ["철수", "영희"]
```
spread 연산자를 이용한 복사는 [[얕은(shallow) 복사]]를 수행하며, **배열 안에 객체가 있는 경우**에는 객체 자체는 복사되지 않고 **원본 값을 참조**한다.
따라서 **원본 배열 내의 객체를 변경하는 경우 새로운 배열 내의 객체 값도 변경**된다.
### 함수에서의 Spread Operator
#### Rest Parameter
함수를 호출할 때 **함수의 매개변수(parameter)를 spread operator로 작성한 형태를 Rest parameter**라고 부른다.
Rest 파라미터를 사용하면 함수의 파라미터로 오는 값들을 모아서 "배열"에 집어넣기 때문에 깔끔한 함수 표현을 적용할 수 있다.
```javascript
function add(...rest) {
	let sum = 0;
	for (let item of rest) {
		sum += item;
	}
	return sum;
}

console.log(add(1)); // 1
console.log(add(1, 2)); // 3
console.log(add(1, 2, 3)); // 6
```
인자의 개수에 상관없이 모든 인자를 더하는 함수를 쉽게 구현할 수 있다.
기존 Javascript에서는 "arguments"를 이용하여 처리할 수 있었다.
다음과 같이 기본 인자를 섞어서 사용하는 방법도 가능하다.
```javascript
function addMul(method, ...rest) {
	if (method === 'add') {
		let sum = 0;
		for (let item of rest) {
			sum += item;
		}
		return sum;
	} else if (method === 'multiply') {
		let mul = 1;
		for (let item of rest) {
			mul *= item;
		}
		return mul;
	}
}

console.log(addMul('add', 1, 2, 3, 4)); // 10
console.log(addMul('multiply', 1,2, 3, 4)); // 24
```
❗**단, Rest 파라미터를 항상 제일 마지막 파라미터로 있어야 한다.**
#### 함수 호출 인자로 사용
위의 경우와 반대로, 함수 정의 쪽이 아니라 **함수를 Call 할 때** spread operator를 사용할 수 있다. 기존에는 배열로 되어있는 내용을 함수의 인자로 넣어주려면 손으로 풀어서 넣어주던지, `Apply`를 이용했어야 하지만 spread operator를 이용하면 **배열 형태에서 바로 함수 인자로 넣어줄 수** 있다.

`Math` 함수의 예를 살펴보자.
```javascript
var numbers = [9, 4, 7, 1];
Math.min(...numbers);
```
`map`과 섞어서 응용할 수도 있다.
```javascript
var input = [{name: '철수', age: 12}, {name: '영희', age: 12}, {name: '바둑이', age: 2}];

// 가장 어린 나이 구하기
var minAge = Math.min(...input.map((item) => item.age));
console.log(minAge); // 2
```
### 객체에서의 Spread Operator
ES2018 (ES9)에서는 객체와 관련된 사항이 추가되었다.
배열에 대한 Spread Operator를 지원하는 최근의 브라우저는 객체에 대한 Spread Operator 역시 지원한다.
#### 객체 복사 또는 업데이트
객체에서 spread operator를 이용하여 객체의 복사 또는 프로퍼티를 업데이트 할 수 있다.
간단한 State Management 구현을 위해서 다음과 같은 방식으로 응용하여 사용하기도 한다.
```javascript
var currentState = {name: '철수', species: 'human'};
currentState = {...currentState, age: 10};

console.log(currentState) // {name: "철수", species: 'human', age: 10}

// 객체의 프로퍼티를 오버라이드 함으로써 객체가 업데이트됨
currentState = { ...currentState, name: '영희', age: 11}; 
console.log(currentState); // {name: "영희", species: "human", age: 11}
```
### Destructuring
배열이나 객체에서의 destructuring에서 사용될 수 있다. 이 경우에 의미적으로는 spread operator라기보다는 rest parameter에 가까운 형태.
```javascript
var a, b, rest;
[a, b] = [10, 20];

console.log(a); // 10
console.log(b); // 20

[a, b, ...rest] = [10, 20, 30, 40, 50];

console.log(rest); // [30,40,50]

({a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40});
console.log(a); // 10
console.log(b); // 20
console.log(rest); // {c: 30, d: 40}
```
