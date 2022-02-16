### Error Message

1. "DataStore - User is unauthorized to query syncChatRooms with auth mode API_KEY. No data could be returned."  
2. “ ... Request failed with status code 401 ... “
    
      ![image](https://user-images.githubusercontent.com/49744535/154335729-22cb21af-2701-4f8c-b938-dba347fec670.png)
### 상황

- DB(dynamoDB)에 데이터 업데이트시 에러. (App.js에서 updateLastOnline 실행)

### 과정

1. 에러메세지를 기반으로 검색하던 중 "graphql"이 계속 보였고, 이 프로젝트에선 graphql을 사용한적이 없는데 이상하다.. 싶어서 찾아보니
  package.json.lock에 "@aws-amplify/api-graphql"가 있었음. (3개월 전, 프로젝트 시작할 때 작성된 것. 내가 몰랐을뿐..)
2. AWS console에서 API KEY가 2월 5일에 만료되었음을 확인함.
3. API KEY 만료일 연장하니 해결됨. 

### 회고

- 에러 메세지에 답이 있었다. “API_KEY”
