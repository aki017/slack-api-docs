This method returns a list of private channels in the team that the caller is in and archived groups that the caller was in. The list of (non-deactivated) members in each private channel is also returned.

## Arguments

This method has the URL `https://slack.com/api/groups.list` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `groups:read`) |
| `exclude_archived` | `1` | Optional, default=0 | Don't return archived private channels. |

## Response

Returns a list of [group objects](/types/group) (also known as "private channel objects"):

```
{
    "ok": true,
    "groups": [
        {
            "id": "G024BE91L",
            "name": "secretplans",
            "created": 1360782804,
            "creator": "U024BE7LH",
            "is_archived": false,
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
        },
        ....
    ]
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

