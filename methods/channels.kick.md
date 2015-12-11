This method allows a user to remove another member from a team channel.

## Arguments

This method has the URL `https://slack.com/api/channels.kick` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `channels:write`) |
| `channel` | `C1234567890` | Required | Channel to remove user from. |
| `user` | `U1234567890` | Required | User to remove from channel. |

## Response

```
{
    "ok": true
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `user_not_found` | Value passed for `user` was invalid. |
| `cant_kick_self` | Authenticated user can't kick themselves from a channel. |
| `not_in_channel` | User was not in the channel. |
| `cant_kick_from_general` | User cannot be removed from #general. |
| `cant_kick_from_last_channel` | User cannot be removed from the last channel they're in. |
| `restricted_action` | A team preference prevents the authenticated user from kicking. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `user_is_restricted` | This method cannot be called by a restricted user or single channel guest. |

