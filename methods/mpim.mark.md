This method moves the read cursor in a multiparty direct message channel.

## Arguments

This method has the URL `https://slack.com/api/mpim.mark` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `post`) |
| `channel` | `G1234567890` | Required | multiparty direct message channel to set reading cursor in. |
| `ts` | `1234567890.123456` | Required | Timestamp of the most recently seen message. |

## Response

```
{
    "ok": true
}
```

After making this call, the mark is saved to the database and broadcast via the message server to all open connections for the calling user.

Clients should try to avoid making this call too often. When needing to mark a read position, a client should set a timer before making the call. In this way, any further updates needed during the timeout will not generate extra calls (just one per channel). This is useful for when reading scroll-back history, or following a busy live channel. A timeout of 5 seconds is a good starting point. Be sure to flush these calls on shutdown/logout.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `invalid_timestamp` | Value passed for `timestamp` was invalid. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
