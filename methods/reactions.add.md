This method adds a reaction (emoji) to an item (file, file comment, channel message, group message, or direct message). One of `file`, `file_comment`, or the combination of `channel` and `timestamp` must be specified.

## Arguments

This method has the URL `https://slack.com/api/reactions.add` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `reactions:write`) |
| `name` | `thumbsup` | Required | Reaction (emoji) name. |
| `file` | `F1234567890` | Optional | File to add reaction to. |
| `file_comment` | `Fc1234567890` | Optional | File comment to add reaction to. |
| `channel` | `C1234567890` | Optional | Channel where the message to add reaction to was posted. |
| `timestamp` | `1234567890.123456` | Optional | Timestamp of the message to add reaction to. |

## Response

```
{
    "ok": true
}
```

After making this call, the reaction is saved and [a `reaction_added` event](/events/reaction_added) is broadcast through the [RTM API](/rtm) for the calling user.

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
| `already_reacted` | The specified item already has the user/reaction combination. |
| `too_many_emoji` | The limit for distinct reactions (i.e emoji) on the item has been reached. |
| `too_many_reactions` | The limit for reactions a person may add to the item has been reached. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

