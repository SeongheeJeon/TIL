# Devided Navigator and Use Local Reactive State
[관련 커밋](https://github.com/SeongheeJeon/Study_ReactNativeCLI-and-Express.js-and-MongoDB/commit/422be496fe4176b1aa8461a5ab96dce42a78aade)

## Use jwt(jsonwebtoken) and setContext

- 사용자 로그인시 token 생성, DB와 기기의 AsyncStorage에 저장.
    - userResolver.js
        
        ```graphql
        async loginUser(_, {loginInput: {email, password}}, context) {
          // Check if user exist with the email
          const user = await User.findOne({email});
        
          if (!user) {
            throw new ApolloError('This email does not exist.', 'INVALID EMAIL');
          }
        
          // Check if password equals
          if (user && (await bcrypt.compare(password, user.password))) {
            //Create a NEW token
            const token = jwt.sign(
              {
                user_id: user._id,
                email,
              },
              process.env.JWT_SECRET,
              {
                expiresIn: '2h',
              },
            );
        
            user.token = token;
        
            // Save user in MongoDB
            const res = await user.save();
        
            return {
              id: res.id,
              ...res._doc,
            };
          } else {
            throw new ApolloError('Incorrect password', 'INCORRECT_PASSWORD');
          }
        },
        ```
        
    - SignInScreen.tsx
        
        ```graphql
        await signInMutation({
          variables: {
            loginInput: {
              email,
              password,
            },
          },
          onCompleted: async data => {
            asyncStorage.setItem('token', data.loginUser.token);
        
            isSignInVar(true);
          },
          onError: data => {
            Alert.alert(data.message);
          },
        });
        ```
        
- 사용자 기기에 token값이 있을 경우 req.header.authorization에 저장(SetContext) ([참고](https://www.apollographql.com/docs/react/networking/authentication/#header)1, [참고2](https://medium.com/@tharun267/authentication-and-authorization-using-node-js-717d94ecc84d))
    
    → req는 서버에 요청을 보낼 때 전달되는 데이터.
    
    → ApolloClient의 최상단(?) context에서 token 값을 req에 담아 서버로 전달하고, ApolloServer의 최상단(?) context에서 token 검증을 하기 때문에 서버로 요청을 보낼 때나, resolver 각각에서 사용자 인증을 구현하지 않아도 됨.
    
    - ApolloClient의 setContext는 서버에 요청을 보낼 때마다 실행되는듯.
    
    - App.tsx
        
        ```graphql
        const httpLink = createHttpLink({
          uri: 'http://10.0.2.2:4000/graphql',
        });
        
        const authLink = setContext(async (_, {headers}) => {
          const token = await AsyncStorage.getItem('token');
          return {
            headers: {
              ...headers,
              authorization: token ? token : undefined,
            },
          };
        });
        
        const client = new ApolloClient({
          link: authLink.concat(httpLink),
          cache: new InMemoryCache(),
        });
        ```
        
- ApolloServer에서 req.header.authorization으로 전달받은 token값 검증 후, 유효하다면 user 정보와 loggedIn=true 값을 Server Context에 저장. 그 context는 모든 resolver에서 사용가능.

## getAuthUser resolver

- context의 loggedIn 값이 true이면 user정보 반환함.

## Devide navigator according to authrization

- SignInNav과 SignOutNav으로 구분.
- 구분 기준은 isSignInVar 값의 true/false 에 따라.
- stack.navigator가 분리되어서 각각 navigator간에 화면 이동이 안됨.
    
    → 전역 변수를 사용해 변수값이 변경되면 RootNav가 다시 렌더되도록 함.
    
    - Stack.navigator를 분리하지 않고, stack.screen만 분리할 수 있나?
        - SignInNav에서 stack.navigator를 쓰지않고, <></> 또는 <Stack.Group>로 screen들 render하면 화면이 안뜲.
        - Stack.navigator 아래에 바로 <SignInNav /> 작성하는것도 안됨. Group이나 Screen만 포함할 수 있음.

## makeVar, useReactiveVar

- navigation/index 에서 apollo/client 의 makeVar로 isSignInVar 생성.
- RootNav에서 isSignInVar 값에 따라 screen 나뉨.
- 로그인,로그아웃시 isSignInVar를 true,false로 변경.
- isSignInVar 값이 바뀌면, index가 re-render되는게 아니라, RootNav에서 re-render됨.

### etc.

- ApolloClient의 옵션값으로 uri나 link 중 하나 필요. **둘 다 주어질 경우 link가 우선함.** 그래서 기존의 uri를 link와 묶어서(concat) 전달함.

<br>

# 다른 방법

## navigation을 local reactive state로 할당. 

- @apollo/client의 makeVar로 navigation을 전역변수로 할당.  
  -> navigation 자체가 할당되지 않고, 함수가 할당되는데.. 정확히는 모르겠지만 무튼 안됨.
- navigation을 로컬전역 변수로 두는건 좋지않은 방법인듯. 그렇게해서 nested된 navigator간에 navigate가 가능하다면 useNavigatioin 으로도 가능했을 것 같음.


## navigator 나누지않고, query 결과값에 따라 screen component 다르게 render해줌.  
[관련 커밋](https://github.com/SeongheeJeon/Study_ReactNativeCLI-and-Express.js-and-MongoDB/commit/763f3363985a939fab85290edcd5e49076f52e90)

- RootNav에 모든 Stack.Screen render.
- RootNav에서 getAuthUser query 요청, 결과값 받아옴. 사용자가 유효한 token을 갖고있을 경우(로그인 상태일 경우) User 데이터를 query 결과로 전달받음.
- query 결과값 data.getAuthUser의 유무에 따라 Stack.Navigator의 initialRouteName를 할당.
    
    ```jsx
    <Stack.Navigator
      initialRouteName={data && data.getAuthUser ? 'Home' : 'SignIn'}>
      ...
    ```
    
    → initial ‘Component’ 가 아니라, initial ‘RouteName’이라서 data.getAuthUser값이 할당되어도 re-render안되는듯. 
    
    ⇒ 안됨.
    
- query 결과값 data.getAuthUser 유무에 따라 Main Screen의 component를 할당.
    
    ```jsx
    <Stack.Navigator>
      <Stack.Screen
        name="Main"
        component={data && data.getAuthUser ? HomeScreen : SignInScreen}
        options={() =>
          data && data.getAuthUser
            ? {
                headerTitle: () => <HomeHeader />,
              }
            : {headerShown: false}
        }
      />
    ...
    ```
    
    → 로그인된 상태에서 앱 실행시, SignInScreen이 잠시 떴다가, getAuthUser Query 결과값으로 data.getAuthUser가 할당되면 re-render되어 HomeScreen으로 전환됨.
    
- 로그인, 로그아웃 시 navigation 상태를 초기화해줌. (로그인, 로그아웃 한 뒤 이전 화면으로 되돌아 갈 수 없도록)
    
    ```jsx
    navigation.reset({
      index: 0,
      routes: [{name: 'Home'}],
    });
    ```
    

 

- **이 방법이 더 좋은듯. Stack.Navigator 하나에 통합되어 있으니 Screen Component에서  navigation.navigator로 화면 이동도 가능하고, 전역변수를 사용하지 않아도 되고.**
- **단점 : 로그인 된 상태에서 앱 실행시 처음에 로그인 화면이 떴다가, 홈 화면으로 전환되는 것. (이전 방법에서도 그랬던가..? ) 그 외엔 아직 모르겠다. 이 방법으로 코드 수정함.**
