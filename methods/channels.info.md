This method returns information about a team channel.

## Arguments

This method has the URL `https://slack.com/api/channels.info` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `channels:read`) |
| `channel` | `C1234567890` | Required | Channel to get info on |

## Response

Returns a [channel object](/types/channel):

```
{
    "ok": true,
    "channel": {
        "id": "C024BE91L",
        "name": "fun",

        "created": 1360782804,
        "creator": "U024BE7LH",

        "is_archived": false,
        "is_general": false,
        "is_member": true,

        "members": […],

        "topic": { … },
        "purpose": { … },

        "last_read": "1401383885.000061",
        "latest": { … },
        "unread_count": 0,
        "unread_count_display": 0
    }
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

