> FoS Project TIL

- 가져오기 버튼 hover시 툴팁text 보이도록 하려는데 안됨. 요소 자체에 대한 hover 효과는 되는데, 본인이 아닌 다른 요소에 효과를 주는건 안됨.

    → 버튼요소 내부에 툴팁text가 있는게 아니라서, '인접 선택자(+)'가 필요했음.

- 인접선택자 ([출처](https://stackoverflow.com/questions/4502633/how-to-affect-other-elements-when-one-element-is-hovered))
    
    If the cube is directly inside the container:

    ```css
    #container:hover > #cube { background-color: yellow; }
    ```

    If cube is next to (after containers closing tag) the container:

    ```css
    #container:hover + #cube { background-color: yellow; }
    ```

    If the cube is somewhere inside the container:

    ```css
    #container:hover #cube { background-color: yellow; }
    ```

    If the cube is a sibling of the container:

    ```css
    #container:hover ~ #cube { background-color: yellow; }
    ```
