This method is used to join a channel. If the channel does not exist, it is created.

## Arguments

This method has the URL `https://slack.com/api/channels.join` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `post`) |
| `name` | &nbsp; | Required | Name of channel to join |

## Response

If successful, the command returns a [channel object](/types/channel), including state information:

```
{
    "ok": true,
    "channel": {
        "id": "C024BE91L",
        "name": "fun",
        "created": 1360782804,
        "creator": "U024BE7LH",
        "is_archived": false,
        "is_member": true,
        "is_general": false,
        "last_read": "1401383885.000061",
        "latest": { … },
        "unread_count": 0,
        "unread_count_display": 0,
        "members": […],
        "topic": {
            "value": "Fun times",
            "creator": "U024BE7LV",
            "last_set": 1369677212
        },
        "purpose": {
            "value": "This channel is for fun",
            "creator": "U024BE7LH",
            "last_set": 1360782804
        }
    }
}
```

If you are already in the channel, the response is slightly different.`already_in_channel` will be true, and a limited `channel` object will be returned. This allows a client to see that the request to join `GeNERaL` is the same as the channel `#general` that the user is already in:

```
{
    "ok": true,
    "already_in_channel": true,
    "channel": {
        "id": "C024BE91L",
        "name": "fun",
        "created": 1360782804,
        "creator": "U024BE7LH",
        "is_archived": false,
        "is_general": false
    }
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `name_taken` | A channel cannot be created with the given name. |
| `restricted_action` | A team preference prevents the authenticated user from creating channels. |
| `no_channel` | Value passed for `name` was empty. |
| `is_archived` | Channel has been archived. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `user_is_restricted` | This method cannot be called by a restricted user or single channel guest. |

