This method provides information about your team.

## Arguments

This method has the URL `https://slack.com/api/team.info` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `team:read`) |

## Response

```
{
     "ok": true,
     "team": {
         "id": "T12345",
         "name": "My Team",
         "domain": "example",
         "email_domain": "",
         "icon": {
             "image_34": "https:\/\/...",
             "image_44": "https:\/\/...",
             "image_68": "https:\/\/...",
             "image_88": "https:\/\/...",
             "image_102": "https:\/\/...",
             "image_132": "https:\/\/...",
             "image_default": true
         }
     }
 }
```

## Team Icon

If a team has not yet set a custom icon, the value of `team.icon.image_default` will be `true`.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

