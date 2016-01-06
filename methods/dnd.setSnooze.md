Adjusts the snooze duration for a user's Do Not Disturb settings. If a snooze session is not already active for the user, invoking this method will begin one for the specified duration.

## Arguments

This method has the URL `https://slack.com/api/dnd.setSnooze` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `dnd:write`) |
| `num_minutes` | `60` | Required | Number of minutes, from now, to snooze until. |

## Response

```
{
    "ok": true,
    "snooze_enabled": true,
    "snooze_endtime": 1450373897,
    "snooze_remaining": 60,
}
```

The `snooze_remaining` field is expressed in seconds. If your request presents a `num_minutes` value of `1`, the response's `snooze_remaining` will be `60`.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `missing_duration` | No value provided for `num_minutes` |
| `snooze_failed` | There was a problem setting the user's Do Not Disturb status |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |

