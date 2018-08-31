Exchanges a temporary OAuth verifier code for an access token.

## Facts

| Method URL: | `https://slack.com/api/oauth.access` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 4](/docs/rate-limits#tier_t4) |

* * *

This method allows you to exchange a temporary OAuth `code` for an API access token.

This is the third step of the [OAuth authentication flow](/docs/oauth).

We strongly recommend supplying the Client ID and Client Secret using the HTTP Basic authentication scheme, as discussed [in RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.3.1).

If at all possible, avoid sending `client_id` and `client_secret` as parameters in your request.

**Keep your tokens secure**. Do not share tokens with users or anyone else.

When used with a workspace app, this method's response differs significantly. It may be used as part of the initial OAuth authentication flow to gain a [**long-lived refresh token** and **short-lived access token**](/docs/rotating-and-refreshing-credentials), or it may be used subsequently to refresh the access token.

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

The response schema for this step of OAuth differs depending on [the scopes](/scopes) requested and the type of application used. When asking for the `bot` scope, you'll receive the token separately from the user token.

Successful user token negotiation for a single scope

```
{
    "access_token": "xoxp-XXXXXXXX-XXXXXXXX-XXXXX",
    "scope": "groups:write",
    "team_name": "Wyld Stallyns LLC",
    "team_id": "TXXXXXXXXX"
}
```

Success example when asking for multiple scopes, a bot user token, and an incoming webhook

```
{
    "access_token": "xoxp-XXXXXXXX-XXXXXXXX-XXXXX",
    "scope": "incoming-webhook,commands,bot",
    "team_name": "Team Installing Your Hook",
    "team_id": "TXXXXXXXXX",
    "incoming_webhook": {
        "url": "https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX",
        "channel": "#channel-it-will-post-to",
        "configuration_url": "https://teamname.slack.com/services/BXXXXX"
    },
    "bot": {
        "bot_user_id": "UTTTTTTTTTTR",
        "bot_access_token": "xoxb-XXXXXXXXXXXX-TTTTTTTTTTTTTT"
    }
}
```

Success example using a workspace app produces a very different kind of response

```
{
    "ok": true,
    "access_token": "xoxa-access-token-string",
    "token_type": "app",
    "app_id": "A012345678",
    "app_user_id": "U0NKHRW57",
    "team_name": "Subarachnoid Workspace",
    "team_id": "T061EG9R6",
    "authorizing_user": {
        "user_id": "U061F7AUR",
        "app_home": "D0PNCRP9N"
    },
    "installer_user": {
        "user_id": "U061F7AUR",
        "app_home": "D0PNCRP9N"
    },
    "scopes": {
        "app_home": [
            "chat:write",
            "im:history",
            "im:read"
        ],
        "team": [],
        "channel": [
            "channels:history",
            "channels:read",
            "chat:write"
        ],
        "group": [
            "chat:write"
        ],
        "mpim": [
            "chat:write"
        ],
        "im": [
            "chat:write"
        ],
        "user": []
    }
}
```

Typical error response

```
{
    "ok": false,
    "error": "invalid_client_id"
}
```

## Workspace app behavior

Workspace apps may call `oauth.access` in **two different scenarios**.

1. Upon receiving a temporary authorization code during the OAuth flow, the app calls this method to gain a **refresh token** and a short-lived **access token**.
2. Subsequently, a workspace app calls this method in order to gain a new **access token** , passing the **refresh token** as argument and `grant_type=refresh_token` as a parameter.

[Read more about the refresh token and access token split](/docs/rotating-and-refreshing-credentials).

Here's a primer on the fields you will initally receive as a workspace app calling this method:

- `access_token` - Your access token for this workspace. The token string begins with `xoxa-2` and can be very long. When automatic expiration is turned on, this token will expire after the duration specified in `expires_in`. After that it must be [refreshed using a `refresh_token` and token rotation](/docs/rotating-and-refreshing-tokens).
- `refresh_token` - Your refresh token for this workspace. The token string begins with `xoxr`. Use this token to refresh a short-lived `access_token` by following the [token rotation instructions](/docs/rotating-and-refreshing-tokens).
- `expires_in` - This integer tells your app how long the access token is valid for, in seconds.
- `token_type` - You'll see `app` here. Workspace tokens were once known as `app` tokens. Maybe someday this value will become `workspace`.
- `app_id` - This is the unique ID for your whole Slack app.
- `app_user` - This is the user ID of your app. It's a user! It's an app! It's a user! It's an app! The user ID is unique to the team installing it.
- `installer_user` - This node holds the `user_id` of the user that _originally_ installed this app. It also contains the `app_home` conversation ID for that same user.
- `authorizing_user` - This node contains the `user_id` and `app_home` converation ID of the user _currently_ navigating through the authorization process. It might be different than the original installer.
- `team_name` - This is what the team calls itself.
- `team_id` - This is the unique ID for the team.
- ~~`permissions`~~ - This field is no longer available. Use [`apps.permissions.info`](/methods/apps.permissions.info) to look up all of your app's permissions for this workspace instead.
- `scopes` - All [OAuth scopes](/scopes) awarded to your app — not just those awarded this authorization attempt.
- `single_channel_id` - if this app is approved for a single channel, that channel will be listed here. See single channel authorizations for more info.

A quick note on rate-limiting: `oauth.access` follows a custom rate-limiting scheme _for the refresh case only_. After your app has received its refresh token, it may call the endpoint with `grant_type=refresh_token` to gain a new access token at a rate of 10 calls per minute, with an occasional burst of up to 50.

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

