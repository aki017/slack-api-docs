This method starts a Real Time Messaging API session. Refer to the [RTM API documentation](/rtm) for full details on how to use the RTM API.

## Arguments

This method has the URL `https://slack.com/api/rtm.start` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token.  
Requires scope: `client` |
| `simple_latest` | &nbsp; | Optional | Return timestamp only for latest message object of each channel (improves performance). |
| `no_unreads` | &nbsp; | Optional | Skip unread counts for each channel (improves performance). |
| `mpim_aware` | &nbsp; | Optional | Returns MPIMs to the client in the API response. |

## Response

This method returns lots of data about the current state of a team, along with a WebSocket Message Server URL:

```
{
    "ok": true,
    "url": "wss:\/\/ms9.slack-msgs.com\/websocket\/7I5yBpcvk",

    "self": {
        "id": "U023BECGF",
        "name": "bobby",
        "prefs": {
            …
        },
        "created": 1402463766,
        "manual_presence": "active"
    },
    "team": {
        "id": "T024BE7LD",
        "name": "Example Team",
        "email_domain": "",
        "domain": "example",
        "icon": {
            …
        },
        "msg_edit_window_mins": -1,
        "over_storage_limit": false
        "prefs": {
            …
        },
        "plan": "std"
    },
    "users": […],

    "channels": […],
    "groups": […],
    "mpims": […],
    "ims": […],

    "bots": […],
}
```

The `url` property contains a WebSocket Message Server URL. Connecting to this URL will initiate a Real Time Messaging session. These URLs are only valid for 30 seconds, so connect quickly!

The `self` property contains details on the authenticated user.

The `team` property contains details on the authenticated user's team. If a team has not yet set a custom icon, the value of `team.icon.image_default` will be `true`.

The `users` property contains a list of [user objects](/types/user), one for every member of the team.

The `channels` property is a list of [channel objects](/types/channel), one for every channel visible to the authenticated user. For regular or administrator accounts this list will include every team channel. The`is_member` property indicates if the user is a member of this channel. If true then the channel object will also include the topic, purpose, member list and read-state related information.

The `groups` property is a list of [group objects](/types/group), one for every group the authenticated user is in.

The `mpims` property is a list of [mpims objects](/types/mpim), one for every group the authenticated user is in. MPIMs are only returned to the client if `mpim_aware` is set when calling `rtm.start`. Otherwise, `mpims` are emulated using the groups API.

The `ims` property is a list of [IM objects](/types/im), one for every direct message channel visible to the authenticated user.

The `bots` property gives details of the integrations set up on this team.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `migration_in_progress` | Team is being migrated between servers. See [the `team_migration_started` event documentation](/events/team_migration_started) for details. |
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

