# Display

> `display` 속성은 플로우 레이아웃에 요소가 참여하는 방식과,  
> 자식 요소를 배치할 때 사용할 레이아웃을 설정한다.

# 특징 & 분류

- 상속 안됨.
- `display` 속성의 키워드 값은 6개의 카테고리로 분류할 수 있다.
    - Outside : [block](#block), [inline](#inline), run-in
    - Inside : flow, flow-root, table, flex, grid, ruby
    - List Item : list-item
    - Internal : table-#, ruby-#
    - box : contents, none
    - legacy : [inline-block](#inline-block)(=inline flow-root), inline-table(=inline table), inline-flex(=inline flex), inline-grid(=inline grid)

## none

- 요소를 렌더링하지 않도록 설정한다. `visibility :hidden`으로 설정한 것과 달리, 영역도 차지하지 않는다.

## inline

- ex : `<span>`,  `<a>`, `<em>`, `<b>`
- content의 너비만큼 영역을 차지한다.
- width, height, margin-top, margin-bottom의 속성값이 적용되지 않는다.

    → 위아래 여백은 line-height로 설정 가능하다.

- padding

    → 좌우 padding은 적용되고, 위아래 padding은 시각적으로 적용되긴 하지만 해당 요소의 영역으로 인정되지 않는다.

- inline 요소 뒤에 공백이 있는 경우 정의하지 않은 space(4px)가 자동 지정된다.

    → 공백 없애기 위해서는 [여러 방법](https://css-tricks.com/fighting-the-space-between-inline-block-elements/ "참고 페이지")이 있는데,
    그 중 하나로 상위 요소의 font-size를 0으로 해주고,  
     &nbsp;&nbsp;&nbsp; 해당 inline요소의 font-size를 다시 설정해주면 된다.

## block

- ex : `<div>`, `<p>`, `<h#>`
- width, height, margin, padding 모두 설정 가능.
- 한 줄의 영역을 차지하는 박스형태.
- 기본적으로 width:100%

## inline-block

- ex : `<button>`, `<select>`
- inline처럼 배치되고 block처럼 크기, 여백을 줄 수 있다.
- inline 처럼 좌우 공백이 생긴다. → 처리 방법 동일.

### (추후에 내용 추가할 예정.)
