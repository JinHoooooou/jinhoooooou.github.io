---
title: "KwinfoBot 리팩토링(1)"
excerpt: "더러운, 악취나는 코드를 가독성있게 고쳐보자"

categories:
 - Blog
tags:
 - Refactoring
 - Java
 - Spring
last_modified_at: 2020-02-24
---



## Refactoring

코드의 외부동작은 바꾸지 않으면서 내부 구조를 좀더 가독성있게 바꾸는 방법이다. `자바로 배우는 리팩토링 입문`책과 구글링을 참고하여 내 **더러운 코드**를 **깨끗한 코드**로 만들어보자

>  완벽하게 잘 고쳤다는 아직 할 수 없지만 최대한 사람들이 봤을때 "어떤 동작을 하겠구나" 라고 알 수 있게끔 코드를 만드려고 노력해보았다.

KwInfoBot 프로젝트에서 내가 봇을 호출하고, 내가 보낸 메세지에 따라 다른 서비스를 제공한다.

"버스"라고 메세지를 보내면 광운대정문, 월계현대아파트, 안암오거리의 173, 1017버스의 도착정보를 출력한다.

"열람실"이라고 메세지를 보내면 광운대 1, 2, 3열람실의 좌석정보를 출력한다.

"공지"라고 메세지를 보내면 광운대 공지사항 상위 25개 목록을 출력한다.

그 외의 메세지는 내가 보낸 메세지를 그대로 출력한다(echo)  

* 매직넘버를 상수로

  RequsetBodyDTO로 받은 내 메세지를 확인해보니 내가 @slackbottest2를 호출하면 텍스트로 "<@USJBNQ36>"으로 나온다. 그래서 모든 메소드에서 `if(message.equals("<@USJBNQ36>"))`으로 체크했었다. 이것 외에도 출력할 공지사항 개수를 25라고 해두거나, flag에 따라 bus open api url을 바꿔주는데 그냥 0, 1, 2라고 표현하는 등 매직넘버를 그냥 뒀었는데, 이것들을 따로 상수로 꺼냈다.

  ```java
  private final static String SLACK_BOT_NAME = "<@USJBNQ36>";
  private final static int KWU = 0;
  private final static int HYUNDAI_APT = 1;
  private final static int ANAM_STREET = 2;
  private final static int KWU_NOTICE_COUNT = 25;
  ```

  확실히 수정할 때 이부분만 수정해주면 되기 때문에, 그리고 다른사람이 봤을때 생각해보면 가독성이 좋아졌다.

* 메소드 추출
  * echoMyMessage

  ![image-20200225122512433]({{site.url}}/assets/images/2020-02-23-Refactoring.assets/image-20200225122512433.png)

  크게 세 부분으로 나눌 수 있다.

  1. 내가 보낸 메세지가 @slackbottest2인지 확인한다. 

  2. 맞다면 내가 보낸 메세지에서 @slackbottest2를 제외한 나머지 text를 추출한다. 

     ex. "@slackbottest2 사랑해"  --> "사랑해" 추출

  3. 추출한 메세지를 channel에 전송한다.

  그래서 내가 보낸 메세지에 SLACK_BOT_NAME이 포함되어있는지 확인하고

  출력할 메세지를 가공한 뒤

  가공한 메세지를 다시 채널에 출력하도록 메소드를 추출했다.

  ![image-20200225123014760]({{site.url}}/assets/images/2020-02-23-Refactoring.assets/image-20200225123014760.png)

  ```java
  private boolean containCorrectBotName(String messageFromUser, String slackBotName) {
  	    String eachWord[] = messageFromUser.split(" ");
  	    for(int i = 0; i<eachWord.length; i++){
  	        if(eachWord[i].equals(slackBotName) && !eachWord[eachWord.length-1].equals(slackBotName))
  	            return true;
          }
  	    return false;
  	}
  
  private boolean extractMyMessage(String messageFromUser, String SlackBotName){
      return messageFromUser.subString(messageFromUser.indexOf(slackBotName) + slackBotName.length() + 1);
  }
  
  private void sendBotMessageToChannel(String message, String channel){
      MultiValueMap<String, String> parameters = new LinkedMultiValueMap<>();
      parameters.add("text", message);
      parameters.add("channel", channel);
      parameters.add("token", botToken);
      
      RestTemplate restTemplate = new RestTemplate();
      final String baseUrl = "https://slack.com/api/chat.postMessage";
  	URI uri = new URI(baseUrl);
  
  	ResponseEntity<String> response = restTemplate.postForEntity(uri, parameters, String.class);
  	restTemplate.delete(baseUrl);
  }
  ```

  

  * sendBusArriveTime, sendKwuNotice, sendKwuStudyRoomSeat는
    1. 내가 보낸 메세지에서 봇 호출을 제대로 했는지 확인
    2. Jsoup을 이용해서 url에 해당하는 html, xml 파싱후, 원하는 데이터 추출
    3. 추출한 데이터를 통해 채널에 보낼 메세지 포맷팅
    4. 메세지를 채널에 출력

  에 맞춰 리팩토링 했다.

  ![image-20200225143956755]({{site.url}}/assets/images/2020-02-23-Refactoring.assets/image-20200225143956755.png)

  ![image-20200225144017023]({{site.url}}/assets/images/2020-02-23-Refactoring.assets/image-20200225144017023.png)

* 클래스 추출

  클래스 추출은 아직 X

