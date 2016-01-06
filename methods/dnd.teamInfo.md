Provides information about the current Do Not Disturb settings for users of a Slack team.

## Arguments

This method has the URL `https://slack.com/api/dnd.teamInfo` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `dnd:read`) |
| `users` | `U1234,U4567` | Optional | Comma-separated list of users to fetch Do Not Disturb status for |

## Response

```
{
    "ok": true,
    "users": {
        "U023BECGF": {
            "dnd_enabled": true,
            "next_dnd_start_ts": 1450387800,
            "next_dnd_end_ts": 1450423800
        },
        "U058CJVAA": {
            "dnd_enabled": false,
            "next_dnd_start_ts": 1,
            "next_dnd_end_ts": 1
        }
    }
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

