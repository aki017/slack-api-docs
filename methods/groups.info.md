This method returns information about a private group.

## Arguments

This method has the URL `https://slack.com/api/groups.info` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `groups:read`) |
| `channel` | `C1234567890` | Required | Group to get info on |

## Response

Returns a [group object](/types/group):

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

        "topic": { … },
        "purpose": { … },

        "last_read": "1401383885.000061",
        "latest": { … }
        "unread_count": 0,
        "unread_count_display": 0

    },
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

