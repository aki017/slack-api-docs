This method takes an existing private channel and performs the following steps:

- Renames the existing private channel (from "example" to "example-archived").
- Archives the existing private channel.
- Creates a new private channel with the name of the existing private channel.
- Adds all members of the existing private channel to the new private channel.

This is useful when inviting a new member to an existing private channel while hiding all previous chat history from them. In this scenario you can call`groups.createChild` followed by `groups.invite`.

The new private channel will have a special `parent_group` property pointing to the original archived private channel. This will only be returned for members of both private channels, so will not be visible to any newly invited members.

## Arguments

This method has the URL `https://slack.com/api/groups.createChild` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `groups:write`) |
| `channel` | `G1234567890` | Required | Private channel to clone and archive. |

## Response

If successful, the command returns the new [group object](/types/group):

```
{
    "ok": true,
    "group": {
        "id": "G024BE91L",
        "name": "secretplans",
        "is_group": "true",
        "created": 1360782804,
        "creator": "U024BE7LH",
        "is_archived": false,
        "members": [
            "U024BE7LH"
        ],
        …
    }
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `already_archived` | An archived group cannot be cloned |
| `restricted_action` | A team preference prevents the authenticated user from creating groups. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `user_is_ultra_restricted` | This method cannot be called by a single channel guest. |

