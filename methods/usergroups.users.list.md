This method returns a list of all users within a user group.

## Arguments

This method has the URL `https://slack.com/api/usergroups.users.list` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `usergroups:read`) |
| `usergroup` | `S0604QSJC` | Required | The encoded ID of the user group to update. |
| `include_disabled` | `1` | Optional | Allow results that involve disabled user groups. |

## Response

```
{
    "ok": true,
    "users": [
        "U060R4BJ4"
    ]
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
| `user_is_restricted` | This method cannot be called by a restricted user or single channel guest. |
| `user_is_ultra_restricted` | This method cannot be called by a single channel guest. |

