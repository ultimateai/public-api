# CHAT API Converse endpoint

This endpoint of the Chat API allows external customers to converse with a chat automation virtual agent. 
It requires an Authorization header, a botid header and a JSON request body which will be used to initiate 
a conversation with the virtual agent. The endpoint returns the response asynchronously to a webhook defined 
by the customer, which allows them to handle the response as per their requirement.

## HTTP Request

Method: `POST` 

Endpoint `/chat/converse`

## Authorization

This endpoint is protected by API Key. 

When becoming a client, you will get an API Key that you will keep safe, and use it in all the requests as an Authorization header value  

<aside class="notice">
Please do not share your API Key with other users or applications and don't publish them in public code repositories.
</aside>

## Request

#### Headers:
```json
[
  {"apiKey":"YOUR_API_KEY"},
  {"botid":"YOUR_BOT_ID"}
]
```


#### Body:
```json
{
   "platformConversationId": "123456789abcdef",
   "eventType":"message",
   "text":"Hello! I want to make a refund, please",
   "cardIndex": 1,
   "metaData":[
      {
         "key":"userId",
         "value":"54321"
      }
   ]
}
```

| Field                      | Type                                           | Required                              | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|----------------------------|------------------------------------------------|---------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **platformConversationId** | string                                         | true                                  | An ID provided by customer to identify the conversation.<br />The customer can decide the format.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| **eventType**              | enum [`startSession`, `message`, `endSession`] | true                                  | Use `startSession` when the conversation is created. The welcomeMessage will be given as a response in case there is one for the given bot.<br />Use `message` if sending a visitor message.<br />Use `endSession` to end the session. Otherwise the Session will end automatically after 2 hours (MARKUS IS THIS TRUE????).                                                                                                                                                                                                                                                                                                   |
| **text**                   | string                                         | true<br />if `eventType` is `message` | The text message that the visitor typed into the chat.<br />The text is optional and not used if the `eventType` is `startSession` or `endSession`.<br />If a button was pressed (also in carousels), send the value of the button as text (unless it’s a link button).                                                                                                                                                                                                                                                                                                                                                        |
| **cardIndex**              | number                                         | false                                 | The card index of the card that the visitor clicked in the chat.<br />The card index is optional and not used if the `eventType` is `startSession` or `endSession`.<br /> Card index start in 0                                                                                                                                                                                                                                                                                                                                                                                                                                |
| **metaData**               | array[`MetaData`]                              | false                                 | Sending metaData gives you the ability to send metadata on the user or the session of the conversation. These params will be saved on the session and can be used by the bot to personalize the conversation flows.<br /><br />Each object of type `MetaData` in the array needs to contain the following fields:<br />**key** `string` - name the session parameter can be referenced by to personalize a conversation<br />**value** `string` - value of the session parameter, note that we only support values of type string at this moment<br />**sanitize** `bool` - optional field, should be set as true for PII data | 

## Response

- 200: Request successfully processed
- 400: Bad Request
- 401: Unauthorized
- 404: Bot not found

## Webhooks

When the bot emit an event (message, escalation, is team online, action), it notifies the customer via webhook.
Two webhooks need to be defined, one for chat events (message, escalation and is team online) and one for actions.

<aside class="notice">
Please note that the customer could define the same webhook for both chat and actions.
</aside>

### Webhook messages format

#### Message

