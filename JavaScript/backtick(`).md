- 템플릿 리터럴(내장된 표현식을 허용하는 문자열 리터럴)에 활용되고,

    마크다운에서는 코드를 강조하는데 사용된다.

<br>

- 장점

    - 줄바꿈을 쉽게 할 수 있다.

    - 문자열 내부에 표현식을 포함할 수 있다. 

<br>

- 문자열 인터폴레이션(String Interpolation)

    - 표현식이 문자열로 타입 변환된다 ( `${ ... }` 으로 표현식을 감싼다.)  

      ```jsx
      const first = 'Ung-mo';
      const last = 'Lee';

      console.log(`My name is ${first} ${last}.`);
      ```
<br>

- 템플릿 중첩

    ```jsx
    const classes = `header ${ isLargeScreen() ? '' :
     `icon-${item.isCollapsed ? 'expander' : 'collapser'}` }`;
    ```

<br>

- 헷갈렸던 부분. 

  ```jsx
  clock.innerHTML = `${hour<10 ? `0${hour}` : hour}`
  ```

    :  ``0${hour}`` 이 부분의 결과가 문자열 인데, `${ ... }` 안에 사용될 수 있나? 의문이었다.

     ⇒ 문자열이 반환되고 나서  `hour<10 ? '03' : hour` 이 표현식이 연산되는거라서 상관x

     ⇒ 그리고 `${ ... }` 안에 'aa'이런식의 문자열이나 숫자는 사용 가능함. 

     ⇒ 그냥 aa 라고 적을 경우 변수로 인식하는데, 선언이 안되있을 경우 오류가 생기는 것.
