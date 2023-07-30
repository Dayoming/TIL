# 상속과 우선순위
**노드는 상위 노드와 같은 속성값**을 갖게 만들어줄 수 있다. 이를 상속이라고 부르는데, margin, border, padding과 같은 배치와 관련된 속성들은 상속되지 않는다.

CSS는 여러가지 스타일정보를 기반으로 최종적으로 '경쟁'에 의해 적절한 스타일이 반영된다.

## 표기 방법
```css
div ul li {
    font-size: 10px;
}
```
위와 같은 코드인 경우, 'div 요소 하위 ul 아래 li 요소에 font-size를 10px로 설정한다'는 의미이다.

각 Selector별 CSS 상속 표기는 다음과 같다.
```css
/* Type Selector - 공백으로 표기 */
h1 h2 h3 h4 {
    ...
}

/* Class Selector - .class명으로 표기 */
li.spacious.elegant {
    ...
}

/* ID Selector - #id명으로 표기 */
div #myId#identified {
  ...
}
```
## 우선순위
### 순서에 따른 우선순위
* 같은 선택자의 경우 가장 뒤에 나온 스타일을 우선적으로 덮어쓰게 된다.
```css
h1 {
    color: red;
    text-align: left;
}

/* h1이 적용된 텍스트는 가운데 정렬됨 */
h1 {
    text-align: center;
}
```

### 선언방식에 따른 우선순위
**1. inline > internal > external**
* 같은 속성이 있는 경우 inline style이 가장 먼저 적용되고, head 태그 안에 있는 internal style, 마지막으로 external style이 적용된다.

**2. 구체적인 표현**
* 선택자의 표현이 구체적인 것이 있다면 먼저 적용됩니다.
```css
/* red 적용 */
body > span {
    color: red;
}

span {
    color: blue;
}
```

### 명시도에 따른 우선순위
**Inline Style > !important > id > class > tag**
* 우선적으로 적용되는 인라인 스타일과 !important는 모두 나쁜 습관으로 취급되지만, 가끔 !important로 인라인 스타일을 덮어써야 하는 경우도 발생
* 이때, 전역 CSS 파일의 몇몇 스타일을 !important로 설정해서 요소에 직접 작성한 인라인 스타일을 덮어쓸 수 있다.
```html
<div class="foo" style="color: red;">나는 무슨 색일까?</div>
```
```css
.foo[style*="color: red"] {
  color: firebrick !important;
}
```
위와 같이 적용하면, .foo의 color는 firebrick이 된다.

#### 참고자료
* https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity
* [네이버 부스트코스 - 웹 프로그래밍(풀스택)](https://www.boostcourse.org/web316/lecture/254262?isDesc=false)