##### Message with text
```json
{
  "botId": "618a30beee4e8e87d2f94015",
  "data": {
    "eventType": "sendMessage",
    "platformConversationId": "session1",
    "type": "text",
    "replyId": "618a30be1c2ffe001ab1078f",
    "buttons": [],
    "text": "Hello, this is a bot's text message",
    "carouselCards": []
  }
}
```
##### Message with buttons
```json
{
  "botId": "610abae7527db9ce033024e1",
  "data": {
    "eventType": "sendMessage",
    "platformConversationId": "session1",
    "type": "text",
    "replyId": "610abc338851860016485d79",
    "buttons": [
      {
        "type": "button",
        "text": "YES" 
      },
      {
        "type": "button",
        "text": "NO"
      }
    ],
    "text": "its a text message with 2 buttons",
    "carouselCards": []
  }
}

```
##### Message with carousel
```json
{
  "botId": "618a30beee4e8e87d2f94015",
  "data": {
    "eventType": "sendMessage",
    "platformConversationId": "session1",
    "type": "carousel",
    "replyId": "61e5439dce4387001311d6db",
    "buttons": [],
    "text": "",
    "carouselCards": [
      {
        "title": "card 1",
        "description": "card 1 description",
        "imageUrl": "URL",
        "buttons": [
          {
            "text": "button 1",
            "type": "button"
          },
          {
            "text": "button 2",
            "type": "button"
          }
        ]
      },
      {
        "title": "card 2",
        "description": "card 2 description",
        "imageUrl": "URL",
        "buttons": [
          {
            "text": "button 1",
            "type": "button"
          },
          {
            "text": "button 2",
            "type": "button"
          }
        ]
      }
    ]
  }
}
```

#### Escalate
```json
{
  "botId": "string",
  "data": {
    "eventType": "escalate",
    "platformConversationId": "string",
    "escalateTo": "string"
  }
}
```

#### Is Team Online
```json
{
  "botId": "string",
  "data": {
    "eventType": "isTeamOnline",
    "teamId": "string"
  }
}
```

#### Action
```json
{
  "botId" : "string",
  "conversationId": "string",
  "platformConversationId": "string",
  "name": "string",
  "data": [
    {
      "key": "string",
      "value": [ "any" ],
      "saveAs?": "string"
    }
  ]
}
```

## Usage
First of all you need to have the apiKey for the `Authorization` header 
and the bot ID for the `botid` header.

### Start conversation
To start a new conversation (ie: when the chat widget is opened)
create a new `platformConversationId` of type `string`. This ID needs to be unique
to identify a single conversation, but the format of it is up to you.
All subsequent visitor messages sent in the conversation, should use the same
`platformConversationId` as the one used to start the conversation.

#### Request body example to start a conversation
```json
{
  "platformConversationId": "00000001",
  "eventType": "startSession"
}
```

### Message
The payload of the message will depend on if it is a text message, a button selection or a carousel's card selection

#### Request body example for a text message
```json
{
  "platformConversationId": "00000001",
  "eventType": "message",
  "text": "I want to make a refund"
}
```

#### Request body example for clicking a button
If the virtual agent previously sent a message with buttons, like this:
```json
{
  "botId": "BOT_ID",
  "data": {
    "type": "text",
    "replyId": "SOME_REPLY_ID",
    "buttons": [
      {
        "type": "button",
        "text": "YES"
      },
      {
        "type": "button",
        "text": "NO"
      },
      {
        "type": "button",
        "text": "Maybe"
      }
    ],
    "text": "This is a virtual agent message text",
    "carouselCards": [],
    "eventType": "sendMessage",
    "platformConversationId": "00000001"
  }
}
```
To send a button click to the endpoint, we need to pass the `text` field of the button
in the `text` field of the request, like in this example selecting the third button with the text `Maybe`
```json
{
  "platformConversationId": "00000001",
  "eventType": "message",
  "text": "Maybe"
}
```

#### Request body example for clicking a carousel card
If the virtual agent previously sent a message with a carousel with two cards, like this:
```json
{
  "botId": "610abae7527db9ce033024e1",
  "data": {
    "type": "carousel",
    "replyId": "6138b04e74b9d100164b6bf5",
    "buttons": [],
    "text": "",
    "carouselCards": [
      {
        "description": "description of first card",
        "imageUrl": "IMAGE_1_URL",
        "title": "title 1",
        "buttons": [
          {
            "type": "button",
            "text": "firstCardButton"
          }
        ]
      },
      {
        "description": "description of second card",
        "imageUrl": "IMAGE_2_URL",
        "title": "title 2",
        "buttons": [
          {
            "type": "button",
            "text": "secondCardButton"
          }
        ]
      }
    ],
    "eventType": "sendMessage",
    "platformConversationId": "00000001"
  }
}
```
To send a carousel card click to the endpoint, we need to pass the `text` field of the button of the card
in the `text` field of the request and the card index (starting index in 0) in the `cardIndex` field as a number,
like in this example selecting the second card
```json
{
  "platformConversationId": "00000001",
  "eventType": "message",
  "text": "secondCardButton",
  "cardIndex": 1
}
```

