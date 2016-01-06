This method returns information about a team member.

## Arguments

This method has the URL `https://slack.com/api/users.info` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `users:read`) |
| `user` | `U1234567890` | Required | User to get info on |

## Response

Returns a [user object](/types/user):

```
{
    "ok": true,
    "user": {
        "id": "U023BECGF",
        "name": "bobby",
        "deleted": false,
        "color": "9f69e7",
        "profile": {
            "first_name": "Bobby",
            "last_name": "Tables",
            "real_name": "Bobby Tables",
            "email": "bobby@slack.com",
            "skype": "my-skype-name",
            "phone": "+1 (123) 456 7890",
            "image_24": "https:\/\/...",
            "image_32": "https:\/\/...",
            "image_48": "https:\/\/...",
            "image_72": "https:\/\/...",
            "image_192": "https:\/\/..."
        },
        "is_admin": true,
        "is_owner": true,
        "has_2fa": true,
        "has_files": true
    }
}
```

## Profile

The profile hash contains as much information as the user has supplied in the default profile fields: `first_name`, `last_name`, `real_name`, `email`, `skype`, and the `image_*` fields. Only the `image_*` fields are guaranteed to be included. Data that has not been supplied may not be present at all, may be null or may contain the empty string (""). Please note that custom profile fields are not yet available through the API.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `user_not_found` | Value passed for `user` was invalid. |
| `user_not_visible` | The requested user is not visible to the calling user |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

