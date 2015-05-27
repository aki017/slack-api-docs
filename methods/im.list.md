This method returns a list of all im channels that the user has.

## Arguments

This method has the URL `https://slack.com/api/im.list` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `read`) |

## Response

Returns a list of [IM objects](/types/im):

```
{
    "ok": true,
    "ims": [
        {
           "id": "D024BFF1M",
           "is_im": true,
           "user": "USLACKBOT",
           "created": 1372105335,
           "is_user_deleted": false
        },
        {
           "id": "D024BE7RE",
           "is_im": true,
           "user": "U024BE7LH",
           "created": 1356250715,
           "is_user_deleted": false
        },
        …
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

