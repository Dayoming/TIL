# DOM?
[Wikipedia - Document Object Model](https://en.wikipedia.org/wiki/Document_Object_Model)

![](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/DOM-model.svg/1024px-DOM-model.svg.png)

# 다양한 APIs
[`document.`로 사용할 수 있는 APIs](https://www.w3schools.com/jsref/dom_obj_document.asp)
**루트 노트부터 찾을 수 있는 APIs**
* `document.getElementById()`
* `document.getElementByClassName()`
* `document.getElementByName()

**Create**
* `document.createAttribute()`
* `document.createComment()`
* `document.createDocumentFragment()`
* `document.createElement()`
* `document.createTextNode()`

[`element.`로 사용할 수 있는 APIs](https://www.w3schools.com/jsref/dom_obj_all.asp)
- `element.childNodes` - 자식 노드들을 리스트 형태로 반환
- `element.ClassList`, `element.ClassNode`
- `element.firstElementChild`
- `element.insertBefore()`
## DOM 탐색 APIs
### 몇 가지 유용한 속성
- `tagName`
	- element 객체의 tag 이름을 알려준다.
- `textContent`
	- element 객체 안에 있는 text 값을 알려준다.
- `nodeType`
### 이동
- `childNodes`
- `firstChild`
- `firstElementChild`
- `parentElement`
- `nextSibling`
## DOM 조작 APIs
삭제, 추가, 이동, 교체를 위해서는 아래 API 사용법을 잘 익혀두면 좋다.
- `removeChild()`
- `appendChild()`
```javascript
var div = document.createElement("div");
var str = document.createTextNode("오늘 하루는 정말 좋아.");
div.appendChild(str);
// div element 안에 textNode가 들어간 것과 같음.
```
- `insertBefore()`- 특정 영역에 해당하는 위치에 삽입
```javascript
// 테이블 사이에 요소 삽입하기
var base = document.querySelector(".w3-table-all tr:nth-child(3)");
var parent = document.querySelector(".w3-table-all tbody");
var div = document.createElement("div");
var str = document.createTextNode("여기에 추가됐어요.");
div.appendChild(str);

parent.insertBefore(div, base);
```
- `cloneNode()`
- `replaceChlid()`
- `closest()` - 상위로 올라가면서 근접 엘리먼트 찾기
## HTML을 문자열로 처리해주는 DOM APIs
- `innerText`
- `innerHTML` -  파싱 작업이 최적화 되어 있어서 간편하고 빠르다.
```javascript
var html = parent.innerHTML;
parent.innerHTML = "<p>child...</p>";
```
- `insertAdjacentHTML` - 특정 위치를 바꾼다.
```javascript
var base = document.querySelector("div");
base.insertAdjacentHTML("afterbegin", "<h1>Hello world</h1>"); // 바로 뒤에 넣기

base.insertAdjacentHTML("beforebegin", "<p>나는 가운데 끼었어요.</p>"); // 바로 앞에 넣기
