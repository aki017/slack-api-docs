Provides information about a user's current Do Not Disturb settings.

## Arguments

This method has the URL `https://slack.com/api/dnd.info` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `dnd:read`) |
| `user` | `U1234` | Optional | User to fetch status for (defaults to current user) |

## Response

```
{
    "ok": true,
    "dnd_enabled": true,
    "next_dnd_start_ts": 1450416600,
    "next_dnd_end_ts": 1450452600,
    "snooze_enabled": true,
    "snooze_endtime": 1450416600,
    "snooze_remaining": 1196
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |

