Ends the current user's snooze mode immediately.

## Arguments

This method has the URL `https://slack.com/api/dnd.endSnooze` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `dnd:write`) |

## Response

```
{
    "ok": true,
    "dnd_enabled": true,
    "next_dnd_start_ts": 1450418400,
    "next_dnd_end_ts": 1450454400,
    "snooze_enabled": false
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `snooze_not_active` | Snooze is not active for this user and cannot be ended |
| `snooze_end_failed` | There was a problem setting the user's Do Not Disturb status |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |

