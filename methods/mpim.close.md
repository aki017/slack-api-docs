This method closes a multiparty direct message channel.

## Arguments

This method has the URL `https://slack.com/api/mpim.close` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `post`) |
| `channel` | &nbsp; | Required | MPIM to close. |

## Response

```
{
    "ok": true
}
```

If the mpim was already closed the response will include `no_op` and`already_closed` properties:

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
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |

