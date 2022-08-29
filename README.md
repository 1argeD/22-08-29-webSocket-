# 22-08-29-webSocket-

1. build gradle에 의존성 주입


2. webSoket 기본 설정 -> webSoketConfig 설정


3. @EnablewebSoket을 사용 -> websocket을 활성화

4. webSoketHandler 추가 -> Json 데이터를 chatMessage, class 에 맞게 변형하는 역할

5. Dto만들기 (json에 맞는 dto)

6.chatRoom -> roomID와 name, 그리고 session을 관리할 sessions를 갖는다

7. serviceController

  chatService 생성
  
8,chatController 
@RequestMapping 어노테이션을 통해서 "/chat" 주소로 post요청이 들어오면, json 데이터 값에서
name을 받아 해당 이름으로된 채팅방을 생성
