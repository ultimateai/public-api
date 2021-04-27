# Chat Automation

This endpoint is used to automate chat conversations. It accepts a visitor message and responds with a bot response and attached actions. 
A response could be a simple text reply or a dialog which includes multiple messages.
In the current API version, the only action supported is `forwardToHuman` paramter, that flags whether the conversation should be escalated to a human (the agent owning the conversation should be changed from bot to human agent / human department). 

### HTTP Request

`POST /api/automation/visitor`

### Query Parameters

Parameter | Type  | Required | Description
---------- | ---------- | ---------- | ------------------------------
botId | string | true | ID of the bot used to identify which bot sends the reply. This ID will be provided by ultimate.ai
conversationId | string | true | An ID used by the chat provider to identify the conversation. The chat provider can decide the format.
message | string | true if type is 'message' | The text message that the visitor typed into the chat. The message is optional if type is join. If a button was pressed, the message should be the value of the button (unless it’s a link button).
type | enum ['message', 'join'] |  false | Use type `join` when the conversation is created. The welcomeMessage will be given as a response in case there is one for the given bot. Use type `message` if sending a visitor message.
platform | enum ['liveAgent', 'giosg', 'liveChat', 'zendesk', 'freshchat', 'sap', 'facebook', 'zendeskSupport']  | false | CRM Platform this chat is integrated with, if applicable.

## Start a conversation

```shell
curl 'https://chat.ultimate.ai/api/automation/visitor' \
  -H 'content-type: application/json;charset=UTF-8' \
  --data-raw '{"botId":"BOT ID PROVIDED BY ULTIMATE.AI","conversationId":"NEW CONVERSATION ID","type":"join"}' \
  --compressed
```

```javascript
const startConversation = async () => {
    const response = await fetch(
        'https://chat.ultimate.ai/api/automation/visitor', 
        {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                botId: 'BOT ID PROVIDED BY ULTIMATE.AI',
                conversationId: 'NEW CONVERSATION ID',
                type: 'join'
            })
        }
    );
    return response.json();
};

startConversation()
    .then(res => {
        constole.log(res);
    });
```

> The above command returns JSON structured like this:

```json
{
  "confidenceThreshold": 0.7,
  "messages": [
    {
      "text": "Hello :)",
      "forwardToHuman": false,
      "secondMessage": {
        "text": "How can I help you today?",
        "buttons": []
      },
      "buttons": []
    }
  ],
  "predictedIntents": null
}
```

To start a conversation (ie: when the chat widget is opened) create a new `conversationId` and POST a message of type `join` with the new conversationId. 
The API will respond with the welcomeMessage if there is a welcomeMessage defined for the given bot.

All subsequent visitor messages sent in the chat, should use the same conversationId as the one used to start the conversation.

<aside class="notice">
Remember — a conversation needs to be started first, by sending a message of type join before visitor messages of type message can be sent in the conversation.
</aside>

## Send a visitor message

```shell
curl 'https://chat.ultimate.ai/api/automation/visitor' \
  -H 'content-type: application/json;charset=UTF-8' \
  --data-raw '{"botId":"BOT ID PROVIDED BY ULTIMATE.AI","conversationId":"CONVERSATION ID","type":"message" "message": "The shoes I have ordered are damaged."}' \
  --compressed
```

```javascript
const sendMessage = async () => {
    const response = await fetch(
        'https://chat.ultimate.ai/api/automation/visitor', 
        {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                botId: 'BOT ID PROVIDED BY ULTIMATE.AI',
                conversationId: 'CONVERSATION ID',
                type: 'message',
                message: 'The shoes I have ordered are damaged.'
            })
        }
    );
    return response.json();
};

startConversation()
    .then(res => {
        constole.log(res);
    });
```

> The above command returns JSON structured like this:

```json
{
  "confidenceThreshold": 0.7,
  "messages": [
    {
      "text": "I'm sorry to hear that, that must be frustrating. When did the damage happen?",
      "forwardToHuman": false,
      "buttons": [
        {
          "text": "Delivered damaged." 
        },
        {
          "text": "After use." 
        }
      ]
    }
  ],
  "predictedIntents": [
    {
      "confidence": 0.9,
      "value": "1234567890",
      "name": "Customer Claims"
    },
    {
      "confidence": 0.1,
      "value": "1234567891",
      "name": "Missing Item"
    }
  ]
}
```

Once a [conversation was started](#start-a-conversation) POST visitor messages with the same `conversationId` used when starting the conversation and use the message type `message` to receive an automated reply.
The response will contain an array with the bot messages for the highest predicted intent as well as a list of predicted intents for the given visitor message. 
Bot messages will contain the text of the reply and might also contain an array of buttons and a flag when conversations should be escalated to a human agent.

When a user presses one of the given buttons, send the value of the button as a message in the conversation to continue the dialogue.

In case you are escalating the conversation to a human agent use the [suggestions/agent endpoint](#api-suggestions-agent) to send the agent's messages to the conversation.
