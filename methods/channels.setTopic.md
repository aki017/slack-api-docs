This method is used to change the topic of a channel. The calling user must be a member of the channel.

## Arguments

This method has the URL `https://slack.com/api/channels.setTopic` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `channels:write`) |
| `channel` | `C1234567890` | Required | Channel to set the topic of |
| `topic` | `My Topic` | Required | The new topic |

## Response

```
{
    "ok": true,
    "topic": "This is the new topic!"
}
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `not_in_channel` | Authenticated user is not in the channel. |
| `is_archived` | Channel has been archived. |
| `too_long` | Topic was longer than 250 characters. |
| `user_is_restricted` | This method cannot be called by a restricted user or single channel guest. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

