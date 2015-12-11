This method updates the list of users that belong to a user group. This method replaces all users in a user group with the list of users provided in the `users` parameter.

## Arguments

This method has the URL `https://slack.com/api/usergroups.users.update` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `usergroups:write`) |
| `usergroup` | `S0604QSJC` | Required | The encoded ID of the user group to update. |
| `users` | `U060R4BJ4,U060RNRCZ` | Required | A comma separated string of encoded user IDs that represent the entire list of users for the user group. |
| `include_count` | `1` | Optional | Include the number of users in the user group. |

## Response

```
{
    "ok": true,
    "usergroup": {
        "id": "S0616NG6M",
        "team_id": "T060R4BHN",
        "is_usergroup": true,
        "name": "Marketing Team",
        "description": "Marketing gurus, PR experts and product advocates.",
        "handle": "marketing-team",
        "is_external": false,
        "date_create": 1447096577,
        "date_update": 1447102109,
        "date_delete": 0,
        "auto_type": null,
        "created_by": "U060R4BJ4",
        "updated_by": "U060R4BJ4",
        "deleted_by": null,
        "prefs": {
            "channels": [

            ],
            "groups": [

            ]
        },
        "users": [
            "U060R4BJ4",
            "U060RNRCZ"
        ],
        "user_count": 1
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

