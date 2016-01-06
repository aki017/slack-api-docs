This method opens a private channel.

## Arguments

This method has the URL `https://slack.com/api/groups.open` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `groups:write`) |
| `channel` | `G1234567890` | Required | Private channel to open. |

## Response

```
{
    "ok": true
}
```

If the private channel was already open the response will include `no_op` and`already_open` properties:

```
{
    "ok": true,
    "no_op": true,
    "already_open": true
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

