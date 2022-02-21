# Build an app with AWS Amplify 

1. AWS에서 ‘앱 구축’ → Studio 시작
2. Setup Authentication 
    
    - 로그인 방법, 비밀번호 규칙 등 설정
    
3. Data Modeling
    
    - Datastore : Query library, On-device caching, Data sync
        
4. `amplify pull` 
        
5. install amplify libraries (React Native & Expo )
    
    **`npm install aws-amplify aws-amplify-react-native @react-native-community/netinfo @react-native-async-storage/async-storage @react-native-picker/picker`**
    
6. Configure Amplify (In App.tsx)
    
    ```jsx
    import Amplify from "aws-amplify";
    import config from "./src/aws-exports";
    Amplify.configure(config);
    ```
    
7. Configure Auth (In App.tsx)
    
    ```jsx
    import {withAuthenticator} from "aws-amplify-react-native";
    ...
    export default withAuthenicator(App);
    ```
    
    
# Lambda Function
- 앱에서 특정 이벤트가 발생했을 때 실행되는 함수
- 회원가입(인증)이 됐을 때 dynamoDB에 User Data 생성.
    
    
1. `amplify auth update`
    -> PostConfirmation Lambda trigger 생성 가능
        
    - custom.js 생성됨 & 작성
        
        ```jsx

        const aws = require("aws-sdk");
        const ddb = new aws.DynamoDB();
        
        const tableName = process.env.USERTABLE;
        
        exports.handler = async (event) => {
          // event.request.userAttributes.(sub, email, ...)
          // insert code to be executed by your lambda trigger
        
          if (!event) {
            return;
          }
        
          if (!event.request.userAttributes.sub) {
            console.log("No sub provided");
            return;
          }
        
          const now = new Date();
          const timestamp = now.getTime();
        
          const userItem = {
            __typename: { S: "User" },
            _lastChangedAt: { N: timestamp.toString() },
            _version: { N: "1" },
            createdAt: { S: now.toISOString() },
            updatedAt: { S: now.toISOString() },
            id: { S: event.request.userAttributes.sub },
            name: { S: event.request.userAttributes.email },
          };
        
          const params = {
            Item: userItem,
            TableName: tableName,
          };
        
          // save a new user to DynamoDB
          try {
            await ddb.putItem(params).promise();
            console.log("success");
          } catch (e) {
            console.log(e);
          }
        };
        ```
        
    
    `amplify push`
    
9. Add Environment Variables "USERTABLE" to Lambda Function
    - AWS Lambda → 구성 → 환경변수 → USERTABLE : “usertablename”


10. Add Permission to Lambda Function at IAM
    - AWS Lambda → 구성 → 권한 → IAM으로 이동 → 청책연결→ 정책생성 
        
        ```jsx
        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Action": ["dynamodb:PutItem"],
                    "Resource": [<usertableARN>]
                }
            ]
        }
        ```
        
    - 정책 연결
    - `amplify pull`


### 참고 
[강의](https://www.youtube.com/watch?v=T0D4GF5YryI) 
