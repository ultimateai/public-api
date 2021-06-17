# Intent Recognition

Send a visitor message from a chat or any type of text to get a 
list of matching intents with their confidences as well as a list of 
entities we extracted from the text. 
Use this endpoint to classify or label any piece of text.

### HTTP Request

`POST /api/intents`

### Query Parameters

Parameter | Type  | Required | Description
---------- | ---------- | ---------- | ------------------------------
botId | string | true | Unique identifier of the bot that will respond to the request 
conversationId | string | true | An ID used by the chat provider to identify the conversation. The chat provider can decide the format.
message | string | true | The text you want to classify.

### Response

The JSON response contains two fields: 

`intents` 
Array of intents that were predicted for the message in the request. The intents are sorted, with the 
top intent having the highest confidence. 

`entities`: each containing an array.

Each intent contains the following fields:

Key | Type  | Description
---------- | ---------- | ------------------------------
confidence | number | Unique identifier of the bot that will respond to the request 
name | string | An ID used by the chat provider to identify the conversation. The chat provider can decide the format.

Each entity contains the following fields:

Key | Type  | Description
---------- | ---------- | ------------------------------
confidence | float | Unique identifier of the bot that will respond to the request 
name | string | An ID used by the chat provider to identify the conversation. The chat provider can decide the format.
