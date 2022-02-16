## End-to-end Encryption(종단간 암호화)

- 발신원부터 수신원까지 정보의 암호화를 유지한 채로 전송하는 방식. E2E(End-to-End) 암호화라고도 부름.  

- **Symmetric Encryptioin(대칭키 암호화)** : 데이터를 암호화할 때와 복호화할 때 동일한 secret key를 사용.
- **Asymmetric Encryptioin(비대칭키 암호화)** : 암호화시 public key를, 복호화시 secret key를 사용. (또는 암호화시 secret key, 복호화시 public key)
- **secret key** : (현재 프로젝트의 경우) 회원가입시 생성되며 각 사용자의 기기에 저장됨
- **public key** : (현재 프로젝트의 경우) 회원가입시 생성되며 DB에 저장.
- **과정 (예시)** :  A가 B에게 data를 전송하는 경우.
    1. A가 B에게 ‘ABC’라는 텍스트를 전송
    2. DB에서 B의 public key를 get. (B가 읽어야 하므로)
    3. B의 public key로 암호화 → 암호화된 텍스트가 DB에 저장됨.
    4. B가 DB로부터 암호화된 텍스트 get.
    5. B의 기기에 저장된 B의 private key로 복호화 → 화면에 나타남
   
- **Case of Group ChatRoom** : 5명이 참여하고있는 대화일 경우, 1:1로 각각 메세지와 key가 저장되어야 함. 그룹별로 key를 사용한다면 그룹원끼리 key를 공유하게 되는데,
  보안상으로 좋지않음.
    
- 암호화,복호화 이해에 도움이 된 글 : [tistory-sieunlim](https://sieunlim.tistory.com/16) / [KOSMOS-post](https://www.ksakosmos.com/post/r-s-a)
