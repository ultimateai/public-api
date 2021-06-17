# Authorization
```shell
curl 'https://chat.ultimate.ai/api/{endpoint}' \
  -H 'authorization: {accessToken}'
```

```javascript
fetch(
    'https://chat.ultimate.ai/api/{endpoint}', 
    {
        headers: {
            'authorization': '{accessToken}'
        },
    }
);
```
To authorize to our API a header named `authorization` needs to be set.
All API Requests expect this header to be present.

You can find your access token in the dashboard in your bots settings under "Tokens".

If you are not a client yet, you request your Access Token to try out the API from 
[here](https://ultimate.ai).

<aside class="notice">
Please do not share your access tokens with other users or applications and don't publish them in public code repositories.
</aside>
