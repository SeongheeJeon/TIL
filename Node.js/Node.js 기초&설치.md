> # Node.js란? ( 공식 사이트 정의 )

Node.js 는 Chrome V8 JavaScript 엔진으로 빌드된 JavaScript 런타임입니다.  
Node.js는 이벤트 기반, Non 블로킹 I/O 모델을 사용해 가볍고 효율적입니다.  
Node.js의 패키지 생태계인 npm은 세계에서 가장 큰 오픈 소스 라이브러리 생태계이기도 합니다.

<br>

> # Node.js란? ( 그 외 )
- 네트워크 애플리케이션(특히 서버사이드) 개발에 사용되는 소프트웨어 플랫폼.  
  →  node.js를 이용하여 서버를 만들 수 있다.
  
- javascript언어 사용.  
→ javascript언어로 web browser를 제어하거나 서버를 제어(node.js)할 수 있다.  
→ cf) 동일한 언어(한국어)를 사용한다 하더라도, 법원에가서 약달라고 하면 안되는 것처럼, 각 용도에 맞는 함수나 기능을 사용해야 한다.

- javascript언어로 만들어진 프로그램은 웹브라우저 없이 사용할 수 없었다.  
→ node.js는 javascript를 웹브라우저에서 독립시킨것으로, 브라우저 없이 바로 실행할 수 있다.

- node.js가 가장 빛을 발하는 곳은 실시간 웹 애플리케이션이다.  
→ 이유는 [About Node.js node.js](https://nodejs.org/en/about/) 정보에 잘 나와 있다.  
→ node.js는 lock이 없으므로 프로세스를 dead-locking 할 걱정이 없고, I/O를 직접 수행하지 않으므로 프로세스가 절대 차단되지 않기 때문이라고 한다.  
→ non Block이기에 확장 가능한 시스템은 노드에서 개발하는 것이 합리적이다.  
<br>

> # Node.js와 NPM 설치하기  
- [Node.js](https://nodejs.org/en/)에서 다운받으면 node.js와 npm이 같이 설치된다.  
→ 설치가 완료되었다면, cmd(명령 프롬프트)에서 "node -v" 와 "npm -v" 를 입력해 확인해볼 수 있다. 버전 정보가 나온다면 제대로 설치된 것.
<br>

> # Node.js 기본 사용 방법  
1. cmd에 "node"를 입력한다.
2. 이제 cmd에 JavaScript코드를 입력하여 실행할 수 있다.

    ( ex : "console.log("hello");" )

3. ctrl+c를 두 번 누르면 node를 종료할 수 있다.
4. js 파일을 실행시킬 수도 있다. cmd에서 해당 파일이 있는 경로로 이동하여, "node js파일명" 입력.

    ( ex : "node hello.js" ) 

    - ? 파일 경로, 한글인식못함 ?

        → 한글은 인식 가능. 드라이브간 이동을 cd로 하려고 해서 안됐던것.  
        → 경로 변경과 드라이브 변경은 별개임. E:로 드라이브 먼저 변경해주고 cd하거나, "cd /d E:\사용자" 이렇게 한번에도 가능.
<br>

> # NPM 기본 사용방법

< npm을 통해 모듈을 다운받아보자. > 

1. 먼저 새 폴더를 만들어 cmd의 경로를 그 폴더로 변경한다.  
2. "npm init -y" 입력. → package.json 파일이 생성된다.

    → package.json은 프로젝트에 대한 명세라고 할 수 있다. 해당 프로젝트의 이름, 버전, 사용되는 모듈 등의 스펙이 정해져있으며, 이 파일을 통해 의존성 모듈 관리도 진행할 수 있다. 

    → 만약 어떤 오픈 소스를 다운받을 때 이 package.json만 있다면 해당 오픈소스가 의존하고 있는 모듈이 어떤 것인지 알 수 있고, 그 모듈들을 한번에 설치할 수도 있다. ( "npm install" 입력 ) 

3. npm을 통해 모듈을 설치한다.

    → "mocha" 라는 Front-End 단위테스트(TDD) 프레임워크를 설치해보자.  ( "npm install mocha --save-dev" 입력 )   
    → 설치가 완료되면, 작업중인 폴더에 "node_modules" 폴더가 생성되고, 그 안에 mocha 모듈이 설치된 것을 확인할 수 있다.   
    → package.json의 devDependencies에 mocha가 추가된 것도 확인.
    ( 설치 시 --save-dev 옵션을 주었기에 추가가 된 것. --save 옵션과 다른점은, devDependencies에 추가된다는 점과 --production 빌드시 해당 플러그인이 포함되지 않는다는 점. )  
    → 이렇게 설치한 npm 모듈은 해당 프로젝트에서 사용할 수 있는, 흔히 말하는 지역변수와 같은 개념이 되는 것이다.
    해당 프로젝트 외에서도 사용할 수 있도록 전역으로 설치하기 위해서는 설치 시 -g 옵션을 추가해주면 된다.
    ( C:\Users\사용자명\AppData\Roaming\npm 에 설치된다. 폴더가 안보인다면 숨김폴더를 해제해보자. )  
    → 전역으로 설치한 모듈을 해당 프로젝트에서 심볼릭 링크로 사용 가능. ( "npm link mocha" )

> # 참고

[Dev.DY - github.io](https://kdydesign.github.io/2017/07/15/nodejs-npm-tutorial.html#node-js-%E1%84%90%E1%85%A6%E1%84%89%E1%85%B3%E1%84%90%E1%85%B3%E1%84%92%E1%85%A2%E1%84%87%E1%85%A9%E1%84%80%E1%85%B5)
