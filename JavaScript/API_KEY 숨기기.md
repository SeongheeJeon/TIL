### .gitignore, import, export 이용하여 API_KEY 숨기기(JavaScript)

1. secret.js 파일을 생성하고, API_KEY를 상수로 선언 후 export 해준다.

    ```javascript
    export const API_KEY = "값";
    ```  
<br>

2. API_KEY키를 사용할 파일("weather.js")의 상단에서 값을 받아온다.

    ( 반드시 중괄호 사용. 파일경로 "secret.js"로 할 경우 오류발생.  )

    ```jsx
    import {API_KEY} from "./secret.js";
    ```
<br>

3. html에 import를 입력한 js파일을 모듈로 명시해줘야한다.

    ```jsx
    <script type="module" src="weather.js"></script>
    ```
<br>

4. .gitignore 파일을 생성하고, secret.js를 입력한다.
