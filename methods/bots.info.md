Gets information about a bot user.

## Facts

| Method URL: | `https://slack.com/api/bots.info` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 3](/docs/rate-limits#tier_t3) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [workspace](/docs/token-types#workspace) | [`users:read`](/scopes/users:read) |
| [user](/docs/token-types#user) | [`users:read`](/scopes/users:read) |

 |

* * *

This method returns extended information about a [bot user](/bot-users).

Per workspace, bot users have both a user ID (which can be looked up using [`users.info`](/methods/users.info)) and a "bot ID."

`bot_id` fields appear in [`bot_message`](/events/message/bot_message) message event subtypes and in the response of methods like [`conversations.history`](/methods/conversations.history).

Use this method to look up the username and icons for those bot users. Use the `app_id` field to identify whether a bot belongs to your [Slack app](/slack-apps).

Look for a `user_id` When the bot corresponds directly to a bot user account. Some bot-like entities aren't actually "bot users" and will not include a `user_id`.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `bot` | `B12345678` | Optional | Bot user to get info on |

<ts-icon class="ts_icon_code"></ts-icon> Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

When successful, returns bot info by bot ID.

```
{
    "ok": true,
    "bot": {
        "id": "B061F7JQ1",
        "deleted": false,
        "name": "commandeer",
        "updated": 1449272004,
        "app_id": "A061BLERW",
        "icons": {
            "image_36": "https://...",
            "image_48": "https://...",
            "image_72": "https://..."
        }
    }
}
```

When successful, returns bot info by bot ID.

```
{
    "ok": true,
    "bot": {
        "id": "B061F7JD2",
        "deleted": false,
        "name": "beforebot",
        "updated": 1449272004,
        "app_id": "A161CLERW",
        "user_id": "U012ABCDEF",
        "icons": {
            "image_36": "https://...",
            "image_48": "https://...",
            "image_72": "https://..."
        }
    }
}
```

When no bot can be found, it returns an error.

```
{
    "ok": false,
    "error": "bot_not_found"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `bot_not_found` | Value passed for `bot` was invalid. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. Make sure your app is a member of the conversation it's attempting to post a message to. |
| `org_login_required` | The workspace is undergoing an enterprise migration and will not be available until migration is complete. |
| `invalid_arguments` | The method was called with invalid arguments. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of accepted or expected values. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
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

