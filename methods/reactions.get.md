This method returns a list of all reactions for a single item (file, file comment, channel message, group message, or direct message).

## Arguments

This method has the URL `https://slack.com/api/reactions.get` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `read`) |
| `file` | `F1234567890` | Optional | File to get reactions for. |
| `file_comment` | `Fc1234567890` | Optional | File comment to get reactions for. |
| `channel` | `C1234567890` | Optional | Channel where the message to get reactions for was posted. |
| `timestamp` | `1234567890.123456` | Optional | Timestamp of the message to get reactions for. |
| `full` | &nbsp; | Optional | If true always return the complete reaction list. |

## Response

The response contains the item with reactions.

```
{
    "type": "message",
    "channel": "C2147483705",
    "message": {
        ...
        "reactions": [
            {
                "name": "astonished",
                "count": 3,
                "users": ["U1", "U2", "U3"]
            },
            {
                "name": "clock1"
                "count": 2,
                "users": ["U1", "U2", "U3"]
            }
        ]
    },
},
```

Different item types can be reacted to. The item returned has a `type` property, the other properties depend on the type of item. The possible types are:

- **`message`** : the item will have a `message` property containing a [message object](/docs/messages) and a `channel` property containing the channel ID for the message.
- **`file`** : this item will have a `file` property containing a [file object](/types/file).
- **`file_comment`** : the item will have a `file` property containing the [file object](/types/file) and a `comment` property containing the file comment.

The users array in the `reactions` property might not always contain all users that have reacted (we limit it to X users, and X might change), however `count` will always represent the count of all users who made that reaction (i.e. it may be greater than `users.length`). If the authenticated user has a given reaction then they are guaranteed to appear in the `users` array, regardless of whether `count` is greater than `users.length` or not. If the complete list of users is required they can still be obtained by specifying the `full` argument.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `bad_timestamp` | Value passed for `timestamp` was invalid. |
| `file_not_found` | File specified by `file` does not exist. |
| `file_comment_not_found` | File comment specified by `file_comment` does not exist. |
| `message_not_found` | Message specified by `channel` and `timestamp` does not exist. |
| `no_item_specified` | `file`, `file_comment`, or combination of `channel` and `timestamp` was not specified. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
