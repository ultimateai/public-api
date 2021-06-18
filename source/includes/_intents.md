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
curl 'https://chat.ultimate.ai/api/intent' \
  -H 'authorization: {accessToken}'
```

```javascript
fetch(
    'https://chat.ultimate.ai/api/api/intents', 
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

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris scelerisque magna eu 
sapien rutrum euismod. Praesent est urna, hendrerit vitae luctus non, mollis non odio. 
Suspendisse potenti. Pellentesque pharetra ultrices lectus, sit amet accumsan justo 
pellentesque pretium. Quisque et mattis leo, ut elementum magna. Praesent at placerat 
quam. Ut tellus quam, consectetur quis metus sit amet, fermentum suscipit tortor. 
Donec elit elit, pellentesque a turpis a, porttitor auctor metus. Fusce non rhoncus 
erat, quis placerat tortor. Morbi molestie porta purus et consequat. Aliquam laoreet 
at leo fermentum ornare. Fusce ut lobortis tortor, in tincidunt purus.

## Response

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris scelerisque magna eu 
sapien rutrum euismod. Praesent est urna, hendrerit vitae luctus non, mollis non odio. 
Suspendisse potenti. Pellentesque pharetra ultrices lectus, sit amet accumsan justo 
pellentesque pretium. Quisque et mattis leo, ut elementum magna. Praesent at placerat 
quam. Ut tellus quam, consectetur quis metus sit amet, fermentum suscipit tortor. 
Donec elit elit, pellentesque a turpis a, porttitor auctor metus. Fusce non rhoncus 
erat, quis placerat tortor. Morbi molestie porta purus et consequat. Aliquam laoreet 
at leo fermentum ornare. Fusce ut lobortis tortor, in tincidunt purus.
