This method helps you test your calling code.

## Arguments

This method has the URL `https://slack.com/api/api.test` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `error` | `my_error` | Optional | Error response to return |
| `foo` | `bar` | Optional | example property to return |

## Response

The response includes any supplied arguments:

```
{
    "ok": true,
    "args": {
        "foo": "bar"
    }
}
```

If called with an `error` argument an error response is returned:

```
{
    "ok": false,
    "error": "my_error",
    "args": {
        "error": "my_error"
    }
}
```

## Errors
 This method has no expected error responses. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response. 