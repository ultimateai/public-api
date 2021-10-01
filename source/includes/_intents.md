# Intent Recognition

Send a visitor message from a chat or any type of text to get a 
list of matching intents with their confidences as well as a list of 
entities we extracted from the text. 
Use this endpoint to classify or label any piece of text.

## HTTP Request

Method: `POST`

Endpoint: `/api/intents`

## Authorization
```shell
curl 'https://chat.ultimate.ai/api/intents' \
  -H 'authorization: {accessToken}'
```

```javascript
fetch(
    'https://chat.ultimate.ai/api/intents', 
    {
        headers: {
            'authorization': '{accessToken}'
        },
    }
);
```

To use our Intent Recognition API a header named `authorization` needs to be set.

You can find your access token in the dashboard in your bots settings under "Tokens".

If you are not a client yet, you request your Access Token to try out the API from 
[here](https://ultimate.ai).

<aside class="notice">
Please do not share your access tokens with other users or applications and don't publish them in public code repositories.
</aside>

## Request

Send your request as a JSON with MIME-type of application/json.

> Example Request:

```json
{
   "botId":"1a2b3c4d5e6f7g8h9i0j1k2l",
   "message":"this is a message", 
   "conversationId":"12345678987654321"
}
```

Field | Type  | Required | Description
---------- | ---------- | ---------- | ------------------------------
**botId** | string | true | ID of the bot that will sends the reply.<br />You can find the ID of your bot in our dashboard, or talk to your Customer Success Manger.
**message** | string | true | The text message that the visitor typed into the chat.
**conversationId** | string | false | An ID used by the chat provider to identify the conversation.<br />The chat provider can decide the format.

## Response

> Example Response:

```json
{
    "intents": [
        {
            "confidence": 0.64636783599853516,
            "name": "intent A"
        },
        {
            "confidence": 0.54660784006118774,
            "name": "intent B"
        },
        {
            "confidence": 0.34867138862609863,
            "name": "intent C"
        },
        {
            "confidence": 0.1374831646680832,
            "name": "intent D"
        },
        {
            "confidence": 0.1208697184920311,
            "name": "intent E"
        }
    ],
    "entities": [
        {
            "confidence": 0.7,
            "form": "name.surname@mail.com",
            "index": 0,
            "name": "email",
            "id": "610abae77bf221077b4fc51e",
            "sanitize": true
        }
    ]
}
```

The endpoint will respond with a JSON with the following format:

Field | Type  | Description
---------- | ---------- | ------------------------------
**intents** | array[`Intent`] | Array containing intents that were predicted for the `message` sent in the request.<br /><br />Each object of type `Intent` contains the following fields:<br />**name** `string` - the intent's name<br />**confidence** `float` - Float value between 0 and 1 - giving the confidence with which this intent was predicted for the given `message`.
**entities** | array [`Entity`] |  Array of entities that were recognized from the visitor message given in the request.<br /><br />Each object of type `Entity` will have the following fields:<br />**name** `string` - Name of the recognized entity<br />**confidence** `float` - Float value between 0 and 1 - giving the confidence with which this entity was recognized for the given `message`.<br />**form** `string` - Form of the entity recognized in the given `message`.<br />**index** `number` - TODO Index of the entity recognized.<br />**id** `number` - Identifier of the entity recognized.<br />**sanitize** `boolean` - Boolean marking if entity is sanitized.

## Usage

```shell
curl 'https://chat.ultimate.ai/api/intents' \
  -H 'Authorization: Basic {basicAuthToken}' \
  -H 'content-type: application/json;charset=UTF-8' \
  --data-raw '{"botId":"{botId}","conversationId":"{conversationId}","message":"{message}"}' \
  --compressed
```

```javascript
const getIntents = fetch(
        'https://chat.ultimate.ai/api/intents',
        {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Basic {basicAuthToken}'
            },
            body: JSON.stringify({
                botId: '{botId}',
                message: '{message}',
                conversationId: '{conversationId}'
            })
        }
    );

getIntents()
    .then(res => {
        console.log(res);
    });
```
