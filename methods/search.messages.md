This method returns messages matching a search query.

## Arguments

This method has the URL `https://slack.com/api/search.messages` and follows the [Slack Web API calling conventions](/web#basics). <aside class="small">Present these parameters as part of an <code>application/x-www-form-urlencoded</code> querystring or POST body. <code>application/json</code> is not currently accepted.</aside>

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token.  
Requires scope: `search:read` |
| `query` | `pickleface` | Required | Search query. May contains booleans, etc. |
| `count` | `20` | Optional, default=20 | Number of items to return per page. |
| `highlight` | `true` | Optional | Pass a value of `true` to enable query highlight markers (see below). |
| `page` | `2` | Optional, default=1 | Page number of results to return. |
| `sort` | `timestamp` | Optional, default=score | Return matches sorted by either `score` or `timestamp`. |
| `sort_dir` | `asc` | Optional, default=desc | Change sort direction to ascending (`asc`) or descending (`desc`). |

## Response

The response envelope contains paging and result information:

```
{
    "ok": true,
    "query": "test",
    "messages": {
        "total": 829,
        "paging": {
            "count": 20,
            "total": 829,
            "page": 1,
            "pages": 42
        },
        "matches": [
            {...},
            {...},
            {...}
        ]
    }
}
```

The actual matches are returned as hashes containing contextual messages:

```
{
    "type": "message",
    "channel": {
        "id": "C2147483753",
        "name": "foo"
    },
    "user": "U2147483709",
    "username": "johnnytest",
    "ts": "1359414002.000003",
    "text": "mention test: johnnyrodgers".
    "permalink": "https:\/\/example.slack.com\/channels\/foo\/p1359414002000003",
    "previous_2": {
        "user": "U2147483709",
        "username": "johnnytest",
        "text": "This was said before before",
        "ts": "1359413987.000000",
        "type": "message"
    },
    "previous": {
        "user": "U2147483709",
        "username": "johnnytest",
        "text": "This was said before",
        "ts": "1359414001.000000",
        "type": "message"
    },
    "next": {
        "user": "U2147483709",
        "username": "johnnytest",
        "text": "This was said after",
        "ts": "1359414020.000000",
        "type": "message"
    },
    "next_2": {
        "user": "U2147483709",
        "username": "johnnytest",
        "text": "This was said after after",
        "ts": "1359414021.000000",
        "type": "message"
    }
}
```

Messages are searched primarily inside the message text themselves, with a lower priority on the messages immediately before and after. If more than one search term is provided, user and channel are also matched at a lower priority. To specifically search within a channel, group, or DM, add `in:channel_name`, `in:group_name`, or `in:username`. To search for messages from a specific speaker, add `from:username` or `from:botname`.

For IM results, the `type` is set to `"im"` and the `channel.name` property contains the user ID of the target user. For private group results, type is set to `"group"`.

All search methods support the `highlight` parameter. If specified, the matching query terms will be marked up in the results so that clients may replace them with appropriate highlighting markers (e.g. `<span class="highlight"></span>`). The UTF-8 markers we use are:

```
start: "\xEE\x80\x80"; # U+E000 (private-use)
end : "\xEE\x80\x81"; # U+E001 (private-use)
```

Please note that the max `count` value is `1000` and the max `page` value is `100`.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
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

