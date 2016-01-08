Delete an existing comment on a file. Only the original author of the comment or a team administrator may delete a file comment.

## Arguments

This method has the URL `https://slack.com/api/files.comments.delete` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `files:write:user`) |
| `file` | `F1234567890` | Required | File to delete a comment from. |
| `id` | `Fc1234567890` | Required | The comment to delete. |

## Response

```
{
    "ok": true
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `file_not_found` | The requested file could not be found. |
| `file_deleted` | The requested file was previously deleted. |
| `cant_delete` | The requested comment could not be deleted. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |

