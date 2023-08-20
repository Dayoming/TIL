# JSP Scope
## Page Scope
* Page Context 추상 클래스를 사용
* JSP 페이지에서 `pageContext`라는 내장 객체로 사용 가능
  * `pageContext.setAttribute, pageContext.getAttribute`
* `forward`가 될 경우 해당 Page scope에 지정된 변수는 사용할 수 없음
* 사용 방법은 Application scope나 Session scope, request scope와 같음
* 마치 **지역 변수**처럼 사용된다는 것이 다른 scope와 다름
* jsp에서 pageScope에 값을 저장 한 후 해당 값을 EL 표기법 등에서 사용할 때 사용됨
* 지역 변수처럼 해당 jsp나 서블릿이 실행되는 동안에만 정보를 유지하고자 할 때 사용됨

## Request Scope
* HTTP 요청을 WAS가 받아서 웹 브라우저에게 응답할 때까지 변수값을 유지하고자 할 경우 사용
* `HttpServletRequest` 객체를 사용
* JSP에서는 `request` 내장 변수 사용
* 서블릿에서는 `HttpServletRequest` 객체 사용
* 값 저장은 `request` 객체의 `setAttribute()`, 값 읽기는 `getAttribute()` 사용
* `forward`시 값을 유지하고자 할 때 사용

## Session Scope
* **웹 브라우저(클라이언트) 별로 변수를 관리**하고자 할 경우 사용
* 웹 브라우저간의 탭 간에는 세션정보가 공유되기 때문에, 각각의 탭에서는 같은 세션 정보 사용 가능
* `HttpSession` 인터페이스를 구현한 객체를 사용
* JSP에서는 `session` 내장 변수를 사용
* 서블릿에서는 `HttpServletRequest`의 `getSession()` 메소드를 이용해 `session` 객체를 얻음
* 값을 저장할 때는 `session` 객체의 `setAttribute()` 메소드 사용
* 값을 읽을 때는 `session` 객체의 `getAttribute()` 메소드 사용
* 장바구니처럼 사용자 별로 유지되어야 할 정보가 있을 때 사용

## Application Scope
* **웹 어플리케이션이 시작되고 종료**될 때까지 변수를 사용할 수 있음
* `SetvletContext` 인터페이스를 구현한 객체를 사용
* jsp에서는 `application` 내장 객체를 이용
* 서블릿의 경우는 `getServletContext()` 메소드 이용
* 웹 어플리케이션 하나당 하나의 `application` 객체가 사용됨
* 값을 저장할 때는 `application` 객체의 `setAttribute()`, 읽을 때는 `getAttribute()`
* 모든 클라이언트가 공통으로 사용해야할 값들이 있을 때 사용