This method removes a reaction (emoji) from an item (file, file comment, channel message, group message, or direct message). One of `file`, `file_comment`, or the combination of `channel` and `timestamp` must be specified.

## Arguments

This method has the URL `https://slack.com/api/reactions.remove` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `post`) |
| `name` | `thumbsup` | Required | Reaction (emoji) name. |
| `file` | `F1234567890` | Optional | File to remove reaction from. |
| `file_comment` | `Fc1234567890` | Optional | File comment to remove reaction from. |
| `channel` | `C1234567890` | Optional | Channel where the message to remove reaction from was posted. |
| `timestamp` | `1234567890.123456` | Optional | Timestamp of the message to remove reaction from. |

## Response

```
{
    "ok": true
}
```

After making this call, the reaction is removed and [a `reaction_removed` event](/events/reaction_removed) is broadcast through the [RTM API](/rtm) for the calling user.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `bad_timestamp` | Value passed for `timestamp` was invalid. |
| `file_not_found` | File specified by `file` does not exist. |
| `file_comment_not_found` | File comment specified by `file_comment` does not exist. |
| `message_not_found` | Message specified by `channel` and `timestamp` does not exist. |
| `no_item_specified` | `file`, `file_comment`, or combination of `channel` and `timestamp` was not specified. |
| `invalid_name` | Value passed for `name` was invalid. |
| `no_reaction` | The specified item does not have the user/reaction combination. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

