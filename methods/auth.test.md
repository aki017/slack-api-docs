This method checks authentication and tells you who you are.

## Arguments

This method has the URL `https://slack.com/api/auth.test` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `identify`) |

## Response

```
{
    "ok": true,
    "url": "https:\/\/myteam.slack.com\/",
    "team": "My Team",
    "user": "cal",
    "team_id": "T12345",
    "user_id": "U12345"
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

