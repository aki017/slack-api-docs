Exchanges a temporary OAuth verifier code for a workspace token.

## Facts

| Method URL: | `https://slack.com/api/oauth.token` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 4](/docs/rate-limits#tier_t4) |

* * *

<ts-icon class="ts_icon_sparkles"></ts-icon> **Developer preview has ended**  

This feature was exclusive to our [workspace apps developer preview](/legacy-workspace-apps). The preview has now ended, but fan-favorite features such as token rotation and the Conversations API will become available to classic Slack apps over the coming months.

This **_deprecated_** method allowed you to exchange a temporary OAuth verifier `code` for a " [**workspace token**](/docs/token-types#workspace)". Use [`oauth.access`](/methods/oauth.access) to retrieve workspace tokens instead.

This is used as part of the [OAuth authentication flow](/docs/oauth) used by [workspace apps](/workspace-apps-preview).

As discussed [in RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.3.1) it is **preferred** to supply the Client ID and Client Secret using the HTTP Basic authentication scheme. If HTTP Basic authentication is used you do not need to supply the `client_id` and `client_secret` parameters as part of the request.

**Keep your tokens secure**. Do not share tokens with users or anyone else.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `client_id` | `4b39e9-752c4` | Required | Issued when you created your application. |
| `client_secret` | `33fea0113f5b1` | Required | Issued when you created your application. |
| `code` | `ccdaa72ad` | Required | The `code` param returned via the OAuth callback. |
| `redirect_uri` | `http://example.com` | Optional | This must match the originally submitted URI (if one was sent). |
| `single_channel` | `true` | Optional, default=false | Request the user to add your app only to a single channel. |

<ts-icon class="ts_icon_code"></ts-icon> Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

This method's response differs from those offered by the classic Slack app [`oauth.access`](/methods/oauth.acces) significantly.

Success example using a workspace app produces a very different kind of response

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
    ],
    "single_channel_id": "C061EG9T2"
}
```

Typical error response

```
{
    "ok": false,
    "error": "invalid_client_id"
}
```

This structure communicates a short story:

1. An access token has been awarded or refreshed on the workspace identified by `T061EG9Z9` (_"Subarachnoid Workspace"_) for your Slack app identified by the ID `A012345678`.
2. The user who originally installed the app is the same user currently performing the authorization flow, user `U061F7AUR`.
3. The user approved the app for the [`chat:write`](/scopes/chat:write) scope, but access was only granted to a single public channel resource, `C061EG9T2`.
4. The app is automatically awarded the ability to read, write, and peruse the history of its conversation with the installing user in its `app_home`.

#### `oauth.token` response field guide

- `access_token` - Your new workspace token that begins with `xoxa`. It can be very long.
- `token_type` - You'll see `app` here. Workspace tokens were once known as `app` tokens. Maybe someday this value will become `workspace`.
- `app_id` - This is the unique ID for your whole Slack app.
- `app_user_id` - This is the user ID of your app. It's a user! It's an app! It's a user! It's an app! The user ID is unique to the team installing it.
- `installer_user_id` - This is the user ID of the user that _originally_ installed this app.
- `authorizing_user_id` - This user ID belongs to the user currently undergoing the authorization process.
- `team_name` - This is what the team calls itself.
- `team_id` - This is the unique ID for the team.
- ~~`permissions`~~ - This field is no longer available. Use [`apps.permissions.info`](/methods/apps.permissions.info) to look up all of your app's permissions for this workspace instead.
- `scopes` - the [OAuth scopes](/scopes) awarded to your app in this particular installation attempt.
- `single_channel_id` - if this app is approved for a single channel, that channel will be listed here. See single channel authorizations for more info.

Use [`apps.permissions.info`](/methods/apps.permissions.info) to look up your app's permissions on the fly.

#### Scopes categorized by resource

Scopes are grouped by resource type:

- `app_home` - your app's direct message conversation with the installer of this app
- `team` - team-level permissions assigned to your app
- `channel` - public channels
- `group` - private channels
- `mpim` - multi-member direct messages
- `im` - direct messages
- `user` - permissions to work with or as specific users

The [Permissions API](/docs/permissions-api) describes in more detail how resources and scopes work together to enforce what your app can do.

#### Single channel authorizations

To request access to only a single channel, add `single_channel=true` to the querystring you generate for the `oauth/authorize` step of the OAuth flow.

Users will be asked to approve your app's requested scopes just for a single channel they choose. When the approval process is complete, the response to `oauth.token` or `oauth.access` will contain a `single_channel_id` field noting the channel resource.

Repeating this process multiple times on the same workspace adds additional channel grants to your app, one by one.

Use this approach with the [`chat:write`](/scopes/chat:write) and [`channels:history`](/scopes/channels:history) scopes to effectively install a bot into a single channel.

Ask for only `chat:write` just to post notifications into a single channel, turning [`chat.postMessage`](/methods/chat.postMessage) into a replacement for [incoming webhooks](/incoming-webhooks).

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

