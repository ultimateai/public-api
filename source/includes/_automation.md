# Chat Automation

## /automation/visitor

```shell
curl "http://example.com/api/kittens" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint accepts a visitor message and responds with a bot response and attached actions. 
A response could be a simple text reply or a dialog which includes multiple messages.
In the current API version, the only action supported is a param that flags whether the conversation should be “escalated” (meaning- the agent owning the conversation should be changed from bot to human agent / human department). 

### HTTP Request

`POST /api/automation/visitor`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
botId | string | ID of the bot used to identify which bot sends the reply. Will be provided by ultimate.ai
message | string | The text message that the visitor typed into the chat, optional if type is join. If a button was pressed, the message should be the value of the button (unless it’s a link button)
conversationId | string | An id used by the chat provider to identify the conversation. Chat provider can decide the format.
type | string | Type of the message, can be `join` when chat is created and want to get the welcomeMessage, `message` if sending a message

<aside class="success">
Remember — a conversation needs to be initialized first, by sending a message of type join before visitor messages of type message can be sent in the conversation.
</aside>
