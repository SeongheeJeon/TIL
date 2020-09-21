## < 가상요소 >

- html상에서는 존재하지 않는 가상의 요소를 css로 만들수 있다.

    `::before` ⇒ 요소의 시작부분에 콘텐츠 추가.

    `::after` ⇒ 요소의 끝부분에 콘텐츠 추가.

- 요소의 특정 부분을 class로 지정하지 않고 css에서 선택하여 디자인 할 수 있다.

    `ul li::first-child` ⇒ li요소 중 첫 번째 요소 선택.
    	(li 요소의 첫 번째 자식이라고 헷갈릴 수 있으니 주의.)
    `ul li::first-letter` ⇒ li요소의 첫글자 선택.

    `::selection` ⇒ 요소의 텍스트에서 사용자에의해 선택(드래그)된 영역의 속성을 변경.

    `::placeholder` ⇒ input 필드의 힌트 텍스트에 스타일 적용.

### ::before , ::after

- content속성 필수.

    → normal, string, image, counter, none, attr

    → 아무값도 없이, 디자인만 할경우 `content: "";` 라고 하면됨.

<br>

## < 가상클래스 >

- 요소의 특정 이벤트마다 적용할 스타일을 설정할 수 있다.

    `:link` - 방문한 적이 없는 링크

    `:visited` - 방문한 적이 있는 링크

    `:hover` - 마우스를 롤오버 했을 때

    `:active` - 마우스가 클릭된 상태일 때

    `:focus` - 포커스 되었을 때 (input 태그 등)

    `:first` - 첫번째 요소

    `:last` - 마지막 요소

    `:first-child` - 첫번째 자식

    `:last-child` - 마지막 자식

    `:nth-child(2n+1)` - 홀수 번째 자식

    `:checked` - 라디오 버튼이나 체크박스가 체크되었을 때 적용.

    `:not(태그)` - not안에 포함된 태그를 제외한 모든 부분에 스타일 적용.

<br>

### 참고

[http://blog.hivelab.co.kr/공유before와after-그들의-정체는/](http://blog.hivelab.co.kr/%EA%B3%B5%EC%9C%A0before%EC%99%80after-%EA%B7%B8%EB%93%A4%EC%9D%98-%EC%A0%95%EC%B2%B4%EB%8A%94/)
