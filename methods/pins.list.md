Lists items pinned to a channel.

## Facts

| Method URL: | `https://slack.com/api/pins.list` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`pins:read`](/scopes/pins:read)&nbsp; |
| [user](/docs/token-types#user) | [`pins:read`](/scopes/pins:read)&nbsp; |

 |

* * *

This method lists the items pinned to a channel.

<ts-icon class="ts_icon_warning"></ts-icon> **Bots belonging to Slack apps are not supported**  

This method cannot be called with bot user tokens belonging to Slack apps, although legacy bot tokens will work. To use this method in a Slack app, use a [user token](/docs/token-types#user) imbued with the necessary scope. [**Stay tuned**](/changelog) for updates as we bring a fuller feast of features to bots belonging to Slack apps.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `C1234567890` | Required | Channel to get pinned items for. |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

The response contains a list of pinned items in a channel.

Typical success response

```
{
    "items": [
        {
            "channel": "C2U86NC6H",
            "created": 1508881078,
            "created_by": "U2U85N1RZ",
            "message": {
                "permalink": "https://hitchhikers.slack.com/archives/C2U86NC6H/p1508197641000151",
                "pinned_to": [
                    "C2U86NC6H"
                ],
                "text": "What is the meaning of life?",
                "ts": "1508197641.000151",
                "type": "message",
                "user": "U2U85N1RZ"
            },
            "type": "message"
        },
        {
            "channel": "C2U86NC6H",
            "created": 1508880991,
            "created_by": "U2U85N1RZ",
            "message": {
                "permalink": "https://hitchhikers.slack.com/archives/C2U86NC6H/p1508284197000015",
                "pinned_to": [
                    "C2U86NC6H"
                ],
                "text": "The meaning of life, the universe, and everything is 42.",
                "ts": "1503289197.000015",
                "type": "message",
                "user": "U2U85N1RZ"
            },
            "type": "message"
        }
    ],
    "ok": true
}
```

Typical error response

```
{
    "ok": false,
    "error": "invalid_auth"
}
```

Different item types can be pinned. Every item in the list has a `type` property, and the other properties depend on the type of item. The possible types are:

- **`message`** : the item will have a `message` property containing a [message object](/docs/messages) and a `channel` property containing the channel ID for the message.
- **`file`** : this item will have a `file` property containing a [file object](/types/file).
- **`file_comment`** : the item will have a `file` property containing the [file object](/types/file) and a `comment` property containing the file comment.

The `created` property on each item is a unix timestamp representing when the item was pinned. The `created_by` property on each item is a string representing the encoded user id of the user who pinned the item.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. Make sure your app is a member of the conversation it's attempting to post a message to. |
| `org_login_required` | The workspace is undergoing an enterprise migration and will not be available until migration is complete. |
| `ekm_access_denied` | Administrators have suspended the ability to post a message. |
| `missing_scope` | The token used is not granted the specific scope permissions required to complete this request. |
| `invalid_arguments` | The method was called with invalid arguments. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of accepted or expected values. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
| `invalid_charset` | The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`. |
| `invalid_form_data` | The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid. |
| `invalid_post_type` | The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/json` `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`. |
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

