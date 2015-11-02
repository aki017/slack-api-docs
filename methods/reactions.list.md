This method returns a list of all items (file, file comment, channel message, group message, or direct message) reacted to by a user.

## Arguments

This method has the URL `https://slack.com/api/reactions.list` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `read`) |
| `user` | `U1234567890` | Optional | Show reactions made by this user. Defaults to the authed user. |
| `full` | &nbsp; | Optional | If true always return the complete reaction list. |
| `count` | `100` | Optional, default=100 | Number of items to return per page. |
| `page` | `2` | Optional, default=1 | Page number of results to return. |

## Response

The response contains a list of items with reactions followed by pagination information.

```
{
    "ok": true,
    "items": [
        {
            "type": "message",
            "channel": "C2147483705",
            "message": {
                ...
                "reactions": [
                    {
                        "name": "astonished",
                        "count": 3,
                        "users": ["U1", "U2", "U3"]
                    },
                    {
                        "name": "clock1"
                        "count": 2,
                        "users": ["U1", "U2", "U3"]
                    }
                ]
            },
        },
        {
            "type": "file",
            "file": { ... },
            "reactions": [
                {
                    "name": "thumbsup",
                    "count": 1,
                    "users": ["U1"]
                }
            ]
        }
        {
            "type": "file_comment",
            "file": { ... },
            "comment": { ... },
            "reactions": [
                {
                    "name": "facepalm",
                    "count": 1034,
                    "users": ["U1", "U2", "U3", "U4", "U5"]
                }
            ]
        }
    ],
    "paging": {
        "count": 100,
        "total": 4,
        "page": 1,
        "pages": 1
    }
}
```

Different item types can be reacted to. Every item in the list has a `type` property, the other properties depend on the type of item. The possible types are:

- **`message`** : the item will have a `message` property containing a [message object](/docs/messages) and a `channel` property containing the channel ID for the message.
- **`file`** : this item will have a `file` property containing a [file object](/types/file).
- **`file_comment`** : the item will have a `file` property containing the [file object](/types/file) and a `comment` property containing the file comment.

The users array in the `reactions` property might not always contain all users that have reacted (we limit it to X users, and X might change), however `count` will always represent the count of all users who made that reaction (i.e. it may be greater than `users.length`). If the authenticated user has a given reaction then they are guaranteed to appear in the `users` array, regardless of whether `count` is greater than `users.length` or not. If the complete list of users is required they can still be obtained by specifying the `full` argument.

The paging information contains the `count` of items returned, the `total`number of items reacted to, the `page` of results returned in this response and the total number of `pages` available. Please note that the max `count` value is `1000` and the max `page` value is `100`.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `user_not_found` | Value passed for `user` was invalid. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

