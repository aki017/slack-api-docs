Gets reactions for an item.

## Facts

| Method URL: | `https://slack.com/api/reactions.get` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [workspace](/docs/token-types#workspace) | [`reactions:read`](/scopes/reactions:read) |
| [user](/docs/token-types#user) | [`reactions:read`](/scopes/reactions:read) [`read`](/scopes/read) |

 |

* * *

This method returns a list of all reactions for a single item (file, file comment, channel message, group message, or direct message).

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `C1234567890` | Optional | Channel where the message to get reactions for was posted. |
| `file` | `F1234567890` | Optional | File to get reactions for. |
| `file_comment` | `Fc1234567890` | Optional | File comment to get reactions for. |
| `full` | `true` | Optional | If true always return the complete reaction list. |
| `timestamp` | `1234567890.123456` | Optional | Timestamp of the message to get reactions for. |

<ts-icon class="ts_icon_code"></ts-icon> Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

The response contains the item with reactions.

Typical success response

```
{
    "file": {
        "channels": [
            "C2U7V2YA2"
        ],
        "comments_count": 1,
        "created": 1507850315,
        "groups": [],
        "id": "F7H0D7ZA4",
        "ims": [],
        "name": "computer.gif",
        "reactions": [
            {
                "count": 1,
                "name": "stuck_out_tongue_winking_eye",
                "users": [
                    "U2U85N1RV"
                ]
            }
        ],
        "timestamp": 1507850315,
        "title": "computer.gif",
        "user": "U2U85N1RV"
    },
    "ok": true,
    "type": "file"
}
```

Typical error response

```
{
    "ok": false,
    "error": "invalid_auth"
}
```

Different item types can be reacted to. The item returned has a `type` property, the other properties depend on the type of item. The possible types are:

- **`message`** : the item will have a `message` property containing a [message object](/docs/messages) and a `channel` property containing the channel ID for the message.
- **`file`** : this item will have a `file` property containing a [file object](/types/file).
- **`file_comment`** : the item will have a `file` property containing the [file object](/types/file) and a `comment` property containing the file comment.

The users array in the `reactions` property might not always contain all users that have reacted (we limit it to X users, and X might change), however `count` will always represent the count of all users who made that reaction (i.e. it may be greater than `users.length`). If the authenticated user has a given reaction then they are guaranteed to appear in the `users` array, regardless of whether `count` is greater than `users.length` or not. If the complete list of users is required they can still be obtained by specifying the `full` argument.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `bad_timestamp` | Value passed for `timestamp` was invalid. |
| `file_not_found` | File specified by `file` does not exist. |
| `file_comment_not_found` | File comment specified by `file_comment` does not exist. |
| `message_not_found` | Message specified by `channel` and `timestamp` does not exist. |
| `no_item_specified` | `file`, `file_comment`, or combination of `channel` and `timestamp` was not specified. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. |
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

