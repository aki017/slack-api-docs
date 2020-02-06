Starts a Real Time Messaging session.

## Facts

| Method URL: | `https://slack.com/api/rtm.connect` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 1](/docs/rate-limits#tier_t1) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [user](/docs/token-types#user) | _No scope required_ |
| [classic&nbsp;bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |

 |

* * *

This method begins a Real Time Messaging API session and reserves your application a specific URL with which to connect via websocket.

Unlike [`rtm.start`](/methods/rtm.start), this method is focused only on connecting to the RTM API.

Use this method in conjunction with other Web API methods like [`channels.list`](/methods/channels.list), [`users.list`](/methods/users.list), and [`team.info`](/methods/team.info) to build a full picture of the team or workspace you're connecting on behalf of.

Please consult the [RTM API documentation](/rtm) for full details on using the RTM API.

[New Slack apps](/authentication/basics) may not use any Real Time Messaging API method. Create a [classic app](/rtm#classic) and [use the V1 Oauth flow](/docs/oauth) to use RTM.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `batch_presence_aware` | `1` | Optional, default= | Batch presence deliveries via subscription. Enabling changes the shape of `presence_change` events. See [batch presence](/docs/presence-and-status#batching). |
| `presence_sub` | `true` | Optional, default=1 | Only deliver presence events when requested by subscription. See [presence subscriptions](/docs/presence-and-status#subscriptions). |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

This method returns a WebSocket Message Server URL and limited information about the team:

Typical success response

```
{
    "ok": true,
    "self": {
        "id": "U4X318ZMZ",
        "name": "robotoverlord"
    },
    "team": {
        "domain": "slackdemo",
        "id": "T2U81E2FP",
        "name": "SlackDemo"
    },
    "url": "wss://..."
}
```

Typical error response

```
{
    "ok": false,
    "error": "invalid_auth"
}
```

The `url` property contains a WebSocket Message Server URL. Connecting to this URL will initiate a Real Time Messaging session. These URLs are only valid for 30 seconds, so connect quickly!

The `self` property contains a small amount of information concerning the connecting user — an `id` and their `name`.

The `team` attribute also houses brief information about the team, including its `id`, `name`, `domain`, and if it's part of an [Enterprise Grid](/enterprise-grid), the corresponding `enteprise_id`.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `migration_in_progress` | Team is being migrated between servers. See [the `team_migration_started` event documentation](/events/team_migration_started) for details. |
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

