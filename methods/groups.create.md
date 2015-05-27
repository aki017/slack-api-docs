This method creates a private group.

## Arguments

This method has the URL `https://slack.com/api/groups.create` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `post`) |
| `name` | &nbsp; | Required | Name of group to create |

## Response

If successful, the command returns a [group object](/types/group), including state information:

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
        "is_open": true,
        "last_read": "0000000000.000000",
        "latest": null,
        "unread_count": 0,
        "unread_count_display": 0,
        "members": [
            "U024BE7LH"
        ],
        "topic": {
            "value": "Secret plans on hold",
            "creator": "U024BE7LV",
            "last_set": 1369677212
        },
        "purpose": {
            "value": "Discuss secret plans that no-one else should know",
            "creator": "U024BE7LH",
            "last_set": 1360782804
        }
    }
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `no_channel` | No group name was passed. |
| `restricted_action` | A team preference prevents the authenticated user from creating groups. |
| `name_taken` | A group cannot be created with the given name. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `user_is_ultra_restricted` | This method cannot be called by a single channel guest. |

