This method opens a direct message channel with another member of your Slack team.

## Arguments

This method has the URL `https://slack.com/api/im.open` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `post`) |
| `user` | `U1234567890` | Required | User to open a direct message channel with. |

## Response

```
{
    "ok": true,
    "channel": {
        "id": "D024BFF1M"
    }
}
```

If the channel was already open the response will include `no_op` and`already_open` properties:

```
{
    "ok": true,
    "no_op": true,
    "already_open": true,
    "channel": {
        "id": "D024BFF1M"
    }
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `user_not_found` | Value passed for `user` was invalid. |
| `user_not_visible` | The calling user is restricted from seeing the requested user. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

