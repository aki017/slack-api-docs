This method returns information about a team member.

## Arguments

This method has the URL `https://slack.com/api/users.info` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token.  
Requires scope: `users:read` |
| `user` | `U1234567890` | Required | User to get info on |

## Response

Returns a [user object](/types/user):

```
{
    "ok": true,
    "user": {
        "id": "U023BECGF",
        "name": "bobby",
        "deleted": false,
        "color": "9f69e7",
        "profile": {
            "first_name": "Bobby",
            "last_name": "Tables",
            "real_name": "Bobby Tables",
            "email": "bobby@slack.com",
            "skype": "my-skype-name",
            "phone": "+1 (123) 456 7890",
            "image_24": "https:\/\/...",
            "image_32": "https:\/\/...",
            "image_48": "https:\/\/...",
            "image_72": "https:\/\/...",
            "image_192": "https:\/\/..."
        },
        "is_admin": true,
        "is_owner": true,
        "has_2fa": true
    }
}
```

## Profile

The profile hash contains as much information as the user has supplied in the default profile fields: `first_name`, `last_name`, `real_name`, `email`, `skype`, and the `image_*` fields. Only the `image_*` fields are guaranteed to be included. Data that has not been supplied may not be present at all, may be null or may contain the empty string ("").

A user's custom profile fields may be discovered using [`users.profile.get`](/methods/users.profile.get).

## Email addresses

Apps created after January 4th, 2017 must request _both_ the `users:read` and `users:read.email` [OAuth permission scopes](/docs/oauth-scopes) when using the [OAuth app installation flow](/docs/oauth) to enable access to the `email` field of user objects returned by this method.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `user_not_found` | Value passed for `user` was invalid. |
| `user_not_visible` | The requested user is not visible to the calling user |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of common decency. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
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

