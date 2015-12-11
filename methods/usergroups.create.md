This method is used to create a user group.

## Arguments

This method has the URL `https://slack.com/api/usergroups.create` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `usergroups:write`) |
| `name` | `My Test Team` | Required | A name for the user group. Must be unique among user groups. |
| `handle` | &nbsp; | Optional | A mention handle. Must be unique among channels, users and user groups. |
| `description` | &nbsp; | Optional | A short description of the user group. |
| `channels` | &nbsp; | Optional | A comma separated string of encoded channel IDs for which the user group uses as a default. |
| `include_count` | `1` | Optional | Include the number of users in each user group. |

## Response

If successful, the command returns a [usergroup object](/types/usergroup), including preferences:

```
{
    "ok": true,
    "usergroup": {
        "id": "S0615G0KT",
        "team_id": "T060RNRCH",
        "is_usergroup": true,
        "name": "Marketing Team",
        "description": "Marketing gurus, PR experts and product advocates.",
        "handle": "marketing-team",
        "is_external": false,
        "date_create": 1446746793,
        "date_update": 1446746793,
        "date_delete": 0,
        "auto_type": null,
        "created_by": "U060RNRCZ",
        "updated_by": "U060RNRCZ",
        "deleted_by": null,
        "prefs": {
            "channels": [

            ],
            "groups": [

            ]
        },
        "user_count": "0"
    }
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `user_is_restricted` | This method cannot be called by a restricted user or single channel guest. |
| `user_is_ultra_restricted` | This method cannot be called by a single channel guest. |

