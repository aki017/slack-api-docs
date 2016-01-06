This method deletes a file from your team.

## Arguments

This method has the URL `https://slack.com/api/files.delete` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `files:write:user`) |
| `file` | `F1234567890` | Required | ID of file to delete. |

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
| `file_not_found` | The file does not exist, or is not visible to the calling user. |
| `file_deleted` | The file has already been deleted. |
| `cant_delete_file` | Authenticated user does not have permission to delete this file. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

