This method adds a star to an item (message, file, file comment, channel, private group, or DM) on behalf of the authenticated user. One of `file`, `file_comment`, `channel`, or the combination of `channel` and `timestamp` must be specified.

## Arguments

This method has the URL `https://slack.com/api/stars.add` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `post`) |
| `file` | `F1234567890` | Optional | File to add star to. |
| `file_comment` | `Fc1234567890` | Optional | File comment to add star to. |
| `channel` | `C1234567890` | Optional | Channel to add star to, or channel where the message to add star to was posted (used with `timestamp`). |
| `timestamp` | `1234567890.123456` | Optional | Timestamp of the message to add star to. |

## Response

```
{
    "ok": true
}
```

After making this call, the item will be starred and [a `star_added` event](/events/star_added) is broadcast through the [RTM API](/rtm) for the calling user.

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
| `already_starred` | The specified item has already been starred by the authenticated user. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |

