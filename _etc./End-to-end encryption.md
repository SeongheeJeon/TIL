## End-to-end Encryption(종단간 암호화)

- 발신원부터 수신원까지 정보의 암호화를 유지한 채로 전송하는 방식. E2E(End-to-End) 암호화라고도 부른다.

- **Symmetric Encryptioin(대칭키 암호화)** : 데이터를 암호화할 때와 복호화할 때 동일한 secret key를 사용
- **Asymmetric Encryptioin(비대칭키 암호화)** : 암호화시 public key를, 복호화시 secret key를 사용 (또는 암호화시 secret key, 복호화시 public key)
- **secret key** : (현재 프로젝트의 경우) 회원가입시 생성되며 각 사용자의 기기에 저장
- **public key** : (현재 프로젝트의 경우) 회원가입시 생성되며 DB에 저장
- **과정 (예시)** :  A가 B에게 data를 전송하는 경우
    1. A가 B에게 ‘ABC’라는 텍스트를 전송
    2. DB에서 B의 public key를 get (B가 읽어야 하므로)
    3. B의 public key로 암호화 → 암호화된 텍스트를 DB에 저장
    4. B가 DB로부터 암호화된 텍스트 get
    5. B의 기기에 저장된 B의 private key로 복호화 → 화면에 출력
   
- **Case of Group ChatRoom** : 5명이 참여하고있는 대화일 경우, 1:1로 각각 메세지와 key가 저장되어야 한다. 그룹별로 key를 사용한다면 그룹원끼리 key를 공유하게 되는데,
  보안상으로 좋지않다.
    
