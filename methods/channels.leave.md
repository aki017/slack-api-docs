This method is used to leave a channel.

## Arguments

This method has the URL `https://slack.com/api/channels.leave` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `channels:write`) |
| `channel` | `C1234567890` | Required | Channel to leave |

## Response

```
{
    "ok": true
}
```

This method will not return an error if the user was not in the channel before it was called. Instead the response will include a `not_in_channel` property:

```
{
    "ok": true,
    "not_in_channel": true
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `is_archived` | Channel has been archived. |
| `cant_leave_general` | Authenticated user cannot leave the general channel |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `user_is_restricted` | This method cannot be called by a restricted user or single channel guest. |

