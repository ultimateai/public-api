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

Field | Type  | Required | Description
---------- | ---------- | ---------- | ------------------------------
**botId** | string | true | ID of the bot that will sends the reply.<br />You can find the ID of your bot in our dashboard, or talk to your Customer Success Manger.
**conversationId** | string | true | An ID used by the chat provider to identify the conversation.<br />The chat provider can decide the format.
**text** | string | true<br />if `eventType` is `message` | The text message that the visitor typed into the chat.<br />The text is optional if the `eventType` is `startSession` or `endSession`.<br />If a button was pressed, send the value of the button as text (unless it’s a link button).
**eventType** | enum [`startSession`, `message`, `endSession`] |  true | Use `startSession` when the conversation is created. The welcomeMessage will be given as a response in case there is one for the given bot.<br />Use `message` if sending a visitor message.<br />Use `endSession` to end the session. Otherwise the Session will end automatically after 2 hours.
**metaData** | array[`MetaData`]  | false | Sending metaData gives you the ability to send metadata on the user or the session of the conversation. These params will be saved on the session and can be used by the bot to personalize the conversation flows.<br /><br />Each object of type `MetaData` in the array needs to contain the following fields:<br />**key** `string` - name the session parameter can be referenced by to personalize a conversation<br />**value** `string` - value of the session parameter, note that we only support values of type string at this moment<br />**sanitize** `bool` - optional field, should be set as true for PII data 

## Response

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
**messages** | array[`Message`] | Array containing one or several messages to show to the visitor.<br /><br />Each object of type `Message` contains the following fields:<br />**text** `string` - Message string<br />**forwardToHuman** `bool` - When `true` this conversation needs to be forwarded to a human agent.<br >**buttons** `array[Button]` - Array of buttons to display to the visitor.<br />**secondMessage** `Message` `nullable` - If this message is part of a dialogue reply it can have multiple messages. `secondMessage` is of type `Message` and can again have it's own secondMessage.<br /><br />Each object of type `Button` contains the following fields:<br />**text** `string` - value of the button, that is shown to the visitor. When the visitor presses the button, send the `text` as a message to the chat.<br />**link** `string` `nullable` - URL to go to, when the button is pressed. If the button has a link, don't send its value to the chat when pressed.
**confidenceThreshold** | float | Float value between 0 and 1 - this is the bot's confidence threshold to answer to a visitor message. If there is no intent predicted (see `predictedIntens`) above this threshold, the bolt will return the default reply.
**predictedIntents** | array[`Intent`] | Array containing intents that were predicted for the `text` sent in the request.<br /><br />Each object of type `Intent` contains the following fields:<br />**value** `string` - Unique ID identifying this intent.<br />**name** `string` - the intent's name<br />**confidence** `float` - Float value between 0 and 1 - giving the confidence with which this intent was predicted for the given `text`.  
**entities** | array [`Entity`] |  Array of entities that were extracted from the visitor message given in the request.<br /><br />Each object of type `Entity` will have the following fields:<br />**name** `string` - Name of the recognized entity<br />**value** `string` - the value that was extracted from the visitor message

## Usage
### Start a conversation

```shell
curl 'https://chat.ultimate.ai/api/v2/automation' \
  -H 'authorization: {basicAuthToken}'
  -H 'content-type: application/json;charset=UTF-8' \
  --data-raw '{"botId":"{botId}","conversationId":"{conversationId}","eventType":"startSession"}' \
  --compressed
```

```javascript
const startConversation = fetch(
        'https://chat.ultimate.ai/api/v2/automation',
        {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'authorization': '{basicAuthToken}'
            },
            body: JSON.stringify({
                botId: '{botId}',
                conversationId: '{conversationId}',
                eventType: 'startSession'
            })
        }
    );

startConversation()
    .then(res => {
        console.log(res);
    });
```

To start a new conversation (ie: when the chat widget is opened) 
create a new `conversationId` of type `string`. This ID needs to be unique 
to identify the conversation, but the format of it is up to you.
All subsequent visitor messages sent in the conversation, should use the same 
`conversationId` as the one used to start the conversation.
  
Send a `POST` request with your `conversationId`, your `botId` and the
`eventType: startSession` to start the conversation session.

The API will respond with a welcome message if there is a welcome message 
defined for the given bot. Otherwise the `messages` array in the response 
will be empty.

Once you have started a conversation session, you can start sending visitor messages
to the conversation. The session will end after 2 hours or when you send 
the `eventType: endSession` to the conversation.

<aside class="notice">
Remember — a conversation needs to be started first, by sending a request with the eventType startSession before visitor messages can be sent in the conversation.
</aside>

### Send a visitor message

```shell
curl 'https://chat.ultimate.ai/api/v2/automation' \
  -H 'authorization: {basicAuthToken}'
  -H 'content-type: application/json;charset=UTF-8' \
  --data-raw '{"botId":"{botId}","conversationId":"${conversationId}","eventType":"message" "text": "Hello! My email is jane.doe@email.com"}, metaData: [{"key": "name", "value": "Jane Doe"}]' \
  --compressed
```

```javascript
const sendMessage = fetch(
        'https://chat.ultimate.ai/api/v2/automation',
        {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'authorization': '{basicAuthToken}'
            },
            body: JSON.stringify({
                botId: '{botId}',
                conversationId: '{conversationId}',
                eventType:'message',
                text: 'Hello! My email is jane.doe@email.com',
                metaData:[
                  {
                    key:'name',
                    value:'Jane Doe'
                  }
               ]
            })
        }
    );

sendMessage()
    .then(res => {
        console.log(res);
    });
```

Once you have started a conversation session, you can start sending visitor messages 
with the same `conversationId` used when starting the conversation.
 
Use the `eventType: message` to send a visitor message receive an automated reply.

The response will contain an array with the bot messages for the highest predicted 
intent as well as a list of predicted intents for the given visitor message.

Bot messages will contain the text of the reply and might also contain an array of 
buttons and the flag `forwardToHuman` when conversations should be escalated 
to a human agent.

When a user presses one of the given buttons, send the value of the button 
as a message in the conversation to continue the dialogue. 
If the button has a link, the visitor should be redirected to the link address, 
the dialogue will not continue in this case. 

In case you are escalating the conversation to a human agent, 
use our [Suggestion Engine](#suggestion-engine) to support your agents by suggesting
replies and sending the agent's messages to the conversation session.

### End a conversation

```shell
curl 'https://chat.ultimate.ai/api/v2/automation' \
  -H 'authorization: {basicAuthToken}'
  -H 'content-type: application/json;charset=UTF-8' \
  --data-raw '{"botId":"{botId}","conversationId":"{conversationId}","eventType":"endSession"}' \
  --compressed
```

```javascript
const endConversation = fetch(
        'https://chat.ultimate.ai/api/v2/automation',
        {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'authorization': '{basicAuthToken}'
            },
            body: JSON.stringify({
                botId: '{botId}',
                conversationId: '{conversationId}',
                eventType: 'endSession'
            })
        }
    );

endConversation()
    .then(res => {
        console.log(res);
    });
```

To end new conversation (ie: when the visitor leaves the page) 
send a request with the  `eventType: endSession`. 

If the conversation does not end with a `endSession` event, it will automatically
end after 2 hours. 
