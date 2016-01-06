This method is used to invite a user to a private channel. The calling user must be a member of the private channel.

To invite a new member to a private channel without giving them access to the archives of the private channel, call the [`groups.createChild` method](/methods/groups.createChild)before inviting.

## Arguments

This method has the URL `https://slack.com/api/groups.invite` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `groups:write`) |
| `channel` | `G1234567890` | Required | Private channel to invite user to. |
| `user` | `U1234567890` | Required | User to invite. |

## Response

If successful, the API response includes a [group object](/types/group):

```
{
    "ok": true,
    "group": {
        …
    },
}
```

If the invited user is already in the private channel, the response will include an`already_in_group` property:

```
{
    "ok": true,
    "already_in_group": true,
    "group": {
        …
    },
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `user_not_found` | Value passed for `user` was invalid. |
| `cant_invite_self` | Authenticated user cannot invite themselves to a group. |
| `is_archived` | Group has been archived. |
| `cant_invite` | User cannot be invited to this group. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `user_is_ultra_restricted` | This method cannot be called by a single channel guest. |

