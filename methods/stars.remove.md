This method removes a star from an item (message, file, file comment, channel, private group, or DM) on behalf of the authenticated user. One of `file`, `file_comment`, `channel`, or the combination of `channel` and `timestamp` must be specified.

## Arguments

This method has the URL `https://slack.com/api/stars.remove` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `stars:write`) |
| `file` | `F1234567890` | Optional | File to remove star from. |
| `file_comment` | `Fc1234567890` | Optional | File comment to remove star from. |
| `channel` | `C1234567890` | Optional | Channel to remove star from, or channel where the message to remove star from was posted (used with `timestamp`). |
| `timestamp` | `1234567890.123456` | Optional | Timestamp of the message to remove star from. |

## Response

```
{
    "ok": true
}
```

After making this call, the item will be unstarred and [a `star_removed` event](/events/star_removed) is broadcast through the [RTM API](/rtm) for the calling user.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `bad_timestamp` | Value passed for `timestamp` was invalid. |
| `message_not_found` | Message specified by `channel` and `timestamp` does not exist. |
| `file_not_found` | File specified by `file` does not exist. |
| `file_comment_not_found` | File comment specified by `file_comment` does not exist. |
| `channel_not_found` | Channel, private group, or DM specified by `channel` does not exist |
| `no_item_specified` | `file`, `file_comment`, `channel` and `timestamp` was not specified. |
| `not_starred` | The specified item is not currently starred by the authenticated user. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

