This method deletes a message from a channel.

## Arguments

This method has the URL `https://slack.com/api/chat.delete` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `chat:write:user`) |
| `ts` | `1405894322.002768` | Required | Timestamp of the message to be deleted. |
| `channel` | `C1234567890` | Required | Channel containing the message to be deleted. |

## Response

```
{
    "ok": true,
    "channel": "C024BE91L",
    "ts": "1401383885.000061"
}
```

The response includes the `channel` and `timestamp` properties of the deleted message.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `message_not_found` | No message exists with the requested timestamp. |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `cant_delete_message` | Authenticated user does not have permission to delete this message. |
| `compliance_exports_prevent_deletion` | Compliance exports are on, messages can not be deleted |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

