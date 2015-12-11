This method is used to create a channel.

## Arguments

This method has the URL `https://slack.com/api/channels.create` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `channels:write`) |
| `name` | `mychannel` | Required | Name of channel to create |

## Naming

Channel names can only contain lowercase letters, numbers, hyphens, and underscores, and must be 21 characters or less. We will validate the submitted channel name and modify it to meet the above criteria. When calling this method, we recommend storing the channel's `name` value that is returned in the response.

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
        "last_read": "0000000000.000000",
        "latest": null,
        "unread_count": 0,
        "unread_count_display": 0,
        "members": […],
        "topic": { … },
        "purpose": { … }
    }
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `name_taken` | A channel cannot be created with the given name. |
| `restricted_action` | A team preference prevents the authenticated user from creating channels. |
| `no_channel` | Value passed for `name` was empty. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `user_is_restricted` | This method cannot be called by a restricted user or single channel guest. |

