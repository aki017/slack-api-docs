This method renames a private group.

## Arguments

This method has the URL `https://slack.com/api/groups.rename` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `groups:write`) |
| `channel` | `C1234567890` | Required | Group to rename |
| `name` | &nbsp; | Required | New name for group. |

## Naming

Private group names can only contain lowercase letters, numbers, hyphens, and underscores, and must be 21 characters or less. We will validate the submitted channel name and modify it to meet the above criteria. When calling this method, we recommend storing the group's `name` value that is returned in the response.

## Response

```
{
    "ok": true,
    "channel": {
        "id": "C024BE91L",
        "is_group": true,
        "name": "new_name",
        "created": 1360782804
    }
}
```

Returns the channel ID, name and date created (as a unix timestamp).

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `invalid_name` | New name is invalid |
| `name_taken` | New channel name is taken |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `user_is_restricted` | This method cannot be called by a restricted user or single channel guest. |

