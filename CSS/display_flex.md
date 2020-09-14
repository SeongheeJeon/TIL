# 용도

- 레이아웃을 매우 효과적으로 표현할 수 있다.
- flex가 없었을 땐 table, position, float을 사용해왔다.

    → table은 구조화된 정보로서의 의미를 가지고 있는데, 그것을 레이아웃으로 사용하게 되면  
    &nbsp;&nbsp;&nbsp;검색엔진이나 html을 해석하는 단계에서 문제가 생길수있다.  
    → float 는 보통 이미지 옆에 글씨가 흘러가도록 하는 효과 줄 때 사용한다.  
  <br>
  
# 사용법

- container역할을 하는 부모요소에 display 속성을 flex로 설정하면, 그 자식요소에 flex가 적용된다.
- item과 container 에 부여해야하는 속성이 구분되어있다.
<br>

# 관련 속성 (*표시는 기본값.)
## < container에 적용 >

1. **display** 
    - flex로 설정.
    
2. **flex-direction** : 요소의 주 축(main-axis)을 설정
    - row*, row-reverse, column, column-reverse
    
3. **flex-wrap** : items의 줄 바꿈 설정
    - nowrap*, wrap
    
4. **flex-flow** ; flex-directon 와 flex-wrap 의 단축속성
    - flex-flow : *flex-direction  flex-wrap*

5. **jusitfy-content** : 주 축에서의 정렬 설정.
    - flex-start*, flex-end, center
    - space-between : 시작 item은 시작점에, 마지막 item은 끝점에 정렬되고 나머지는 사이에 고르게.
    - space-around : 균등한 여백으로 정렬.

6. **align-items** : 교차 축에서 items의 정렬 방법(1줄)
    - stretch*, flex-start, flex-end, center 등

7. **align-content** : wrap설정되어있고 2줄이상일 경우 교차축에서의 정렬
<br>

## < item >

1. **order** : item의 순서를 설정

2. **flex-basis** : item의 (공간배분 전)기본 크기 설정
    - auto* : width, height로 item의 너비를 설정할 수 있다. 하지만 flex-basis 값이 주어질 경우 width, height는 적용되지 않는다.
    - grow, shrink 적용될 때 기본값이 되기도 함.

3. **flex-grow** : 각 item들이 container 내의 여백을 어떻게 나눠가질지.
(각 item별로 다르게 설정가능)

4. **flex-shrink** : container의 크기가 줄어들 경우 각 item의 여백이 줄어드는 정도.

5. **flex** :  단축속성
    - flex : *flex-grow [flex-shrink] [flex-basis]*
6. **align-self** : 각 item별로 정렬 설정.

# 참고

[flex-생활코딩](https://opentutorials.org/course/2418/13526) 

[HEROPY Tech CSS Flex 완벽 가이드](https://heropy.blog/2018/11/24/css-flexible-box/)
