This method un-pins an item (file, file comment, channel message, or group message) from a channel. The `channel` argument is required and one of `file`, `file_comment`, or `timestamp` must also be specified.

## Arguments

This method has the URL `https://slack.com/api/pins.remove` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `post`) |
| `channel` | `C1234567890` | Required | Channel where the item is pinned to. |
| `file` | `F1234567890` | Optional | File to un-pin. |
| `file_comment` | `Fc1234567890` | Optional | File comment to un-pin. |
| `timestamp` | `1234567890.123456` | Optional | Timestamp of the message to un-pin. |

## Response

```
{
    "ok": true
}
```

After making this call the pin is removed from the database and [a `pin_removed` event](/events/pin_removed) is broadcast via the [RTM API](/rtm).

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `bad_timestamp` | Value passed for `timestamp` was invalid. |
| `file_not_found` | File specified by `file` does not exist. |
| `file_comment_not_found` | File comment specified by `file_comment` does not exist. |
| `message_not_found` | Message specified by `channel` and `timestamp` does not exist. |
| `no_item_specified` | One of `file`, `file_comment`, or `timestamp` was not specified. |
| `not_pinned` | The specified item is not pinned to the channel. |
| `permission_denied` | The user does not have permission to remove pins from the channel. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |

