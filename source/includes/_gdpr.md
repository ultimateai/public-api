# Delete User Data

Delete a user's conversation and message data based on conversationIds. 

## HTTP Request

Method: `POST`

Endpoint: `/api/gdpr/delete-user-data`

## Authorization
```shell
curl 'https://chat.ultimate.ai/api/gdpr/delete-user-data' \
  -H 'authorization: {basicAuthToken}'
```

```javascript
fetch(
    'https://chat.ultimate.ai/api/gdpr/delete-user-data', 
    {
        headers: {
            'authorization': '{basicAuthToken}'
        },
    }
);
```

To use the delete user data API endpoint, a header named `authorization` needs to be set.

## Request

Send your request as a JSON with MIME-type of application/json.

> Example Request:

```json
{
   "conversationIds":["12345678987654321","567456789876543255" ]
}
```

Field | Type  | Required | Description
---------- | ---------- | ---------- | ------------------------------
**conversationIds** | array | true | An array of IDs used to fetch the user's messages and conversations to be deleted<br />

## Response

> Example Response:
The endpoint will respond with a status code depending on the deletion process results:

Status Code | Type  | Description
---------- | ---------- | ------------------------------
**200** | Success  | deletion process was successfully completed.
**400** | Bad Request Error  | request data passed was not valid.
**401** | Auth Error  | user does not have the right permissions, auth token is invalid or expired.
**500** | Internal Server Error  | an unknown server error occurred.

## Usage

```shell
curl 'https://chat.ultimate.ai/api/gdpr/delete-user-data' \
  -H 'Authorization: Basic {basicAuthToken}' \
  -H 'content-type: application/json;charset=UTF-8' \
  --data-raw '"conversationIds":"[{conversationId}]"' \
  --compressed
```

```javascript
const deleteUserData = fetch(
        'https://chat.ultimate.ai/api/gdpr/delete-user-data',
        {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Basic {basicAuthToken}'
            },
            body: JSON.stringify({
                conversationId: '[{conversationId}, {conversationId}]'
            })
        }
    );

deleteUserData()
    .then(res => {
        console.log(res);
    });
```
