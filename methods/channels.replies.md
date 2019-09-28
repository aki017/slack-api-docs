Retrieve a thread of messages posted to a channel

## Facts

| Method URL: | `https://slack.com/api/channels.replies` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 3](/docs/rate-limits#tier_t3) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [user](/docs/token-types#user) | [`channels:history`](/scopes/channels:history)&nbsp; |

 |

* * *

This method returns an entire thread (a message plus all the messages in reply to it).

<ts-icon class="ts_icon_warning"></ts-icon> **Bots belonging to Slack apps are not supported**  

This method cannot be called with bot user tokens belonging to Slack apps, although legacy bot tokens will work. To use this method in a Slack app, use a [user token](/docs/token-types#user) imbued with the necessary scope. [**Stay tuned**](/changelog) for updates as we bring a fuller feast of features to bots belonging to Slack apps.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `C1234567890` | Required | Channel to fetch thread from |
| `thread_ts` | `1234567890.123456` | Required | Unique identifier of a thread's parent message |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

The `channel` and `thread_ts` arguments are always required. `thread_ts` must be the timestamp of an existing message with 0 or more replies. If there are no replies then just the single message referenced by `thread_ts` will be returned - it is just an ordinary message.

## Sample Response

Typical success response

```
{
    "has_more": false,
    "messages": {
        "0": {
            "last_read": "1509484885.000082",
            "replies": {
                "0": {
                    "ts": "1509484424.000601",
                    "user": "U2U85N1RZ"
                },
                "1": {
                    "ts": "1509484885.000082",
                    "user": "U2U85N1RZ"
                }
            },
            "reply_count": 2,
            "subscribed": true,
            "text": "This is a channel message",
            "thread_ts": "1485913694.000025",
            "ts": "1485913694.000025",
            "type": "message",
            "unread_count": 0,
            "user": "U2X9P5FEL"
        },
        "1": {
            "parent_user_id": "U2X9P5FEL",
            "text": "This is a thread reply",
            "thread_ts": "1485913694.000025",
            "ts": "1509484424.000601",
            "type": "message",
            "user": "U2U85N1RZ"
        },
        "2": {
            "parent_user_id": "U2X9P5FEL",
            "text": "This is another thread reply",
            "thread_ts": "1485913694.000025",
            "ts": "1509484885.000082",
            "type": "message",
            "user": "U2U85N1RZ"
        }
    },
    "ok": true
}
```

Typical error response

```
{
    "error": "thread_not_found",
    "ok": false
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value for `channel` was missing or invalid. |
| `thread_not_found` | Value for `thread_ts` was missing or invalid. |
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

