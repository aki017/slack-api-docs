This method lists the items starred by a user.

## Arguments

This method has the URL `https://slack.com/api/stars.list` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `read`) |
| `user` | `U1234567890` | Optional | Show stars by this user. Defaults to the authed user. |
| `count` | `100` | Optional, default=100 | Number of items to return per page. |
| `page` | `2` | Optional, default=1 | Page number of results to return. |

## Response

The response contains a list of starred items followed by pagination information.

```
{
    "ok": true,
    "items": [
        {
            "type": "message",
            "channel": "C2147483705",
            "message": {...}
        },
        {
            "type": "file",
            "file": { ... }
        }
        {
            "type": "file_comment",
            "file": { ... },
            "comment": { ... }
        }
        {
            "type": "channel",
            "channel": "C2147483705"
        },

    ],
    "paging": {
        "count": 100,
        "total": 4,
        "page": 1,
        "pages": 1
    }
}
```

Different item types can be starred. Every item in the list has a `type` property, the other property depend on the type of item. The possible types are:

- **`message`** : the item will have a `message` property containing a [message object](/docs/messages)
- **`file`** : this item will have a `file` property containing a [file object](/types/file).
- **`file_comment`** : the item will have a `file` property containing the [file object](/types/file) and a `comment` property containing the file comment.
- **`channel`** : the item will have a `channel` property containing the channel ID.
- **`im`** : the item will have a `channel` property containing the channel ID for this direct message.
- **`group`** : the item will have a `group` property containing the channel ID for the private group.

The paging information contains the `count` of files returned, the `total`number of items starred, the `page` of results returned in this response and the total number of `pages` available.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `user_not_found` | Value passed for `user` was invalid |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |

