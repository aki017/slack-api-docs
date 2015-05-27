This method archives a private group.

## Arguments

This method has the URL `https://slack.com/api/groups.archive` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `post`) |
| `channel` | `G1234567890` | Required | Private group to archive |

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
| `channel_not_found` | Value passed for `channel` was invalid. |
| `already_archived` | Group has already been archived. |
| `group_contains_others` | Restricted accounts cannot archive groups containing others. |
| `last_ra_channel` | You cannot archive the last channel for a restricted account. |
| `restricted_action` | A team preference prevents the authenticated user from archiving. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `user_is_ultra_restricted` | This method cannot be called by a single channel guest. |

