# Ticket Automation with Salesforce

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris scelerisque magna eu 
sapien rutrum euismod. Praesent est urna, hendrerit vitae luctus non, mollis non odio. 
Suspendisse potenti. Pellentesque pharetra ultrices lectus, sit amet accumsan justo 
pellentesque pretium. Quisque et mattis leo, ut elementum magna. Praesent at placerat 
quam. Ut tellus quam, consectetur quis metus sit amet, fermentum suscipit tortor. 
Donec elit elit, pellentesque a turpis a, porttitor auctor metus. Fusce non rhoncus 
erat, quis placerat tortor. Morbi molestie porta purus et consequat. Aliquam laoreet 
at leo fermentum ornare. Fusce ut lobortis tortor, in tincidunt purus.

## Authentication

```shell
curl 'https://chat.ultimate.ai/api/v2/automation' \
  -H 'Authorization: Basic {basicAuthToken}'
```

```javascript
fetch(
    'https://chat.ultimate.ai/api/v2/automation', 
    {
        headers: {
            'Authorization': 'Basic {basicAuthToken}'
        },
    }
);
```
This endpoint is protected by [Basic Access Athentication](https://en.wikipedia.org/wiki/Basic_access_authentication).

If you are a client already, you can create your basic access token by generating a 
[Base64](https://en.wikipedia.org/wiki/Base64) encoded string using your email and password 
joined by a colon: `jane.doe@email.com:janesPassword`

If you are not a client yet, you can request access to try out the API from 
[here](https://ultimate.ai).

Once you have your Basic Auth Token, you can authorize to the API by setting 
a header named `Authorization` containing your token in the form of 
`Basic {basicAuthToken}`.

<aside class="notice">
Please do not share your auth token with other users or applications 
and don't publish them in public code repositories.
</aside>

## Ticket Automation

Send a visitor’s email to receive matching intents with confidences and SF ticket parameters.

### HTTP Request

Method: `POST`

Endpoint: `/salesforce/ticket/converse`

### Request

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris scelerisque magna eu 
sapien rutrum euismod. Praesent est urna, hendrerit vitae luctus non, mollis non odio. 
Suspendisse potenti. Pellentesque pharetra ultrices lectus, sit amet accumsan justo 
pellentesque pretium. Quisque et mattis leo, ut elementum magna. Praesent at placerat 
quam. Ut tellus quam, consectetur quis metus sit amet, fermentum suscipit tortor. 
Donec elit elit, pellentesque a turpis a, porttitor auctor metus. Fusce non rhoncus 
erat, quis placerat tortor. Morbi molestie porta purus et consequat. Aliquam laoreet 
at leo fermentum ornare. Fusce ut lobortis tortor, in tincidunt purus.

### Response

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris scelerisque magna eu 
sapien rutrum euismod. Praesent est urna, hendrerit vitae luctus non, mollis non odio. 
Suspendisse potenti. Pellentesque pharetra ultrices lectus, sit amet accumsan justo 
pellentesque pretium. Quisque et mattis leo, ut elementum magna. Praesent at placerat 
quam. Ut tellus quam, consectetur quis metus sit amet, fermentum suscipit tortor. 
Donec elit elit, pellentesque a turpis a, porttitor auctor metus. Fusce non rhoncus 
erat, quis placerat tortor. Morbi molestie porta purus et consequat. Aliquam laoreet 
at leo fermentum ornare. Fusce ut lobortis tortor, in tincidunt purus.

## Error Logging and Alerting

Send received errors for logging and alerting

### HTTP Request

Method: `POST`

Endpoint: `/salesforce/ticket/error`

### Request

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris scelerisque magna eu 
sapien rutrum euismod. Praesent est urna, hendrerit vitae luctus non, mollis non odio. 
Suspendisse potenti. Pellentesque pharetra ultrices lectus, sit amet accumsan justo 
pellentesque pretium. Quisque et mattis leo, ut elementum magna. Praesent at placerat 
quam. Ut tellus quam, consectetur quis metus sit amet, fermentum suscipit tortor. 
Donec elit elit, pellentesque a turpis a, porttitor auctor metus. Fusce non rhoncus 
erat, quis placerat tortor. Morbi molestie porta purus et consequat. Aliquam laoreet 
at leo fermentum ornare. Fusce ut lobortis tortor, in tincidunt purus.

### Response

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris scelerisque magna eu 
sapien rutrum euismod. Praesent est urna, hendrerit vitae luctus non, mollis non odio. 
Suspendisse potenti. Pellentesque pharetra ultrices lectus, sit amet accumsan justo 
pellentesque pretium. Quisque et mattis leo, ut elementum magna. Praesent at placerat 
quam. Ut tellus quam, consectetur quis metus sit amet, fermentum suscipit tortor. 
Donec elit elit, pellentesque a turpis a, porttitor auctor metus. Fusce non rhoncus 
erat, quis placerat tortor. Morbi molestie porta purus et consequat. Aliquam laoreet 
at leo fermentum ornare. Fusce ut lobortis tortor, in tincidunt purus.
