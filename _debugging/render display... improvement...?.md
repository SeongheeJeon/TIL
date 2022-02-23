### Message 로딩 개선 (왼쪽에 파란색 티끌 제거)

</br>
<p align="center" >
  <img src = "https://user-images.githubusercontent.com/49744535/155384611-7907cfef-4b6b-4fdc-9c39-17448a36b09e.gif" width="30%">&nbsp&nbsp->&nbsp&nbsp
  <img src = "https://user-images.githubusercontent.com/49744535/155387021-68373260-b890-4760-9a10-48c801878881.gif" width="30%">
</p>

# 현재

- ChatRoomScreen에서 Message render시 왼쪽에 파란색으로 살짝 비췄다가 오른쪽으로 바뀜.
    
    → 파란색이 왜 뜨는거지? 
    
    ```jsx
    return (
      <Pressable
        onLongPress={openActionMenu}
        style={[
          styles.container,
          isMe ? styles.rightContainer : styles.leftContainer,
          { width: soundURI ? "75%" : "auto" },
        ]}
      > 
      ...
      </Pressable>
    )
    ```
    
    → 파란색이 뜨는건 styles.leftContainer이 적용되었다는 건데, isMe 값이 true가 되기 전(null일 때)에 render되는건가?  
        (isMe : 내가 보낸 메세지일 경우 true, 아닐 경우 false값을 가짐. setUser 이후 할당됨)
    
    → 맞음. 
    

# 개선

- isMe 변수가 boolean 값으로 set 되었을 때에만 render 되도록
    
    ```jsx
    return (
      isMe !== null && (
        <Pressable ... > 
        ...
        </Pressable>
      )
    )
    ```

