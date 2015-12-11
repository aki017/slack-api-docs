This method closes a direct message channel.

## Arguments

This method has the URL `https://slack.com/api/im.close` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `im:write`) |
| `channel` | `D1234567890` | Required | Direct message channel to close. |

## Response

```
{
    "ok": true
}
```

If the channel was already closed the response will include a `no_op`property:

```
{
    "ok": true,
    "no_op": true,
    "already_closed": true
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `user_does_not_own_channel` | Calling user does not own this DM channel. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