### End conversation
To end an existing conversation (ie: when the chat widget is closed)
send `platformConversationId` of that conversation

#### Request body example to end a conversation
```json
{
  "platformConversationId": "00000001",
  "eventType": "endSession"
}
```


### API Client code snippets
Please make sure that you are using a secure method for storing your environment variables, 
such as a secret management tool. Storing sensitive data like API keys in plaintext or in the 
codebase can be a security risk.
#### Typescript
```javascript
import axios from 'axios';

const headers = {
  Authorization: process.env.AUTHORIZATION_HEADER,
  botid: process.env.BOT_ID
};

const request = {
  platformConversationId: "my-conversation-id",
  eventType: "message",
  text: "Hello, I want to return my shoes",
  metaData: {
    key: "user_id",
    value: "1234"
  }
};

async function converseChat() {
  try {
    const response = await axios.post('https://api.example.com/converse/chat', request, { headers });
    console.log(response.data);
  } catch (error) {
    console.error(error);
  }
}

converseChat();
```

#### Java
```java
import java.io.IOException;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.HttpClientBuilder;
import org.json.JSONObject;

public class ChatApiExample {
    public static void main(String[] args) {
        final HttpClient httpClient = HttpClientBuilder.create().build();
        final HttpPost request = new HttpPost("https://api.ultimate.ai/converse/chat");

        // Set headers
        final String authorization = System.getenv("AUTHORIZATION");
        request.setHeader("Authorization", authorization);
        final String botId = System.getenv("BOTID");
        request.setHeader("botid", botId);
        
        // Create request body
        final JSONObject requestBody = new JSONObject();
        requestBody.put("platformConversationId", "123456");
        requestBody.put("eventType", "message");
        requestBody.put("text", "What is the refund policy for the article I bought yesterday?");
        
        final StringEntity entity = new StringEntity(requestBody.toString());
        request.setEntity(entity);
        try {
            final HttpResponse response = httpClient.execute(request);
            final int statusCode = response.getStatusLine().getStatusCode();
            if (statusCode >= 200 && statusCode < 300) {
                final HttpEntity responseEntity = response.getEntity();
                System.out.println(responseEntity.getContent());
            } else {
                System.err.println("Error: "+response.getStatusLine().getReasonPhrase());
            }
        } catch (IOException e) {
            System.err.println("Error: "+e.getMessage());
        }
    }
}
```

#### Java + Spring
```java
@Service
public class ChatService {

    private final String authorization;
    private final String botid;
    private final RestTemplate restTemplate;

    @Autowired
    public ChatService(
            @Value("${CHAT_API_AUTHORIZATION}") String authorization,
            @Value("${CHAT_API_BOTID}") String botid,
            RestTemplate restTemplate
    ) {
        this.authorization = authorization;
        this.botid = botid;
        this.restTemplate = restTemplate;
    }

    public ResponseEntity<Object> converseChat(final ChatRequest chatRequest) {
        final HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization", authorization);
        headers.set("botid", botid);

        final HttpEntity<ChatRequest> request = new HttpEntity<>(chatRequest, headers);
        return restTemplate.postForEntity("https://api.ultimate.ai/converse/chat", request, Object.class);
    }
}
```

#### Java + Spring Webflux
```java
@Service
public class ChatService {

    private final String authorization;
    private final String botid;
    private final WebClient webClient;

    public ChatService(
            @Value("${CHAT_API_AUTHORIZATION}") String authorization,
            @Value("${CHAT_API_BOTID}") String botid,
            WebClient webClient
    ) {
        this.authorization = authorization;
        this.botid = botid;
        this.webClient = webClient;
    }

    public Mono<ResponseEntity<Object>> converseChat(final ChatRequest chatRequest) {
        return webClient.post()
                .uri("https://api.ultimate.ai/converse/chat")
                .header("Authorization", authorization)
                .header("botid", botid)
                .body(Mono.just(chatRequest), ChatRequest.class)
                .exchange()
                .flatMap(response -> response.toEntity(Object.class));
    }
}

```
