This method lets you set the calling user's manual presence. [Consult the presence documentation](/docs/presence) for more details.

## Arguments

This method has the URL `https://slack.com/api/users.setPresence` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `post`) |
| `presence` | `away` | Required | Either `auto` or `away` |

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
| `invalid_presence` | Value passed for `presence` was invalid. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

