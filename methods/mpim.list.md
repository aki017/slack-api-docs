This method returns a list of all multiparty direct message channels that the user has.

## Arguments

This method has the URL `https://slack.com/api/mpim.list` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `mpim:read`) |

## Response

Returns a list of [group objects](/types/group):

```
{
    "ok": true,
    "groups": [
        {
            "id": "G024BE91L",
            "name": "dm-messaging-user-1",
            "created": 1360782804,
            "creator": "U024BE7LH",
            "is_archived": false,
            "is_mpim": true
            "members": [
                "U024BE7LH",
                "U1234567890",
                "U2345678901",
                "U3456789012"
            ],
            "topic": {
                "value": "Group messaging.",
                "creator": "U024BE7LH",
                "last_set": 1360782804
            },
            "purpose": {
                "value": "Group messaging with: @user @user_a @user_b @user_c",
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

