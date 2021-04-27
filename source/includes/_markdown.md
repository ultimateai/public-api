#Markdown Syntax

Here is a little cheat sheet providing an overview of different components that can be used in these markdown files to show you how things can be documented and how that will be displayed in the slate documentation.
Please make sure to remove this section from the list includes in `source/index.htl.md` before we publish.

You can find more information in the [Slate Wiki](https://calendar.google.com/calendar/u/0/r?tab=kc&pli=1).

##Sub Section

```shell
curl "https://my-api.com/endpoint" \
-H 'authorization: API_TOKEN' \
-H 'content-type: application/json;charset=UTF-8' \
--data-raw '{"parameter":"value"}' \
--compressed
```

```javascript
const queryData = async () => {
    const response = await fetch(
        'https://my-api.com/endpoint', 
        {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'authorization': API_TOKEN
            },
            body: JSON.stringify({
                parameter: 'value'
            })
        }
    );
    return response.json();
};

queryData()
    .then(res => {
        constole.log(res);
    });
```

> Code Annotations will appear in all tabs:

```json
[
  {
    "id": 1,
    "key": "Value"
  },
  {
    "id": 2,
    "key": "Value"
  }
]
```

This section will also appear as an element in the navigation.
There are two code examples for this sub section. if you want to display more tabs with other languages, add them to the list of languages in `source/index.html.md` and then add code examples for those languages where they are needed.

###Sub Sub Section I

This headline can be used to divide your documentation into several sections, that won't be listed separately in the navigation.

<aside class="notice">
Useful Information
</aside>

<aside class="success">
A Success Message
</aside>

<aside class="warning">
A Warning
</aside>

###Sub Sub Section II

Column 1 | Column 2 | Column 3
--------- | ------- | -----------
key 1 | value 1 | Lorem ipsum dolor sit amet
key 2 | value 2 | Mauris scelerisque magna eu sapien rutrum euismod
key 3 | value 3 | Praesent est urna, hendrerit vitae luctus non
key 4 | value 4 | Ut tellus quam, consectetur quis metus sit amet
key 5 | value 5 | Aliquam laoreet at leo fermentum ornare
