Searches for messages matching a query.

## Facts

| Method URL: | `https://slack.com/api/search.messages` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [user](/docs/token-types#user) | [`search:read`](/scopes/search:read) [`read`](/scopes/read) |

 |

* * *

This method returns messages matching a search query.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `query` | `pickleface` | Required | Search query. May contains booleans, etc. |
| `count` | `20` | Optional, default=20 | Pass the number of results you want per "page". Maximum of `100`. |
| `highlight` | `true` | Optional | Pass a value of `true` to enable query highlight markers (see below). |
| `page` | `2` | Optional, default=1 | Page number of results to return. |
| `sort` | `timestamp` | Optional, default=score | Return matches sorted by either `score` or `timestamp`. |
| `sort_dir` | `asc` | Optional, default=desc | Change sort direction to ascending (`asc`) or descending (`desc`). |

<ts-icon class="ts_icon_code"></ts-icon> Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

The matching items are returned as hashes containing contextual messages. This response envelope also contains paging and result information.

Typical success response

```
{
    "messages": {
        "matches": [
            {
                "channel": {
                    "id": "C12345678",
                    "is_ext_shared": false,
                    "is_mpim": false,
                    "is_org_shared": false,
                    "is_pending_ext_shared": false,
                    "is_private": false,
                    "is_shared": false,
                    "name": "general",
                    "pending_shared": []
                },
                "iid": "cb64bdaa-c1e8-4631-8a91-0f78080113e9",
                "permalink": "https:\/\/hitchhikers.slack.com\/archives\/C12345678\/p1508284197000015",
                "team": "T12345678",
                "text": "The meaning of life the universe and everything is 42.",
                "ts": "1508284197.000015",
                "type": "message",
                "user": "U2U85N1RV",
                "username": "roach"
            },
            {
                "channel": {
                    "id": "C12345678",
                    "is_ext_shared": false,
                    "is_mpim": false,
                    "is_org_shared": false,
                    "is_pending_ext_shared": false,
                    "is_private": false,
                    "is_shared": false,
                    "name": "random",
                    "pending_shared": []
                },
                "iid": "9a00d3c9-bd2d-45b0-988b-6cff99ae2a90",
                "permalink": "https:\/\/hitchhikers.slack.com\/archives\/C12345678\/p1508795665000236",
                "team": "T12345678",
                "text": "The meaning of life the universe and everything is 101010",
                "ts": "1508795665.000236",
                "type": "message",
                "user": "",
                "username": "robot overlord"
            }
        ],
        "pagination": {
            "first": 1,
            "last": 2,
            "page": 1,
            "page_count": 1,
            "per_page": 20,
            "total_count": 2
        },
        "paging": {
            "count": 20,
            "page": 1,
            "pages": 1,
            "total": 2
        },
        "total": 2
    },
    "ok": true,
    "query": "The meaning of life the universe and everything"
}
```

Typical error response

```
{
    "error": "No query passed",
    "ok": false
}
```

Messages are searched primarily inside the message text themselves, with a lower priority on the messages immediately before and after. Search results may be returned in which matches are only on the `previous`, `previous_2`, `next`, or `next_2` messages, but not the message itself. When a search query matches more than one message in close proximity to each other only one match will be returned. Using the`highlights=true` parameter you can identify which items match the query, and which are provided for context only.

If more than one search term is provided, users and channels are also matched at a lower priority. To specifically search within a channel, group, or DM, add `in:channel_name`, `in:group_name`, or `in:username`. To search for messages from a specific speaker, add `from:username` or`from:botname`.

For IM results, the `type` is set to `"im"` and the `channel.name` property contains the user ID of the target user. For private group results, type is set to `"group"`.

All search methods support the `highlight` parameter. If specified, the matching query terms will be marked up in the results so that clients may replace them with appropriate highlighting markers (e.g. `<span class="highlight"></span>`). The UTF-8 markers we use are:

```
start: "\xEE\x80\x80"; # U+E000 (private-use)
end : "\xEE\x80\x81"; # U+E001 (private-use)
```

Please note that the max `count` value is `100` and the max `page` value is `100`.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of accepted or expected values. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
| `invalid_array_arg` | The method was passed a PHP-style array argument (e.g. with a name like `foo[7]`). These are never valid with the Slack API. |
| `invalid_charset` | The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`. |
| `invalid_form_data` | The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid. |
| `invalid_post_type` | The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`. |
| `missing_post_type` | The method was called via a `POST` request and included a data payload, but the request did not include a `Content-Type` header. |
| `team_added_to_org` | The workspace associated with your request is currently undergoing migration to an Enterprise Organization. Web API and other platform operations will be intermittently unavailable until the transition is complete. |
| `request_timeout` | The method was called via a `POST` request, but the `POST` data was either missing or truncated. |
| `fatal_error` | The server could not complete your operation(s) without encountering a catastrophic error. It's possible some aspect of the operation succeeded before the error was raised. |

## Warnings

This table lists the expected warnings that this method will return. However, other warnings can be returned in the case where the service is experiencing unexpected trouble.

| Warning | Description |
| --- | --- |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

