This method lists the items pinned to a channel.

## Arguments

This method has the URL `https://slack.com/api/pins.list` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `pins:read`) |
| `channel` | `C1234567890` | Required | Channel to get pinned items for. |

## Response

The response contains a list of pinned items in a channel.

```
{
    "ok": true,
    "items": [
        {
            "type": "message",
            "channel": "C2147483705",
            "message": {...},
        },
        {
            "type": "file",
            "file": { ... },
        }
        {
            "type": "file_comment",
            "file": { ... },
            "comment": { ... },
        }
    ]
}
```

Different item types can be pinned. Every item in the list has a `type` property, and the other properties depend on the type of item. The possible types are:

- **`message`** : the item will have a `message` property containing a [message object](/docs/messages) and a `channel` property containing the channel ID for the message.
- **`file`** : this item will have a `file` property containing a [file object](/types/file).
- **`file_comment`** : the item will have a `file` property containing the [file object](/types/file) and a `comment` property containing the file comment.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |

