Edit an existing comment on a file. Only the user who created a comment may make edits. Teams may configure a limited time window during which file comment edits are allowed.

## Arguments

This method has the URL `https://slack.com/api/files.comments.edit` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `files:write:user`) |
| `file` | `F1234567890` | Required | File containing the comment to edit. |
| `id` | `Fc1234567890` | Required | The comment to edit. |
| `comment` | `Everyone should take a moment to read this file, seriously.` | Required | Text of the comment to edit. |

## Response

If successful, the response will include a file comment object.

```
{
    "ok": true,
    "comment": {
        "id": "Fc1234567890",
        "created": 1356032811,
        "timestamp": 1356032811,
        "user": "U1234567890",
        "comment": "Everyone should take a moment to read this file, seriously."
    }
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `file_not_found` | The requested file could not be found. |
| `file_deleted` | The requested file was previously deleted. |
| `no_comment` | The `comment` field was empty. |
| `edit_window_closed` | The timeframe for editing the comment has expired. |
| `cant_edit` | The requested file could not be found. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |

