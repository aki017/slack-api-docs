This method opens a multiparty direct message.

## Facts

| Method URL: | `https://slack.com/api/mpim.open` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | `application/x-www-form-urlencoded`, [`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON") |
| Rate limiting: | [Tier 3](/docs/rate-limits#tier_t3) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [user](/docs/token-types#user) | [`mpim:write`](/scopes/mpim:write) |

 |

* * *

This method opens a multiparty direct message.

Opening a multiparty direct message takes a list of up-to 8 encoded user ids. If there is no MPIM already created that includes that exact set of members, a new MPIM will be created. Subsequent calls to `mpim.open` with the same set of users will return the already existing MPIM conversation.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `users` | `W1234567890,U2345678901,U3456789012` | Required | Comma separated lists of users. The ordering of the users is preserved whenever a MPIM group is returned. |

<ts-icon class="ts_icon_code"></ts-icon> This method supports `application/json` via HTTP POST. Present your `token` in your request's `Authorization` header. [Learn more](/web#posting_json).

## Response

If successful, the command returns a [mpim object](/types/mpim), including state information:

```
{
    "ok": true,
    "group": {
        "id": "G024BE91L",
        "name": "mpdm-user--user_a--user_b--user_c-1",
        "is_group": "true",
        "created": 1360782804,
        "creator": "U024BE7LH",
        "is_archived": false,
        "is_open": false,
        "is_mpim": true,
        "last_read": "0000000000.000000",
        "latest": null,
        "unread_count": 0,
        "unread_count_display": 0,
        "members": [
            "U024BE7LH",
            "U1234567890",
            "U2345678901",
            "U3456789012"
        ],
        "topic": {
            "value": "Group messaging.",
            "creator": "U024BE7LH",
            "last_set": 1360782804
        },
        "purpose": {
            "value": "Group messaging with: @user @user_a @user_b @user_c",
            "creator": "U024BE7LH",
            "last_set": 1360782804
        }
    }
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `users_list_not_supplied` | Missing `users` in request |
| `not_enough_users` | Needs at least 2 users to open |
| `too_many_users` | Needs at most 8 users to open |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. Make sure your app is a member of the conversation it's attempting to post a message to. |
| `org_login_required` | The workspace is undergoing an enterprise migration and will not be available until migration is complete. |
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

