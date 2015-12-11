This method pins an item (file, file comment, channel message, or group message) to a particular channel. The `channel` argument is required and one of `file`, `file_comment`, or `timestamp` must also be specified.

## Arguments

This method has the URL `https://slack.com/api/pins.add` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `pins:write`) |
| `channel` | `C1234567890` | Required | Channel to pin the item in. |
| `file` | `F1234567890` | Optional | File to pin. |
| `file_comment` | `Fc1234567890` | Optional | File comment to pin. |
| `timestamp` | `1234567890.123456` | Optional | Timestamp of the message to pin. |

## Response

```
{
    "ok": true
}
```

After making this call the pin is saved to the database and [a `pin_added` event](/events/pin_added) is broadcast via the [RTM API](/rtm).

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `bad_timestamp` | Value passed for `timestamp` was invalid. |
| `file_not_found` | File specified by `file` does not exist. |
| `file_comment_not_found` | File comment specified by `file_comment` does not exist. |
| `message_not_found` | Message specified by `channel` and `timestamp` does not exist. |
| `channel_not_found` | The `channel` argument was not specified or was invalid |
| `no_item_specified` | One of `file`, `file_comment`, or `timestamp` was not specified. |
| `already_pinned` | The specified item is already pinned to the channel. |
| `permission_denied` | The user does not have permission to add pins to the channel. |
| `file_not_shared` | File specified by `file` is not public nor shared to the channel. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

