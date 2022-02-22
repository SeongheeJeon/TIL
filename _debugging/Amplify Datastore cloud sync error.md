### 상황

- 로그인 직후 HomeHeader에 사용자 이미지 안보임.
- 로그인 이후에 앱을 다시 실행하면 나타남. 


### 분석1

1. HomeHeader의 uesEffect에서 Auth.currentAuthenticatedUser()는 제대로 실행되는데, DB에서 user 받아오는게 안됨. dbUesr가 undefined라고 뜲.
2. 로그인한 상태에서 앱 재실행하면 제대로 작동함. 
3. 아니네? 어떨땐 또 로그인한 상태에서 앱 재실행해도 제대로 안뜨네. network connection 문제인가?
4. network connection이 계속 false라서 이래저래, 컴터를 껐다켜봐도 그대로.
5. 폰 와이파이 껐다켜니까 true, false 왔다갔다 하다가 결국 false인데, 그래도 db는 불러와졌음...

### 분석2

1. 클라우드와 동기화 되기전에  쿼리 보내서 데이터를 못가져오는건가? → 맞았음!!
2. 로컬과 클라우드간의 동기화는 왜 필요한거지? 로컬 없이, 바로 클라우드로 보내면?  
    → 매번 클라우드에 요청을 보내고 데이터를 받기엔 시간 소요가 커서, 앱을 처음 실행할 때 클라우드에서 데이터를 모두 받아와 로컬과 동기화시키고, 그 로컬 저장소를 이용하는 듯.

### 과정

- HomeHeader에 datastore listener 추가함.
    
    ```jsx
    // fetch user when 'syncQueriesReady'
    useEffect(() => {
    	const listener = Hub.listen("datastore", async (hubData) => {
        const { event } = hubData.payload;
        if (event === "syncQueriesReady") {
          fetchUser();
        }
      });
      return () => listener();
    }, []);
    ```
    
    → syncQueriesReady : 로컬이 클라우드와 동기화 되면 전달됨
    
    → useEffect()에서 return되는 함수는 컴포넌트가 unmount될 때 실행됨.
    

### 결론

1. ~~network connection이 true 일 때 다시 테스트해보기.~~
2. 로컬 저장소와 Amplify Datastore 클라우드의 동기화 문제였다.
3. App Component실행시 동기화가 진행되고, 완료된 다음 User 받아옴.
