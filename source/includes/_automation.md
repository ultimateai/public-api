# Chat Automation

This endpoint can be used to automate chat conversations.
It accepts a visitor message and responds with a bot reply and attached actions.
A response could be a simple text reply or a dialog which includes multiple messages.

In the current API version, the only action supported is the `forwardToHuman` 
parameter, that flags whether the conversation should be escalated to a human agent.
The agent owning the conversation should then be changed from bot to  a human agent /
human department.

## HTTP Request

Method: `POST` 

Endpoint `/api/v2/automation`

## Authentication

```shell
curl 'https://chat.ultimate.ai/api/v2/automation' \
  -H 'authorization: {basicAuthToken}'
```

```javascript
fetch(
    'https://chat.ultimate.ai/api/v2/automation', 
    {
        headers: {
            'authorization': '{basicAuthToken}'
        },
    }
);
```
This endpoint is protected by [Basic Access Athentication](https://en.wikipedia.org/wiki/Basic_access_authentication).

If you are a client already, contact your customer support manager to create 
an API user for you. They will provide you with the Basic Auth Token for your user.

If you are not a client yet, you can request a Basic Auth Token 
to try out the API from [here](https://ultimate.ai).

Once you have your Basic Auth Token, you can authorize to the API by setting 
a header named `authorization` containing your token.

<aside class="notice">
Please do not share your auth token with other users or applications 
and don't publish them in public code repositories.
</aside>

## Request

Send your request as a JSON with MIME-type of application/json.

> Example Request:

```json
{
   "botId":"1a2b3c4d5e6f7g8h9i0j1k2l",
   "conversationId":"12345678987654321",
   "eventType":"message",
   "text":"Hello! My email is jane.doe@email.com",
   "metaData":[
      {
         "key":"name",
         "value":"Jane Doe"
      }
   ]
}
```

Parameter | Type  | Required | Description
---------- | ---------- | ---------- | ------------------------------
botId | string | true | ID of the bot that will sends the reply. This ID will be provided by ultimate.ai
conversationId | string | true | An ID used by the chat provider to identify the conversation. The chat provider can decide the format.
text | string | true if `eventType` is 'message' | The text message that the visitor typed into the chat. The text is optional if the `eventType` is 'startSession' or 'endSession'. If a button was pressed, send the value of the button as text (unless it’s a link button).
eventType | enum ['startSession', 'message', 'endSession'] |  false | Use `startSession` when the conversation is created. The welcomeMessage will be given as a response in case there is one for the given bot. Use `message` if sending a visitor message. Use `endSession` to end the session. Otherwise the Session will end automatically after 2 hours.
metaData | array  | false | Array of key-value pairs, giving you the ability to send metadata on the user or the session of the conversation. These params will be saved on the session and can be used by the bot to personalize the conversation flows. The flag `sanitize` should be set as true for PII data. Note: We currently support only string values. 

### Response

> Example Response:

```json
{
    "messages": [
        {
            "text": "Hello Jane Doe, How can i help you today?",
            "buttons": []
        },
        {
            "text": "Type your question or select one of the options below:",
            "buttons": [
                {
                    "text": "Support"
                },
                {
                    "text": "Payments"
                },
                {
                    "text": "External link",
                    "link": "https://ultimate.ai"
                }
            ]
        }
    ],
    "confidenceThreshold": 0.7,
    "predictedIntents": [
        {
            "value": "5ee3593d2e4a8a47573252e8",
            "confidence": 0.7038699984550476,
            "name": "Intent Name 1 "
        },
        {
            "value": "5f68bc4f140e681cbe8ad6ce",
            "confidence": 0.14397311210632324,
            "name": "Intent Name 2 "
        },
        {
            "value": "5eaaa247d377f1001d2628ad",
            "confidence": 0.03942745178937912,
            "name": "Intent Name 3"
        },
        {
            "value": "5fcdf52b618b6b0015da984f",
            "confidence": 0.03256703168153763,
            "name": "Intent Name 4"
        },
        {
            "value": "5d84f76a957e151f61ed5f17",
            "confidence": 0.010709668509662151,
            "name": "Intent Name 5"
        }
    ],
    "entities": [
        {
            "name": "email",
            "value": "jane.doe@email.com"
        }
    ]
}
```

The endpoint will respond with a JSON with the following format:

Field | Type  | Description
---------- | ---------- | ------------------------------
botId | string | ID of the bot that will sends the reply. This ID will be provided by ultimate.ai
conversationId | string | An ID used by the chat provider to identify the conversation. The chat provider can decide the format.
text | string | The text message that the visitor typed into the chat. The text is optional if the `eventType` is 'startSession' or 'endSession'. If a button was pressed, send the value of the button as text (unless it’s a link button).
eventType | enum ['startSession', 'message', 'endSession'] |  Use `startSession` when the conversation is created. The welcomeMessage will be given as a response in case there is one for the given bot. Use `message` if sending a visitor message. Use `endSession` to end the session. Otherwise the Session will end automatically after 2 hours.
metaData | array  | Array of key-value pairs, giving you the ability to send metadata on the user or the session of the conversation. These params will be saved on the session and can be used by the bot to personalize the conversation flows. The flag `sanitize` should be set as true for PII data. Note: We currently support only string values. 



## Usage

### Start a conversation

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

### Send a visitor message

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

### End a conversation
