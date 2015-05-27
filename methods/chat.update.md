This method updates a message in a channel.

## Arguments

This method has the URL `https://slack.com/api/chat.update` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `post`) |
| `ts` | `1405894322.002768` | Required | Timestamp of the message to be updated. |
| `channel` | `C1234567890` | Required | Channel containing the message to be updated. |
| `text` | `Hello world` | Required | New text for the message, using the [default formatting rules](/docs/formatting). |

## Response

```
{
    "ok": true,
    "channel": "C024BE91L",
    "ts": "1401383885.000061",
    "text": "Updated Text"
}
```

The response includes the `text`, `channel` and `timestamp` properties of the updated message so clients can keep their local copies of the message in sync.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `message_not_found` | No message exists with the requested timestamp. |
| `cant_update_message` | Authenticated user does not have permission to update this message. |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `edit_window_closed` | The message cannot be edited due to the team message edit settings |
| `msg_too_long` | Message text is too long |
| `no_text` | No message text provided |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

