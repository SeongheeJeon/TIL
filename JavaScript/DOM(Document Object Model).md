# DOM(Document Object Model)
- html 파일에 존재하는 요소들을 브라우저가 이해할 수 있도록 트리형태로 구조화하여 메모리에 적재한 것.
- 정적인 웹페이지에 접근하여 동적으로 웹페이지를 변경하기 위한 유일한 방법은 메모리 상에 존재하는 DOM을 변경하는 것이고, 이때 필요한 것이 DOM에 접근하고 변경하는 프로퍼티와 메소드의 집합인 DOM API이다.
 
## 노드의 종류

- **문서 노드**

    트리의 최상위에 존재하며 각각 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 한다. 즉, DOM tree에 접근하기 위한 시작점

- **요소 노드**

    HTML 요소를 표현한다. 문서의 구조를 서술한다고 말 할 수 있다.

- **어트리뷰트 노드**

    해당 어트리뷰트가 지정된 요소의 자식이 아니라 해당 요소의 일부로 표현된다.

- **텍스트 노드**

    요소 노드의 자식이며, 자식 노드를 가질 수 없다.

> DOM 트리를 크롬 브라우저에서 확인하려면 
개발자도구 > Elements > properties

## DOM Query

- document.getElementBy*()  : 대상들을 배열로 반환. 하나일 경우에도 배열.

- document.getElementById(id)

    id 어트리뷰트 값으로 요소 노드를 한 개 선택한다.

    복수개가 선택된 경우, 첫번째 요소만 반환한다.

- document.querySelector(cssSelector)

    CSS 셀렉터를 사용하여 요소 노드를 한 개 선택한다. 

    복수개가 선택된 경우, 첫번째 요소만 반환한다.


## HTML 콘텐츠 조작

- 마크업이 포함된 콘텐츠를 추가하는 행위는 크로스 스크립팅 공격(XSS: Cross-Site Scripting Attacks)에 취약하므로 주의가 필요하다.

- textContent 

   - 요소의 **텍스트 콘텐츠**를 취득 또는 변경하며, 이때 마크업은 무시된다.
   
- innerText 

   - 요소의 **텍스트 콘텐츠**에 접근할 수 있다. 하지만 CSS에 순종적이다. 
   
   - 예를 들어 CSS에 의해 비표시(visibility: hidden;)로 지정되어 있다면 텍스트가 반환되지 않는다.
   
- innerHTML 

   - 해당 요소의 모든 자식 요소를 포함하는 **모든 콘텐츠**를 하나의 **문자열**로 취득할 수 있다. 
   
   - 이 문자열은 마크업을 포함한다

## innerHTML vs. DOM 조작 방식 vs. insertAdjacentHTML()

- innerHTML

    - DOM 조작 방식에 비해 빠르고 간편하다   

    - XSS공격에 취약점이 있기 때문에 사용자로 부터 입력받은 콘텐츠(untrusted data: 댓글, 사용자 이름 등)를 추가할 때 주의하여야 한다.

    - 간편하게 문자열로 정의한 여러 요소를 DOM에 추가할 수 있다.

- DOM 조작 방식

    - 특정 노드 한 개(노드, 텍스트, 데이터 등)를 DOM에 추가할 때 적합하다.

    - innerHTML보다 느리고 더 많은 코드가 필요하다.

- insertAdjacentHTML()

    - 간편하게 문자열로 정의된 여러 요소를 DOM에 추가할 수 있다.

    - XSS공격에 취약점이 있기 때문에 사용자로 부터 입력받은 콘텐츠(untrusted data: 댓글, 사용자 이름 등)를 추가할 때 주의하여야 한다.

    - 삽입되는 위치를 선정할 수 있다.

### 참고

[https://poiemaweb.com/js-dom](https://poiemaweb.com/js-dom)