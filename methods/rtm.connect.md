This method begins a Real Time Messaging API session and reserves your application a specific URL with which to connect via websocket.

Unlike [`rtm.start`](/methods/rtm.start), this method is focused only on connecting to the RTM API.

Use this method in conjunction with other Web API methods like [`channels.list`](/methods/channels.list), [`users.list`](/methods/users.list), and [`team.info`](/methods/team.info) to build a full picture of the team or workspace you're connecting on behalf of.

Please consult the [RTM API documentation](/rtm) for full details on using the RTM API.

## Arguments

This method has the URL `https://slack.com/api/rtm.connect` and follows the [Slack Web API calling conventions](/web#basics). <aside class="small">Present these parameters as part of an <code>application/x-www-form-urlencoded</code> querystring or POST body. <code>application/json</code> is not currently accepted.</aside>

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token.  
Requires scope: `client` |
| `presence_sub` | `true` | Optional, default=false | Only deliver presence events when requested by subscription. See [presence subscriptions](/docs/presence-and-status#subscriptions). |
| `batch_presence_aware` | `1` | Optional, default=false | Group presence change notices as `presence_change_batch` events when possible. See [batching](/docs/presence-and-status#batching). |

## Response

This method returns a WebSocket Message Server URL and limited information about the team:

```
{
    "ok": true,
    "url": "wss:\/\/ms9.slack-msgs.com\/websocket\/2I5yBpcvk",
    "team": {
        "id": "T654321",
        "name": "Librarian Society of Soledad",
        "domain": "libsocos",
        "enterprise_id": "E234567",
        "enterprise_name": "Intercontinental Librarian Society"
    },
    "self": {
        "id": "W123456",
        "name": "brautigan"
    }
}
```

The `url` property contains a WebSocket Message Server URL. Connecting to this URL will initiate a Real Time Messaging session. These URLs are only valid for 30 seconds, so connect quickly!

The `self` property contains a small amount of information concerning the connecting user &mdash; an `id` and their `name`.

The `team` attribute also houses brief information about the team, including its `id`, `name`, `domain`, and if it's part of an [Enterprise Grid](/enterprise-grid), the corresponding `enteprise_id`.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of common decency. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
| `invalid_array_arg` | The method was passed a PHP-style array argument (e.g. with a name like `foo[7]`). These are never valid with the Slack API. |
| `invalid_charset` | The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`. |
| `invalid_form_data` | The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid. |
| `invalid_post_type` | The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`. |
| `missing_post_type` | The method was called via a `POST` request and included a data payload, but the request did not include a `Content-Type` header. |
| `team_added_to_org` | The team associated with your request is currently undergoing migration to an Enterprise Organization. Web API and other platform operations will be intermittently unavailable until the transition is complete. |
| `request_timeout` | The method was called via a `POST` request, but the `POST` data was either missing or truncated. |

## Warnings

This table lists the expected warnings that this method will return. However, other warnings can be returned in the case where the service is experiencing unexpected trouble.

| Warning | Description |
| --- | --- |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

