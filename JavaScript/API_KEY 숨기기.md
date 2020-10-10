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

4. .gitignore 파일을 생성하고, secret.js를 입력한다.<br><br>

### 간단한 방법

- 다른 js 파일에서 선언한 변수를 따로 import 해주지 않아도 사용가능하다.  
    -> 즉, secret.js 파일에 API_KEY를 상수로 선언하면 weather.js 에서 사용가능하다.  
    -> github에 공유되지 않도록 숨기려면 .gitignore에 secret.js를 입력해주면 된다.     
        -> API_KEY가 secret.js에 선언되어있지 않은 상태에서 구문에 사용할 경우 오류가 생기기 때문에,   
        선언되지 않은 경우와 선언된 경우를 나눠서 코드를 작성해야 한다.
