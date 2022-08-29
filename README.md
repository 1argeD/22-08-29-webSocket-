# 22-08-29-webSocket-

1. build gradle에 의존성 주입

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-websocket'
}


2. webSoket 기본 설정 -> webSoketConfig 설정


@EnableWebSocket
public class WebSockConfig implements WebSocketConfigurer {
    private final WebSocketHandler webSocketHandler;

    @Override
    public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
        registry.addHandler(webSocketHandler, "ws/chat").setAllowedOrigins("*");
    }
}

3. @EnablewebSoket을 사용 -> websocket을 활성화


4. webSoketHandler 추가 -> Json 데이터를 chatMessage, class 에 맞게 변형하는 역할

스프링은 Text와 Binary 타입의 헨들러를 지원, 
TextWebSocketHandler를 상속하여 WebSocketHandler를 만들어줄것이다.
 메세지를 json형식을 통해서 웹소켓을 통해 서버로 보내면, 
 Handler는 이를받아 ObjectMapper를 통해서 해당 json 데이터를 chatMessage.class에 맞게 파싱하여 ChatMessage 객체로 변환하고,
 이 json 데이터에 들어있는 roomId를 이용해서 해당 채팅방을 찾아 handlerAction() 이라는 메서드를 실행시킬 것이다. 
 그러면 handlerAction() 메서드는 이 참여자가 현재 이미 채팅방에 접속된 상태인지, 아니면 이미 채팅에 참여해있는 상태인지를 판별하여, 
 만약 채팅방에 처음 참여하는거라면 session을 연결해줌과 동시에 메시지를 보낼것이고 아니라면 메시지를 해당 채팅방으로 보내게 될 것이다.

5. Dto만들기 (json에 맞는 dto)
위의 WebSocketHandler에서 사용했던 ChatMessage와 ChatRoom 클래스를 만들어 json데이터로 받아온 정보를 통해 해당 채팅방으로 채팅 메시지를 보낼 수 있도록 해당 
json에 맞는 DTO들을 만들어주자. 간단하게 사용자가 ENTER, 처음 채팅방에 들어오는 상태인지 Talk, 이미 session에 연결 되어있고 채팅중인 상태인지를 파악하기 위해
ENUM 타입으로 Message 타입을 선언해주고, 이를 type으로 갖게 하도록한다. 
이 메세지를 보낼 채팅방 id인 roomId와, 보내는 사람의 닉네임인 sender, 메세지를 담는 변수 message를 만들어, json 데이터로 채팅

6.chatRoom -> roomID와 name, 그리고 session을 관리할 sessions를 갖는다

7. serviceController

  chatService 생성
  
8,chatController 
@RequestMapping 어노테이션을 통해서 "/chat" 주소로 post요청이 들어오면, json 데이터 값에서
name을 받아 해당 이름으로된 채팅방을 생성
