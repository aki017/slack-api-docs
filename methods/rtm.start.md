Starts a Real Time Messaging session.

## Facts

| Method URL: | `https://slack.com/api/rtm.start` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 1](/docs/rate-limits#tier_t1) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [user](/docs/token-types#user) | [`rtm:stream`](/scopes/rtm:stream) [`client`](/scopes/client) |

 |

* * *

This method begins a Real Time Messaging API session and reserves your application a specific URL with which to connect via websocket.

It's user-centric and team-centric: your app connects _as_ a specific user or bot user on a specific team. Many apps will find the [Events API](/events-api)'s subscription model more scalable when working against multiple teams.

This method also returns a smorgasbord of data about the team, its channels, and members. Some times more information than can be provided in a timely or helpful manner.

Please use [`rtm.connect`](/methods/rtm.connect) instead, especially when connecting on behalf of an [Enterprise Grid](/enterprise-grid) customer.

Consult the [RTM API documentation](/rtm) for full details on using the RTM API. You'll also find our [changelog entry](/changelog/2017-04-start-using-rtm-connect-and-stop-using-rtm-start) useful.

The `members` array found in this and other methods will begin automatically truncating at 1,500 and eventually fewer results beginning December 1, 2017. As of March, 2018 the cap is 500. Please Use [`conversations.members`](/methods/conversations.members) to manage memberships instead. [Read on to learn more.](/changelog/2017-10-members-array-truncating)

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `batch_presence_aware` | `1` | Optional, default=false | Batch presence deliveries via subscription. Enabling changes the shape of `presence_change` events. See [batch presence](/docs/presence-and-status#batching). |
| `include_locale` | `true` | Optional | Set this to `true` to receive the locale for users and channels. Defaults to `false` |
| `mpim_aware` | `true` | Optional | Returns MPIMs to the client in the API response. |
| `no_latest` | `1` | Optional, default=0 | Exclude latest timestamps for channels, groups, mpims, and ims. Automatically sets `no_unreads` to `1` |
| `no_unreads` | `true` | Optional | Skip unread counts for each channel (improves performance). |
| `presence_sub` | `true` | Optional, default=true | Only deliver presence events when requested by subscription. See [presence subscriptions](/docs/presence-and-status#subscriptions). |
| `simple_latest` | `true` | Optional | Return timestamp only for latest message object of each channel (improves performance). |

<ts-icon class="ts_icon_code"></ts-icon> Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

Note that setting `no_latest=1` will automatically set `no_unreads=1`.

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
            ...
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
            ...
        },
        "msg_edit_window_mins": -1,
        "over_storage_limit": false
        "prefs": {
            ...
        },
        "plan": "std"
    },
    "users": [...],

    "channels": [...],
    "groups": [...],
    "mpims": [...],
    "ims": [...],

    "bots": [...],
}
```

The `url` property contains a WebSocket Message Server URL. Connecting to this URL will initiate a Real Time Messaging session. These URLs are only valid for 30 seconds, so connect quickly!

The `self` property contains details on the authenticated user.

The `team` property contains details on the authenticated user's team. If a team has not yet set a custom icon, the value of `team.icon.image_default` will be `true`.

The `users` property contains a list of [user objects](/types/user), one for every member of the team. `presence` is no longer included for users. The list of users may truncate on especially long teams. Use [Web API methods](/methods#users) to retrieve information about users instead.

The `channels` property is a list of [channel objects](/types/channel), one for every channel visible to the authenticated user. For regular or administrator accounts this list will include every team channel. The`is_member` property indicates if the user is a member of this channel. If true then the channel object will also include the topic, purpose, member list and read-state related information. The `latest`, `unread_count`, and `unread_count_display` attributes have been removed from this method's response. See [this changelog entry](/changelog/2017-04-start-using-rtm-connect-and-stop-using-rtm-start).

The `groups` property is a list of [group objects](/types/group), one for every group the authenticated user is in.

The `mpims` property is a list of [mpims objects](/types/mpim), one for every group the authenticated user is in. MPIMs are only returned to the client if `mpim_aware` is set when calling `rtm.start`. Otherwise, `mpims` are emulated using the groups API.

The `ims` property is a list of [IM objects](/types/im), one for every direct message channel visible to the authenticated user.

The `bots` property gives details of the integrations set up on this team.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `migration_in_progress` | Workspace is being migrated between servers. See [the `team_migration_started` event documentation](/events/team_migration_started) for details. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. |
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
| `member_list_truncated` | A `members` array included in a channel object was truncated at 1500 results. Use `conversations.members` to retrieve all results. |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

