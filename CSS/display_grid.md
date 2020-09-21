# Grid Layout

- 그리드 기반 레이아웃 시스템을 제공하여, float나 position을 사용하지 않고 요소들을 더 쉽게 디자인 할 수 있다.
- flex는 한 방향 레이아웃, grid는 두 방향(가로-세로) 레이아웃.

<br>

# Grid Elements

- 하나의 부모요소와, 한개 혹은 그 이상의 자식요소로 구성된다.

<br>

# Display Property

## < **부모요소 >**

- `display` - grid 또는inline-grid로 설정.
- `grid-gap` - 요소간의 간격 설정.

    `grid-row-gap` 

    `grid-column-gap` 

- `grid-template-columns`  : column의 갯수와 크기 설정.

    `grid-template-rows` : row의 갯수와 크기 설정.

    ```css
    grid-template-columns: auto auto 10px;
    grid-template-rows: 2fr 1fr;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(3, minmax(100px, auto));
    grid-template-columns: repeat(auto-fill, minmax(20%, auto));
    ```

- `justify-content` : 수평축(row)에서의 정렬

    space-evenly(모든 여백이 동일크기, 요소간 여백 중첩o)

    space-around(요소간 여백 중첩x)

    space-between(요소 사이의 여백 모두 동일크기

    center, start, end

- `align-content` : 수직축(column)에서의 정렬

<br>

## < **자식요소 >**

- `grid-column-start`   : 요소의 시작과 끝 지점 설정.

    `grid-column-end` 

    `grid-row-start` 

    `grid-row-end`


- `grid-column`: grid-column-start / grid-column-end

    ex) grid-column: 1 / 5; → line1에서 시작, line5에서 끝

    grid-column: 1 / span 3; → line1에서 시작, 3열에 걸침.

- `grid-area` : grid-row-start / grid-column-start / grid-row-end / grid-column-end

    ex) grid-area: 2 / 1 / span 2 / span 3;

    → 요소의 이름을 설정해주고, 부모요소의 grid-template-areas에서 사용할수도 있음.

    ```css
    .item1 {
      grid-area: myArea;
    }
    .grid-container {
      grid-template-areas: 'myArea myArea . .' 'myArea myArea . .';
    }
    ```

### 참고

[w3schools](https://www.w3schools.com/css/css_grid.asp)

[1분코딩](https://studiomeal.com/archives/533)

[HEROPY Tech](https://heropy.blog/2019/08/17/css-grid/)
