Exchanges a temporary OAuth verifier code for a workspace token.

## Facts

| Method URL: | `https://slack.com/api/oauth.token` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | [`application/x-www-form-urlencoded`](/web#post_bodies "Learn more about sending requests") |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [user](/docs/token-types#user) | _No scope required_ |

 |

* * *

This method allows you to exchange a temporary OAuth verifier `code` for a " [workspace token](/docs/token-types#workspace)".

This is used as part of the [OAuth authentication flow](/docs/oauth) used by [workspace token-based Slack apps](/slack-apps-preview).

As discussed [in RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.3.1) it is possible to supply the Client ID and Client Secret using the HTTP Basic authentication scheme. If HTTP Basic authentication is used you do not need to supply the `client_id` and `client_secret` parameters as part of the request.

**Keep your tokens secure**. Do not share tokens with users or anyone else.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `client_id` | `4b39e9-752c4` | Required | Issued when you created your application. |
| `client_secret` | `33fea0113f5b1` | Required | Issued when you created your application. |
| `code` | `ccdaa72ad` | Required | The `code` param returned via the OAuth callback. |
| `redirect_uri` | `http://example.com` | Optional | This must match the originally submitted URI (if one was sent). |
| `single_channel` | `1` | Optional, default=0 | Request the user to add your app only to a single channel. |

<ts-icon class="ts_icon_code"></ts-icon> Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

This method's response differs from `oauth.access` significantly. This response format is also subject to change to better match responses you'll find in the [Permissions API](/docs/permissions-api).

```
{
    "ok": true,
    "access_token": "xoxa-access-token-string",
    "token_type": "app",
    "app_id": "A012345678",
    "app_user_id": "U0AB12ABC",
    "installer_user_id": "U061F7AUR",
    "authorizing_user_id": "U0HTT3Q0G",
    "team_name": "Subarachnoid Workspace",
    "team_id": "T061EG9Z9",
    "permissions": [
        {
            "scopes": [
                "channels:read",
                "chat:write:user"
            ],
            "resource_type": "channel",
            "resource_id": 0
        }
    ]
}
```

### Brief field guide:

- `access_token` - Your new workspace token that begins with `xoxa`. It can be very long.
- `token_type` - You'll see `app` here. Workspace tokens were once known as `app` tokens. Maybe someday this value will become `workspace`.
- `app_id` - This is the unique ID for your whole Slack app.
- `app_user_id` - This is the user ID of your app. It's a user! It's an app! It's a user! It's an app! The user ID is unique to the team installing it.
- `installer_user_id` - This is the user ID of the user that _originally installed_ this app.
- `authorizing_user_id` - This is the user ID of the user that initiated this authorization.
- `team_name` - This is what the team or workspace calls itself.
- `team_id` - This is the unique ID for the team or workspace.
- `permissions` - Here's where we communicate all the details of [your permissions](/docs/permissions-api). It's an array of the different permissions you have. Each array contains:
  - `scopes` - the [OAuth scopes](/scopes) associated with this permission, further scoped by the `resource_id`.
  - `resource_type` - the type of resource this permission describes: like a `channel`, a `im`, etc.
  - `resource_id` - the unique ID correlated to the `resource_type` — the direct message ID for resources of type `im`, the channel ID for a public `channel`, etc. When `0`, it's a [wildcard](/docs/permissions-api#wildcard).

More detail on this response in [Permissions API](/docs/permissions-api)

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_client_id` | Value passed for `client_id` was invalid. |
| `bad_client_secret` | Value passed for `client_secret` was invalid. |
| `invalid_code` | Value passed for `code` was invalid. |
| `bad_redirect_uri` | Value passed for `redirect_uri` did not match the `redirect_uri` in the original request. |
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

