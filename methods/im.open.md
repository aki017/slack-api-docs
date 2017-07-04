This method opens a direct message channel with another member of your Slack team.

## Arguments

This method has the URL `https://slack.com/api/im.open` and follows the [Slack Web API calling conventions](/web#basics). <aside class="small">Present these parameters as part of an <code>application/x-www-form-urlencoded</code> querystring or POST body. <code>application/json</code> is not currently accepted.</aside>

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token.  
Requires scope: `im:write` |
| `user` | `W1234567890` | Required | User to open a direct message channel with. |
| `return_im` | `true` | Optional | Boolean, indicates you want the full IM channel definition in the response. |

## Response

```
{
    "ok": true,
    "channel": {
        "id": "D024BFF1M"
    }
}
```

If the channel was already open the response will include `no_op` and`already_open` properties:

```
{
    "ok": true,
    "no_op": true,
    "already_open": true,
    "channel": {
        "id": "D024BFF1M"
    }
}
```

In either case, if the `return_im` argument was passed, the channel object will contain the full channel definition:

```
{
    "ok": true,
    "channel": {
        "id":"D024BE91L",
        "is_im":true,
        "user":"U024BE7LH",
        "created":1434412652,
        "last_read":"1442525627.000002",
        "latest":{
            "type":"message",
            "user":"U024BE7LH",
            "text":"hello",
            "ts":"1442525627.000002"
        },
        "unread_count":0,
        "unread_count_display":0,
        "is_open":true
    }
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `user_not_found` | Value passed for `user` was invalid. |
| `user_not_visible` | The calling user is restricted from seeing the requested user. |
| `user_disabled` | The `user` has been disabled. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of common decency. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
| `invalid_array_arg` | The method was passed a PHP-style array argument (e.g. with a name like `foo[7]`). These are never valid with the Slack API. |
| `invalid_charset` | The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`. |
| `invalid_form_data` | The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid. |
| `invalid_post_type` | The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`. |
| `missing_post_type` | The method was called via a `POST` request and included a data payload, but the request did not include a `Content-Type` header. |
| `team_added_to_org` | The team associated with your request is currently undergoing migration to an Enterprise Organization. Web API and other platform operations will be intermittently unavailable until the transition is complete. |
| `request_timeout` | The method was called via a `POST` request, but the `POST` data was either missing or truncated. |

## Warnings

This table lists the expected warnings that this method will return. However, other warnings can be returned in the case where the service is experiencing unexpected trouble.

| Warning | Description |
| --- | --- |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