- 암호화,복호화 이해에 도움이 된 글 : [tistory-sieunlim](https://sieunlim.tistory.com/16) / [KOSMOS-post](https://www.ksakosmos.com/post/r-s-a)

---

### tweetnacl Library

- cryptographic(암호화) library to JavaScript ([github](https://github.com/dchest/tweetnacl-js))
- “no PRNG” ERROR : “랜덤 난수 생성기가 없음”. tweetnacl의 PRNG는 js로 되어있어 브라우저에선 사용 가능하지만,
    현재 플젝은 expo를 활용한 모바일 환경이기 때문에 별도의 PRNG가 필요하다.

- **getRandomBytes with expo-random (PRNG)**
    
    ```jsx
    import { secretbox, randomBytes, setPRNG } from "tweetnacl";
    import { getRandomBytes } from "expo-random";
    
    setPRNG(function (x, n) {
      // ... copy n random bytes into x ...
      const randomBytes = getRandomBytes(n);
      for (let i = 0; i < n; i++) {
        x[i] = randomBytes[i];
      }
    });
    
    console.log(randomBytes(secretbox.nonceLength));
    ```
    
- **tweetnacl-util-js** : encoding, decoding function
    
    → but, `@stablelib/utf8` `@stablelib/base64` 사용하길 권장함. 
    
- **box** : **asymmetric public key encryption**
    - example code
        
        ```jsx
        import { setPRNG, box } from "tweetnacl";
        import { getRandomBytes } from "expo-random";
        import { decode as decodeUTF8, encode as encodeUTF8 } from "@stablelib/utf8";
        import {
          decode as decodeBase64,
          encode as encodeBase64,
        } from "@stablelib/base64";
        
        const PRNG = (x, n) => {
          // ... copy n random bytes into x ...
          const randomBytes = getRandomBytes(n);
          for (let i = 0; i < n; i++) {
            x[i] = randomBytes[i];
          }
        };
        
        setPRNG(PRNG);
        
        const newNonce = () => getRandomBytes(box.nonceLength);
        const generateKeyPair = () => box.keyPair();
        
        const encrypt = (secretOrSharedKey, json, key) => {
          const nonce = newNonce();
          const messageUint8 = encodeUTF8(JSON.stringify(json));
          const encrypted = key
            ? box(messageUint8, nonce, key, secretOrSharedKey)
            : box.after(messageUint8, nonce, secretOrSharedKey);
        
          const fullMessage = new Uint8Array(nonce.length + encrypted.length);
          fullMessage.set(nonce);
          fullMessage.set(encrypted, nonce.length);
        
          const base64FullMessage = encodeBase64(fullMessage);
          return base64FullMessage;
        };
        
        const decrypt = (secretOrSharedKey, messageWithNonce, key) => {
          const messageWithNonceAsUint8Array = decodeBase64(messageWithNonce);
          const nonce = messageWithNonceAsUint8Array.slice(0, box.nonceLength);
          const message = messageWithNonceAsUint8Array.slice(
            box.nonceLength,
            messageWithNonce.length
          );
        
          const decrypted = key
            ? box.open(message, nonce, key, secretOrSharedKey)
            : box.open.after(message, nonce, secretOrSharedKey);
        
          if (!decrypted) {
            throw new Error("Could not decrypt message");
          }
        
          const base64DecryptedMessage = decodeUTF8(decrypted);
          return JSON.parse(base64DecryptedMessage);
        };
        
        const obj = { hello: "world" };
        const pairA = generateKeyPair();
        const pairB = generateKeyPair();
        
        const sharedA = box.before(pairB.publicKey, pairA.secretKey);
        const encrypted = encrypt(sharedA, obj);
        
        const sharedB = box.before(pairA.publicKey, pairB.secretKey);
        const decrypted = decrypt(sharedB, encrypted);
        
        console.log(obj, encrypted, decrypted);
        ```
        
---

### Public Key Setting at DB

- Add ‘publicKey’ to User Model
- Add 1:1 relationship at Message with User.
    - Message에 User와 1:1 관계 추가하고 배포하니 에러뜲. (User와 Message가 서로를 언급할 수 없다.)
        
        ### 분석
        
        이미 User에서 Message와 1:n 관계가 있었고, 거기서 추가로 1:1 관계를 만들 수 없나봄. 하긴 필요없는 중복이긴하겠다. 1:1 관계로 이미 연결되어 있으면 1:n은 굳이.
        
        (근데 왜 슨생님은 배포가 되는것이여?..)
       
        
        ### 해결
        
        1. User에서 1:n relationship 삭제 후 배포. (pull)
            
            → ERROR “... team-provider-info.json' does not exist ...”  (해당 파일 있는데, 존재하지 않는다함)
            
            → amplify cli 업데이트  (며칠전에 API_KEY 기간 만료된게 반영이 안됐거나, 프로젝트에서 수정한 내용을 ampify로 push 하는걸 까먹어서 동기화가 안됐거나..)
            
            → 그래도 해결안됨.
            
            → `amplify configure project`
            
            → 해결! **pull 완료**.
            
            → models/index.d.ts 에서 ChatRoomUser Model의 ChatroomID랑 userID가 삭제됐는데 에러 없는지 확인. 사용하는곳 없는지.
            
            → 에러없음. 
            
        2. Message에 User와 1:1 relationship 추가 후 배포.
    
            → pull 
    
        3. 추후에 1:1 relationship 삭제하고, ForUserID key 추가함(User 정보를 모두 알 필요 없고, userID만 알면 되기때문). 원래있던 User 와 Message의 1:n 관계 다시 복구함.
            (이전에 잘못 이해하고 있던 부분 : 1:n relationship는 발신자와 메세지의 관계였고, 1:1 relationship은 수신자와 메세지의 관계임.) 
    

- **PublicKey 생성 & db에 저장**
    
    → 현재 amplify의 withAuthenticator()로 App을 실행중이라 회원가입 과정이 amplify에 구현되어있다. 회원가입시 Amplify Lambda function을 통해  danamoDB 데이터가 생성되는데, 그 과정은 서버에서 진행된다. E2E의 key들은 인터넷을 거치거나(브라우저→서버 ? ) 외부(Amplify Server)에서 생성되면 보안에 취약하기 때문에 서버로 가기 이전에 생성되는게 좋다.
    
    - 근데 어차피 public key는 DB에 저장해야되니 인터넷을 거치게되는거 아닌가? public는 서버에서 생성해도 문제 없지 않는지?
        
        public key와 private key를 tweetnacl에서 동시에, 키쌍으로 생성하기 때문에 public key만 서버에서 따로 생성할 수가 없는 듯하다. private key 는 서버에서 생성되면 안전하지 않고. 
        
    
    → 그래서 회원가입과 동시에 key 작업을 하지않고, 가입 후 따로 함수를 통해 생성&저장함.
    
- updateKeyPair function
    

---
    

### Private Key Setting at Device

- AsyncStorate library
    
    → device storage에 data 저장
    
    ```jsx
    import AsyncStorage from "@react-native-async-storage/async-storage";
    
    await AsyncStorage.setItem(PRIVATE_KEY, secretKey.toString());
    ```
    
--- 

### Message Encrypt

- map & Promise
    
    ```jsx
    await Promise.all(users.map((user) => sendMessageToUser(user, user)));
    ```
    

