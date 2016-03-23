This method returns a list of all multiparty direct message channels that the user has.

## Arguments

This method has the URL `https://slack.com/api/mpim.list` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `mpim:read`) |

## Response

Returns a list of [group objects](/types/group):

```
{
    "ok": true,
    "groups": [
        {
            "id": "G024BE91L",
            "name": "dm-messaging-user-1",
            "created": 1360782804,
            "creator": "U024BE7LH",
            "is_archived": false,
            "is_mpim": true
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
        },
        ....
    ]
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `invalid_array_arg` | The method was passed a PHP-style array argument (e.g. with a name like `foo[7]`). These are never valid with the Slack API. |
| `invalid_charset` | The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`. |
| `invalid_form_data` | The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid. |
| `invalid_post_type` | The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/json` `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`. |
| `missing_post_type` | The method was called via a `POST` request and included a data payload, but the request did not include a `Content-Type` header. |
| `request_timeout` | The method was called via a `POST` request, but the `POST` data was either missing or truncated. |

## Warnings

This table lists the expected warnings that this method will return. However, other warnings can be returned in the case where the service is experiencing unexpected trouble.

| Warning | Description |
| --- | --- |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

